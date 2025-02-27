# superpixels-segmentation-gui-opencv
## Superpixels segmentation algorithms with QT and OpenCV, with a nice GUI to manage labels and colorize the cells
### v2.3 - 2019-07-08

![Screenshot - Global](screenshots/screenshot.jpg?raw=true)
<br/>

## HISTORY

* v0: launch
* v1: added contours + several improvements
* v2: tabbed workflow + labels management + PSD and TIFF export + create cell
* v2.1: several bugs fixed, select labels from the viewport with "ALT"+click, show the "holes" in the mask + adapted to openCV 4.1
* v2.3: GrabCut algorithm in create cell mode + code cleanup
<br/>
<br/>

## LICENSE

The present code is under GPL v3 license, that means you can do almost whatever you want
with it!

I used bits of code from several sources, mainly from the openCV examples
<br/>
<br/>

## WHY?

I didn't find any simple tool (understand: GUI) to produce depth maps from my stereo pictures.

One solution to help produce depth maps is image segmentation: coloring zones with superpixel cells is so easy! In photoshop, it is then child play to apply gray gradients to colored areas...

This tool can also help separating an object or person from the background, for photomanipulation purposes.

I'm not an ace of C++ and QT, in fact I only started using them some month ago. So, if you don't find my code pretty never mind, because it WORKS, and that's all I'm asking of it :)
<br/>
<br/>

## WITH WHAT?

Developed using:
* Linux Ubuntu	16.04
* QT Creator 3.5
* Requires these libraries:
  * QT 5
  * openCV 4.1 compiled with openCV-contribs - should work with 3.x versions without much editing
  * Image Magick 7

This software should also work under Microsoft Windows: if you tried it successfully please contact me, I'd like to offer compiled Windows executables
<br/>
<br/>

## HOW?

* Help is available in the GUI, with each element's tooltip
* This tool is composed of 3 tabs
* A pen tablet is higly recommended

### IMAGE TAB

![Screenshot - Image tab](screenshots/screenshot-image.jpg?raw=true)

* To begin, load an image, nothing is possible without it! The image appears in the main viewport and its thumbnail is shown on the left side

* You can then apply filters and effects on the image:
  * tick the chosen actions
  * press APPLY
  * some filters can take a long time to compute, especially the noise filter (which is highly recommended by the way)
  * filters are always applied to the ORIGINAL image, you can't restart from a previously altered image
  * click on the ORIGINAL button to revert to the loaded image file
  
* You can come back when you want to the Image tab and re-apply filters

* Be aware that when you load a new image, if you computed some segmentation and/or worked on labels and cells, all this work will be lost if you don't save the session! More about this later
  
### SEGMENTATION TAB

![Screenshot - Segmentation tab](screenshots/screenshot-segmentation.jpg?raw=true)

* First, select the algorithm you want to use, and tune the parameters

* Available superpixels algorithms:
  * SLIC (Simple Linear Iterative Clustering) clusters pixels using pixel channels and image plane space to efficiently generate compact, nearly uniform superpixels
  * SLICO (Zero parameter SLIC) optimizes SLIC, using adaptive compactness factor
  * MSLIC (Manifold SLIC) optimizes SLIC using manifold methods resulting in more content-sensitive superpixels
  * LSC (Linear Spectral Clustering) produces compact and uniform superpixels with low computational costs, and keeps global images properties
  * SEEDS (Superpixels Extracted via Energy-Driven Sampling) uses an efficient hill-climbing algorithm to optimize the superpixels' energy function that is based on color histograms and a boundary term, producing smooth boundaries
  
* Push the COMPUTE button, wait a little

* The result is displayed:
  * the number of defined cells is displayed next to the COMPUTE button
  * you now have on your right 3 layers that you can activate / deactivate / blend in the viewport :
    * the processed image
    * the (for the moment) empty colored mask
    * the superpixels grid (superpixel cells, this is what you just computed)
  
* You can save and retrieve the segmentation parameters (XML openCV format) - the filters and effects parameters from the Image tab will also be saved

### LABELS TAB

![Screenshot - Labels tab](screenshots/screenshot-labels.jpg?raw=true)

This tab has two functions: colorize the cells and manage the labels

Basic operations:
  * on your left, you can find the default "rename me!" label:
  * double-click on it to change its name
  * now choose a color, that you can pick from the default palette buttons, or with the more sophisticated color-picker just over them
  * the selected label's color appears on the label indicator (the one with the pen, just above the color palette). The indicator also shows the selected label's name

* Manage the labels:
  * you can add a label: rename it and choose its color, then begin to fill in the cells for that label
  * you can delete a label: it will also delete the corresponding color in the mask
  * select several labels and click the JOIN button: the result is only one label with all the cells the same color. Rename the label and change its color as it pleases you
  * select one label and click on the eye button above it: its color is hidden in the mask. Select a color and it will be back
  
* It is time to colorize the cells:
  * left mouse button to colorize with the the current color - you can hold down the mouse button and move it to set several cells
  * right mouse button to unset a cell (you don't have to select the label it belongs to)
  * "CTRL" key + left mouse click to floodfill a closed area
  * "CTRL" key + right mouse click to floodfill to transparent a whole contiguous cells area previously colorized
  * "ALT" key + left mouse button to select a label directly in the viewport, this will select it in the list
  * UNDO: use this button to cancel the last action (only the last action can be cancelled)
  * click on the "Holes" button to view every cell that hasn't been colored yet. Click again on it to hide this feature

* Change the view:
  * you can choose the transparency of the image, mask and grid with the sliders on the right
  * you can change the grid color with the control under the grid layer button
  * use the zoom controls on the right to zoom in / out - you can also use the mouse wheel over the viewport
  * click on the thumbnail to position the zoomed area in the viewport
  * hold the middle mouse button on the viewport, or hold down the "SPACE" key to move the view position with your mouse when the image is zoomed in - you can also use the scrollbars
  
* Modifiy the cells:
  * the superpixel algorithms sometimes misses important details: you can define your own cells with the special "Create new cell" button
  * the editor enters a special state: you have to draw your cell in white color
  * you can set pixels with the left mouse button, and unset them with the right. You can choose the thickness in the right panel
  * use the "CTRL" key to floodfill entire zones, just like explained before
  * once a pixel is set, it can become the origin of a line:
    * hold down the "X" key and move the mouse over the viewport: a temporary line appears
    * release "X" and the line is set
    * the end of a line becomes the new origin from which you can draw another one
    * click with the left mouse button to set a pixel to change once again the origin of the lines
  * you can only UNDO the last action (as usual)
  * the GrabCut algorithm can help you define a zone:
    * it can automatically cut out an object/person: choose what you don't want with the red "reject" color, define the inside of the chosen zone with the green "keep" color (or white "mask"), and fill with blue in the intermediate zone ("maybe")
    * click the "GrabCut" button, and after a while a white zone is defined, with a blue zone outside of it. Add or erase blue where it is interesting, fill the blue zone with the white "mask" color and you're done
    * you can GrabCut any number of times to refine 
  * when you are done with drawing:
    * keep in mind that the only color that will be accounted for is the white "mask", any other color (reject, keep, maybe) will be ignored
    * click on the "create new cell" button: you can choose to define the new cell or cancel
    * if effectively drawn:
      * it is not possible to get back to the previous state, the UNDO button will not work. Save your work before using this function!
      * the new cell boundaries appear in the grid mask
      * it is filled with purple color, in the mask layer (I like purple, eh)
      * the new label contains this cell: you can now join it to another (or leave this label as is). Be sure to rename it
      * now you can set and unset this new cell
 
* Pixel hunting:
  * Even if you paid attention in filling all the zones, sometimes pixels are lost, especialy when you draw new cells. No pixel should be left uncolored
  * Use the Holes button to make missing pixels appear in white
  * The pixels count is also displayed under this button. You are finished when the count is 0
  * If your image is big, depending on the level of zoom, you'll be unsuccessfuly chasing pixels. That's when the H key becomes necessary: hold down the H key on your keyboard, and the lost pixels will flash in color, and change size temporarily, as long as you hold the H key down. It also works with high zoom out levels, this time

* Load and save:
  * you can save the current session with a base name (several files are saved)
  * no need to recompute when you retrieve a session: just load the image first, then the session XML file
  * what is saved in the session:
    * processed image (PNG image)
    * mask (PNG image)
    * grid (PNG image)
    * cells / labels (XML openCV format) 
  * you can export the processed image and the labels to PSD or TIFF image formats:
    * in a PSD Photoshop image, the processed image is the background, and the labels are transparent separate layers
    * in a TIFF image, each image and label has its own page. Be careful, many software only open the first page
    * the PSD and TIFF images work well with an open-source program: The Gimp
    * the PSD files cannot be loaded with Photoshop 7. They work with PHotoshop CS6, but I don't know if it is the case with Photoshop CS to CS5 (let me know if you tried)
   
<br/>
<br/>

## Enjoy!

### AbsurdePhoton
My photographer website ''Photongénique'': www.absurdephoton.fr


