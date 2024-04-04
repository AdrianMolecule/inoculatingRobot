This repository is just for documentation. 
The software is stored at https://github.com/AdrianMolecule/labRobot
The movies are:
Hi Res Cellular Leaf: https://drive.google.com/file/d/1BTx-P7F2dU0esMkg9szmRZTBShIUq02r/view?usp=drive_link
Cellular Mammoth: https://drive.google.com/file/d/1AFmfaFm3UuAPwcy_cuPo2LcFZmnuJ3OE/view?usp=drive_link
Cellular Leaf: https://drive.google.com/file/d/18wSzhuMMJFmsK8t1lpt_-yD1knkCLvlB/view?usp=drive_link


Please read carefully the info below. While the project is straightforward, this will make your experience more pleasant.
The models folder holds the models for the head parts and the bed parts.
The base folder holds most images.
The results folder holds pictures of inoculated Petri dishes.

In principle you will 3d print and either CNC or laser cut a couple of boards. Then you will install the two loop holder parts and the two bed parts on the CNC machine. After that, you place the labware including the one you 3d print and  execute the labRobot software that produce a gcodefile. You will load that in a software that is used for the machine (Candle) and that's it.
I added a video of the machine in action and all the files including modifiable moldel files which you will need Only if you want to customize things. Fusion360 still has a free limited license.
There is no explicit BOM: just 8 screws and nuts (4 for attaching the bed overlay and 4 for the loop holder) and a 3018 CNC machine.
Watch first the video to get the overall picture. Then, look at the picturesfor more detail. Then print the 2 head parts and cut the 2 bed parts.

You will need several software apps: 
-The one that is needed for the CNC which is probably Candle. Test the CNC machine independently first. 
-3D printing software like Cura
-Software to do the laser cutting, like Adobe PDF and Inkscape only if you want to make modifications OR
-OR CNC software like FUsion360 if you want to cutyour bed parts Only if you do modifications
- Pyton library for https://docs.pylabrobot.org. Install with pip as per instructions 
- my code LabRobot  https://github.com/AdrianMolecule/labRobot. Just copy the directory called src so you can modify the sample programs.
- some Python editor like VSC, Eclipse etc

Details

All screws are 3 mm except for the metal bed fastening ones that are 5 mm ( the four large ones)
There are 2 parts/layers that go on teh metal bed. The bottom layer and the top layer which is the frame layer where labware modules go.

I provided the original Fusion360 files so you can change the parameters (from Design mode/Modify/change parameters. The last several operations in the design history sometimes contain a cutting rectangle to print just a small section of the final product to test for fit. They can be disabled by moving left in history (bottom) several steps until you see the full body.
All CNC cutting is done with 2 mm 2 flute end mill. The cheap ones work fine.
In case the bed is not perfectly flat and you use wood you can do a facing operation to make it parallel. That should  work with plastic too.
If you want to be cheap and fast you can use 3.175 mm mdf or plywood. Make sure it's flat. You can use HDPE plastic or ABS. In my case I used a thick 7 mm MDF base with a thick 8ish mm frame because I had them like that.  Neither thicknesses are important but you will have to adjust the cutting depth for your cut thickness or use a spoil board.
For the bed partsif you want to lasr cut, you can use acrylic, either 1/8 or 1/4 inch for the base and 1/8 for the frame.
There are extra holes in case you want to use more screws to attach the labware like Petri dish holder to the bed.
The frame and the base are attached to the aluminum machine bed with the 4 thick screws. I had only small t-nut metal sliders so I printed an adapter so they don't come out loose. You can use directly the appropriate size t-nuts that slide in your bed. Some should come with the machine. I use a cheap 3018 clone that is similar to https://www.sainsmart.com/products/sainsmart-genmitsu-cnc-router-3018-pro-diy-kit
I printed in ABS as I always do,  PLA or something more advance should work too. The claws on the Petri dish holder are easy to break under larger force. A super fast acetone dunk of the holder or a vapour treatment (Google acetone vapors ABS) - will make them stronger. Breaking the claws will not affect functionality as long as you still have some left.
After you print the plastic pieces you have to remove the spindle and push in the printed head attachment, it's a tight fit. After that, connect the loop holder with the 2 screws. You can use regular nuts, not the long cylinder ones like I did. The hole for the inoculating loop is intentionally left large to accommodate any size. The model will print two additional plastic shims to bridge that gap if necessary. You can use tape around the loop handle if you need it but the 2 screws that tighten the loop handle should be enough.
The slots are same size as Opentrons's slots (slotSizeX=127.76; slotSizeY=85.48 ) and the plate is also incuded in the Opentrons library. See comment and urls in the software. I used an OS compatibility wrapper called pyLabRobot that is intended to allow you to execute the same code on several robots like Hamilton, Opentrons etc.
    # https://docs.pylabrobot.org

My code is only several files, all under labRobot in GitHub. Just copy them in a local directory, make sure pylabrobot is installed and start. Code places where you need (media  z distance) or might want to make adjustments are tagged with "#change here, so do a search for that string in the code.
The start point file is start.py. There are unused files that might be useful so only the files referred from start.py are mandatory.
The generated gcode file is called gcode.txt and that is what you load in Candle.

As per industrial robots you need a general calibration and sometimes an experiment specific calibration.
If you don't have limits on your machine, which is what happens to mine you need to set zeroes manually. Use the Candle or other software and move the robot until the top of the loop barely touches the bed and set Z =0. Then lift and bring the tip in the lower left corner of the lower left slot and hit X=0 Y=0.

After that it would be good to check a simple program that goes to a well in the plate, let's say H12. Check that it goes there properly and readjust the XY if necessary.

The loop interior diam is < 1.5 mm. It transferred about 20% of a 360ul well into about 200 points so about .3 ul per drop. That is probably better than a regular pipette for this purpose because it's good to have small volumes.

If you don't have a 96 well plate getone on Amazon or ask me to design some simple source container.

Similar to the industrial robots you need to 'calibrate  the height of the media in the agar plate". Put the plate with agar in and lower the loop until it barely touches the top of the LB.Agar media. Memorize the z value and assign it to calibrationMediaHeight in start.py.


It's good to test with something simple as a big plus sign that put a drop in the middle of the plate and 4 more around. Just un-comment the call to drawBibPlusSign in start.py.

In order to 'draw/inoculate something' you need a file with the dots saved in a x, y 'Python format".
The subdirectory called /images contain some code like imageToDotsArrayNoFiles.py to take a file and produce an outline and an array of dots. You can change the block size to get dots within the -40 and +40 for the dots. The dots are relative to the center of the Petri dish which is about  -  diameter = 84.8 mm interior diameter (see adUtil.py/createPetri... if you want to change it which should not be necessary. You don't need that directory if you want to produce the array using other means. You cen use the existing samples to start.

It's OK to stab the agar or just touch the surface. For art a stab will create crispier points because the liquid stays in the small hole. You will see the liquid held in the eye of the loop sucked in the media when the loop touches the surface. 
To my surprise, I managed to achieve perfect touchdowns from the first try. It's possible the agar surface is not horizontal so it might not touchdown in some cases. Don't worry. Change the calibrationMediaHeight with a lower value and re-run the program over the same plate.
I used  a super folded GFP in the MarpleLeaf.
My first try was with about 150 points and it lasted about 1 hour. If you change the value of safe_z to a lower value and the allocation of the source well to a closer one you should be able to cut that down to 30 min.
I 'flame' my loop directly in the machine. If you don't have a loop check out our web at specyal.com on how to make one based on another article by this great guy Alex.
The CNC machine is "as it was in the box". I payed about 200 US$ for it. There are kits to extend x and y and maybe z. The system would run on a different machine but some elements will probably need redesign. I might add some auto zero limit switches.
The precision is very good, probably around .3 mm mostly due to play as the internal precision is much better.
The Opentrons's bays are 1 2 and 3 on the bottom row and 4,5,6 on next one etc. That is why my top left slot is 4 not 3 as it would normally be. The simulator will show you where to put labware and the picture with the points you will get. The zoom is not updating the picture at this point but that should not be an issue.
All works well and it took several months of work to get it where it is. It was done while juggling several things including TA-ing on the HTGAA.org
Should the community like it, I will continue adding features. My main interest is molecular biology so I consider this as enablement.

I hope to add a microscope to it. Most likely based on OpenFlexure.
The documentation for the microscope part of the project can be found at https://openflexure.org/projects/microscope/build

Other possible usages:
Besides inoculation, the robot can be used to create points on paper for cheap sensors using cell-free lysates or other methods.
Maybe also for Antibiotic Sussceptibility Tests (AST).
It could also be used to create paper shipping dots.
