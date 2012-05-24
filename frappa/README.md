Synchronization for FRAPPA
==========================

Backport FRAPPA systems
-----------------------
<img src="https://github.com/downloads/omsai/journals/ringing_bleach.PNG"
 alt="ringing artifact" title="40us dwell time, confocal image" align="right" />

Metamorph's asynchronous acquisition projects the vibration "ringing" artifacts
from the motorized microscope epi laser cube onto the bleach region.  If the
laser dwell time is long enough, instead of ringing only thin streaks will
appear above and below one edge of the bleach region.

### Solution
Use `_delay_for_illumination.jnl` to insert a short delay for each FRAPPA channel
to surpress this "ringing".  Add it to the " Run journal when toggling active shutter"
of each FRAPPA illumination.

Multi-camera
------------
To create a "Coordinate system setting" (i.e. bleach calibration) for each camera,
an objective magnification needs to be created of the same objective named
specifically for the camera.  So a microscope with a single "100x Apo TIRF"
objective would need to be replaced by magnifications e.g. "100x left camera" and
"100x right camera"

Unfortunately having multiple calibrations for same objective means Metamorph
will no longer change it's Magnification dropdown menu to match the automated
microscope's position.  This requires you to often change both, the "Coordinate 
system setting" in the Targeted Illumination window as well as the Magnification
dropdown!

### Solution
Set the Magnification and then set the Coordinate system to the Manification
as follows:

1.  Detect the objective in use with `_new_var_objective.jnl` by linking this
    journal to your camera selection button.
2.  Have said camera selection button update the Magnification dropdown
    by updating the builtin `Device.Magnification.Setting`
3.  Finally `_mda_pulse.jnl` action in the protocol will be able to override the
    "Coordinate system setting" with Device.Magnification.Setting