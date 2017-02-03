
Table of Contents

    2.5D milling
            Toolchain 1: FreeCAD DXF output Tweaking with LibreCAD Heekscad FABtotum
            Toolchain 2: FreeCAD STEP export Heekscad import STEP/Convert Face to sketch FABtotum
            Toolchain 3: KiCAD DXF export LibreCAD import DXF and save as DXF 2007 Heekscad import DXF FABtotum
            Toolchain 4: Gerber Flatcam FABtotum

2.5D milling

Stampa

A 2.5D image is a simplified three-dimensional (x, y, z) surface representation that contains at most one depth (z) value for every point in the (x, y) plane.
Toolchain 1: FreeCAD DXF output Tweaking with LibreCAD Heekscad FABtotum

Actually you can supposely use whatever software you feel like using that can export a DXF. But my best results were by using the DXF exported by LibreCAD using the DXF in the 2007 version. LibreCAD also allows to move the cutting profile and in this way set the origin.

What I wanted to mill was a support material for milling, a MDF of 2 cm depth with the slots and holes of the milling plane of the FABtotum. This was it in FreeCAD:

This is the result imported in LibreCAD and allowing the origin to be centered in the MDF table:

I also removed the two bottom slots of the corners, because there is where I have my fixations during manufacturing and I did not want tool colisions.

I saved the result using the 2007 DXF version and imported that DXF into Heekscad (I used latest git version):

In that image you can see that I created a pocket. The mill I used for manufacturing is a 12 mm long 3 mm endmill. The values I used for the pocket are milling 10 mm (ends in -10 mm) in steps of 2 mm at 400 mm/min feedrate vertical and horizontal at 7000 rpm.

With this values you guessed it, I only milled half of my 20 mm MDF, and had to repeat the procedure from the other face. Why? Because I did not have a longer mill handy. Also take into account that even if my mill was 12 mm, I could only use the first 10 mm because otherwise the extruder would hit the MDF panel.

But if you generate that code, it won't work very well or at least it did not for me. Then I realised I had to tweak the gcode to be “Marlin/Fabtotum” compatible. At the end I opted for creating a definition of a new machine in Heekscad and a specific gcode post-processor. It is the first time I do something like this, so it may not work for everything… Here they are the files containing the machine definition and the post-processor:

/usr/share/heekscnc/machines.xml /usr/lib/heekscnc/nc/marlin.py

in here: fabtotum4heeks.zip (updated to 15 March, see GitHub for changelog)

The result is this (well I used it quite a lot before uploading this foto and it got “hurt” in the process):

ASCII User commentsASCII User comments

Of course, I simulated the gcode in openscam before uploading it to the FABtotum. However, I found out that sometimes arcs seem to be in the wrong direction in OpenSCAM, while the code as generated by Heekscad produces the right result.
Toolchain 2: FreeCAD STEP export Heekscad import STEP/Convert Face to sketch FABtotum

This is an alternative toolchain. After importing with Heekscad and converting a face to a sketch, the flow is exactly the same as above.
Toolchain 3: KiCAD DXF export LibreCAD import DXF and save as DXF 2007 Heekscad import DXF FABtotum

The intermediate step via LibreCAD is necessary as otherwise Heekscad imports the milimeters as if there were millis (10^-3 inches). After having imported the tracks in Heekscad, the result fo the sketch is marked in the properties as “Bad”. Change this to “re-order” and it will automatically switch to “multi”, allowing you to perform machining operations on the profile.
Toolchain 4: Gerber Flatcam FABtotum

Flatcam does not currently support successive passes at different depths. So you will have to make sure that you use a mill configuration that withstands the tool deflection during the milling or your tool will break. Not having sucesive passes at different depths puts a limitation on the smaller mill diameter you may use, or the type of tool (engraving V shaped vs endmill).
2d_milling_workflow.txt · Last modified: 2015/07/07 12:44 by jj