Micropoint
==========
Metamorph's bleaches a single point at the center of the target 
Region of Interest (ROI).
To bleach an area larger than a single point over a region, one has
to segment the polygon or line into smaller regions.

1.  `CreateSegmentROIs_deleteROI1.jnl` bleaches straight lines

2.  `CreateROIsForLaserApp.jnl` bleaches irregularly shaped objects


Setup
-----
Enter the administrator and enable the `gridbin` Drop-in.
Make sure the `Legacy` category is checked, located under
the list of Drop-ins.  Then in Metamorph you will then be able to
see the drop-in listed under Display > Graphics > Boxes


Configure segmented ROI size
----------------------------
Edit the journal statement `Boxes on Binary Image` to set Box Width
and Box Height


(Irregular ROI only) Exclude small "boundary" ROIs
---------------------------------------------------
Modify the Integrated Morphometry Analysis (IMA) state file to
exclude objects of a certain size.

When using irregular regions, small segmented regions can be
generated towards the edges of the targeted ROI.  This has the
effect of making the pulse energy more dense, so for even bleaching
you may want to delete these or exclude them from being created
by the IMA state file.