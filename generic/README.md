Import dataset from Metamorph NX
================================

Necessity
---------
It can be useful to import Metamorph NX data into Metamorph 7.
Metamorph 7 imports all data as stacks, and multidimensional data from
NX gets imported as a single stack with the channel images in a single
stack instead of separate stacks.

Splitting the stacks manually would require:

1.  Open the Experiment info.dataset file
2.  Run "Stack" > "Keep Planes"
3.  Set the "High Range" to size of the full stack
4.  Set Action to "Copy Selected"
5.  Set "Step Size" to the number of channels in the dataset
6.  Set the "Low Range" to the channel number being split
7.  Click "Select Planes in Range"
8.  Run "Apply" for each channel.  This creates the new stack
9.  Click "Clear All" and repeat from step 6 for all channels

Solution
--------
"Keep Planes" cannot be overwritten for all the values we need, so
each plane traversed using the image properties and copied into a new
stack.

Installation
------------
Add the buttons `split_channels_from_nx_dataset.JNL` to the journal
taskbar

User interaction
----------------
Button: `Split Channels`

> 1.  Enter number of channels in dataset
> 2.  Click "OK"
<!-- content below automatically generated by doc_jnl.py -->
Source Code
-----------
drop_off.JNL:
```python
'''
Drop Off measurement of Confocal emission.
'''

# get total of region 1
Select_Region(m	1 5 9 0 1 -1 -1 8 Untitled ,
              1 1)
Configure_Show_Region_Statistics_Log(16 O               )
Show_Region_Statistics(m	1 5 9 0 1 -1 -1 8 Untitled ,
                       FALSE,
                       FALSE,
                       0)
Assign_Variable(66,
                20 0 0 0 199 0 0 0 153 0 0 0 14 0 0 0 32 0 0 0 67 101 110 116 101 114 65 118 101 114 97 103 101 0 83 104 111 119 82 101 103 105 111 110 83 116 97 116 105 115 116 105 99 115 46 73 110 116 101 103 114 97 116 101 100 0 )
# get average of N-1 regions
# initialize total to 0
Assign_Variable(34,
                20 0 0 0 199 0 0 0 153 0 0 0 12 0 0 0 2 0 0 0 67 111 114 110 101 114 84 111 116 97 108 0 48 0 )
# go through all of the rest of the regions
for RegionNum in range(2, Image.NumRegions+1, 1):,
        Select_Region(m	1 5 9 0 1 -1 -1 8 Untitled ,
              11 %RegionNum%)
    Show_Region_Statistics(m	1 5 9 0 1 -1 -1 8 Untitled ,
                       FALSE,
                       FALSE,
                       0)
    # sum up the integrated intensity of the regions
    Assign_Variable(78,
                20 0 0 0 199 0 0 0 153 0 0 0 12 0 0 0 46 0 0 0 67 111 114 110 101 114 84 111 116 97 108 0 67 111 114 110 101 114 84 111 116 97 108 32 43 32 83 104 111 119 82 101 103 105 111 110 83 116 97 116 105 115 116 105 99 115 46 73 110 116 101 103 114 97 116 101 100 0 )

# Calculate average
Assign_Variable(71,
                20 0 0 0 199 0 0 0 153 0 0 0 14 0 0 0 37 0 0 0 67 111 114 110 101 114 65 118 101 114 97 103 101 0 67 111 114 110 101 114 84 111 116 97 108 32 47 32 40 73 109 97 103 101 46 78 117 109 82 101 103 105 111 110 115 32 45 32 49 41 0 )
# Calculate (I1 - In-1) / I1
Assign_Variable(89,
                20 0 0 0 199 0 0 0 153 0 0 0 15 0 0 0 54 0 0 0 68 114 111 112 79 102 102 80 101 114 99 101 110 116 0 40 67 101 110 116 101 114 65 118 101 114 97 103 101 32 45 32 67 111 114 110 101 114 65 118 101 114 97 103 101 41 32 47 32 67 101 110 116 101 114 65 118 101 114 97 103 101 32 42 32 49 48 48 0 )
Trace(DropOffPercent)
```

split_channels_from_nx_dataset.JNL:
```python
'''
Split NX Dataset into channel stacks
'''
# Author: Bruce Gonzaga

Enter_Variable(8 channels,
               3,
               18 Number of Channels,
               TRUE,
               2,
               10,
               1,
               13 # of Channels,
               0 ,
               FALSE,
               -1,
               -1)
planes = Image.NumPlanes
# Autoscale is off by default on opened dataset
Scale_Image(m	1 5 10 0 1 -1 -1 8 Untitled ,
            1,
            TRUE,
            0,
            65535,
            0,
            0,
            TRUE,
            0,
            65535,
            0,
            0,
            TRUE,
            0,
            65535,
            0,
            0,
            TRUE,
            0,
            65535,
            0,
            0,
            FALSE)

for current_channel in range(1, channels+1, 1):,
        Trace("Current Channel: " + str(current_channel))
    channel_name = "Channel " + str(current_channel)
    
    for current_plane in range(current_channel, planes+1, channels):,
        Select_Image(m	1 5 9 0 1 -1 -1 8 Untitled ,
             0 )
    Trace("Current Image: " + str(current_plane))
    Image.ActivePlane = current_plane
    Add_Plane(m	1 5 9 0 1 -1 -1 8 Untitled ,
          m	0 6 14 16 1 -1 -1 14 %channel_name% ,
          FALSE,
          9)

    Select_Image(m	1 7 9 0 1 4 0 8 Untitled ,
             0 )
    Image.ActivePlane = Image.NumPlanes / 2
    Scale_Image(m	1 3 10 0 1 -1 -1 8 Untitled ,
            1,
            TRUE,
            0,
            65535,
            0,
            0,
            TRUE,
            0,
            65535,
            0,
            0,
            TRUE,
            0,
            65535,
            0,
            0,
            TRUE,
            0,
            65535,
            0,
            0,
            FALSE)

Select_Image(m	1 5 9 0 1 -1 -1 8 Untitled ,
             0 )
Image.ActivePlane = Image.NumPlanes / 2
```
