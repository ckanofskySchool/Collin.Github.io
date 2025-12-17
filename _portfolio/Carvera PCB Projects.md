---
title: Carvera PCB Projects
subtitle: Using MakeraCAM and the Carvera CNC machine
image: assets/img/portfolio/DailyJournal/SeeedFrontSoldered.jpg
alt: 

caption:
  title: Carvera PCB Projects
  subtitle: Using MakeraCAM and the Carvera CNC machine
  thumbnail: assets/img/portfolio/DailyJournal/SeeedFrontSoldered.jpg
---

# Using MakeraCAM for PCB Milling

## MakeraCAM thoughts

For my project, I used a pocket cut between the traces and the edge, and while that makes the end result very clean and have nothing but traces left, it is very time consuming to cut every single part except traces out. 

When I thought some more about this situation, I wondered if I could use a contour instead to mill just the area around the traces, and take way less time. However, this concept had an isssues, the 2D Contour Toolpath only creates one path around the traces which might not be enough and additionaly, only one mill bit could be selected, unlike the pocket where a larger and detail bit could both be selected.

One interesting solution to this I thought up was to have 2 contours, the first being a .8mm Corn bit contour which had a .1mm offset, meaning another countour cut with the .2mm detail bit ould take off just the edges achieving good space around each path while also having the nice detailed cut. I have yet to test this theory, but hope to do so.

## MakeraCAM Workflow - Collin and Aaron Creation

Before starting the CAM, make sure you have the MakeraCAM application installed, the .grb files for your custom PCB board(should have at least an F-Cu and EdgeCut file).

### CAM
MakeraCAM is the software we use to take a PCB design file and make toolpaths that the machine can follow to create the PCB.



1. Open up Makera CAM and start a new 3-axis project.
<img src="assets/img/portfolio/MakeraCAMworkflow/Step1.png" width="960" height="540">

2. Then we need to import the PCB files. These files are the .grb & .drl files.
<img src="assets/img/portfolio/MakeraCAMworkflow/Step2.png" width="960" height="540">

3. Files will then appear under WCS1
<img src="assets/img/portfolio/MakeraCAMworkflow/Step3.png" width="960" height="540">

4. Once all our files are in the software, they will likely not be in the correct position, so we need to move them to have the bottom left of the files 6mm offset in both the x and y axes. 
<img src="assets/img/portfolio/MakeraCAMworkflow/Step4.png" width="960" height="540">

5. Click and drag over the entire file, which should turn into dotted lines when selected. Then go to the transform tool, or use the keybind “m” to open the move menu. Finally, make sure the bottom left dot in the menu is selected and set both the x and y to 6mm.
<img src="assets/img/portfolio/MakeraCAMworkflow/Step5.png" width="960" height="540">

6. Now that the files are in place, it’s time to start telling the CNC machine how to cut each part, starting with the traces. The key to making toolpaths in MakeraCAM is to effectively manage which layers are hidden to click and drag over only what needs to be selected. For the traces, we want to hide all layers except the following: FileName-F_Cu.grb & FileName-Edge_Cuts.grb . You might notice that there are .grb_pad files, but we will never use these, so keep them hidden so as not to accidentally select them.
<img src="assets/img/portfolio/MakeraCAMworkflow/Step6.png" width="960" height="540">

7. When only the 2 main files, F_Cu and Edge_Cuts, are visible, click and drag over the entire area to select everything. Then, zoom in on one of the edges of the Edge_Cut file and deselect the outermost layer of the edge cut. Note that while it looks like a single line zoomed out, it actually is two offset lines.
<img src="assets/img/portfolio/MakeraCAMworkflow/Step7.png" width="960" height="540">

8. Once the selection is all correct, go up to the top menu and select the 2D paths icon, which looks like an arc with a horizontal tangent line. 
<img src="assets/img/portfolio/MakeraCAMworkflow/Step8.png" width="960" height="540">

9. When clicked, a dropdown should appear with a choice for 2D Pocket; select that option. Note that the selection from the previous step should still be active and selected. For the settings, set the End Depth to 0.05mm, Retract to 5mm(for faster cutting).
<img src="assets/img/portfolio/MakeraCAMworkflow/Step9.png" width="960" height="540">

10. Click on the button “Add Tool” and select the .8mm Corn, click select on the bottom right of that menu.
<img src="assets/img/portfolio/MakeraCAMworkflow/Step10.png" width="960" height="540">

11. Do the same for adding the .2mm 30* Engraving Bit(make sure PCB is selected below the Bit Selection menu.).
<img src="assets/img/portfolio/MakeraCAMworkflow/Step11.png" width="960" height="540">

12. Finally, click calculate at the bottom, and the toolpath should generate. If you would like to hide it until la11ter, it can be hidden using the left menu under the layers area.
<img src="assets/img/portfolio/MakeraCAMworkflow/Step12.png" width="960" height="540">

13. Once the PCB traces are cut, we need to add the holes if your design has any. If your file has no .drl files, then skip this step. If it does have .drl files, then we need to go into the 2D path icon dropdown and select 2D drilling.
<img src="assets/img/portfolio/MakeraCAMworkflow/Step13.png" width="960" height="540">

14. In the 2D drilling menu, click on the Choose Tool button and select the .8mm Corn mill bit, the same bit we used to cut the traces. Ensure that PCB is still highlighted in the bottom menu of the selection box when the .8mm corn is selected. 
<img src="assets/img/portfolio/MakeraCAMworkflow/Step14.png" width="960" height="540">

15. Once the tool is selected, set the depth of the drilling to 1.7mm, which is the depth of the PCB board. Then scroll to the bottom of the menu and click calculate. 
<img src="assets/img/portfolio/MakeraCAMworkflow/Step15.png" width="960" height="540">

16. Once the PCB holes are cut, we can then cut the board out of the big copper sheet by cutting along the Edge cut line. However, unlike with traces where we used a 2D pocket, we will instead use a 2D contour cut to only cut along the edge line of our file. Similarly to the traces, select the inside line of the Edge Cut file.
<img src="assets/img/portfolio/MakeraCAMworkflow/Step16.png" width="960" height="540">

17. Once selected, go to the 2D paths icon used before and select 2D Contour.
<img src="assets/img/portfolio/MakeraCAMworkflow/Step17.png" width="960" height="540">

18. Once selected, select the .8mm corn mill bit as the tool, and ensure PCB is still highlighted in the bottom menu of the selection box when the .8mm corn is selected. 
<img src="assets/img/portfolio/MakeraCAMworkflow/Step18.png" width="960" height="540">

19. Once the tool and vectors(edge cut lines) are selected, choose outside as the path of travel to account for the width of the .8mm bit.
<img src="assets/img/portfolio/MakeraCAMworkflow/Step19.png" width="960" height="540">

20. Finally, before we calculate this cut file, we need to make sure that the board won’t come loose mid-cut and potentially fly into the bit. We can do this by using Tabs, essentially small areas of the cut area, which we will skip over to keep the milled PCB board attached to the big copper plate. To add these tabs, scroll to the bottom of the 2D Contour menu and select the circle labeled custom under the Tabs header. Then, click on the button labeled “Add” and select where you want to add Tabs. Typically, you want at least 3 tabs distributed equally on the cut, but this can vary depending on the size and shape of the board.
<img src="assets/img/portfolio/MakeraCAMworkflow/Step20.png" width="960" height="540">

21. Once the tabs have been added, which is shown by a square with an X in it, you can finally click calculate, and the cut file is complete
<img src="assets/img/portfolio/MakeraCAMworkflow/Step21.png" width="960" height="540">

22. Yay, the board is complete! Now all that is left is to preview and export the toolpaths so that the CNC machine can cut them. To preview the toolpath before cutting, select the preview toolpath icon in the upper menu, which looks like a sheet of paper with a magnifier glass on it. 
<img src="assets/img/portfolio/MakeraCAMworkflow/Step22.png" width="960" height="540">

23. Select this icon and check the boxes for all the toolpaths shown.
<img src="assets/img/portfolio/MakeraCAMworkflow/Step23.png" width="960" height="540">

24. Click the play icon at the bottom to watch the cut preview. The speed of the preview can be adjusted using the slider under the play button, and the upper-right menu box labeled Preview Toolpaths changes what is seen in the preview.
<img src="assets/img/portfolio/MakeraCAMworkflow/Step24.png" width="960" height="540">

25. Click exit preview
<img src="assets/img/portfolio/MakeraCAMworkflow/Step25.png" width="960" height="540">

26. Export the file 
<img src="assets/img/portfolio/MakeraCAMworkflow/Step26.png" width="960" height="540">

27. Select all toolpaths
<img src="assets/img/portfolio/MakeraCAMworkflow/Step27.png" width="960" height="540">

28. Name file ![LastnameResistorgcode] and save file as a g-code
<img src="assets/img/portfolio/MakeraCAMworkflow/Step28.png" width="960" height="540">

### My Files for the Custom PCB Board

[PCB Board Design Files](assets/Files/Electrical/SeeedControlBoard.zip)
[PCB Board CAM files](assets/Files/Electrical/SeeedControlBoard-F_Cu.zip)

### Milled and Soldered Board

![Seeed with Headers Placed](assets/img/portfolio/DailyJournal/SeeedBackPinsPlaced.jpg)

![Seeed top headers soldered](assets/img/portfolio/DailyJournal/SeeedFrontPinsSoldered.jpg)

![Seeed Top Final Soldering](assets/img/portfolio/DailyJournal/SeeedFrontSoldered.jpg)

![Seeed Bottom Final Soldering](assets/img/portfolio/DailyJournal/SeeedBackSoldered.jpg)