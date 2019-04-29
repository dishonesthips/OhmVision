# OhmVision
A python script using opencv and haar cascades to identify and calculate the values of resistors, live, from a webcam feed.

## How it works

### Haar Cascade
Using a Haar Cascade, an object classifier trained, in this case, to detect features belonging to resistors, the webcam image is scanned for resistors.

### Adaptive Threshold
A digital zoom is applied to each area in which a resistor was detected. The subimage is modified with a bilateral filter. An adaptive threshold is then applied which filters out the background as well as the body of the resistor itself (this minimizes the effect of the colour of the resistor). What's left from the threshold are the areas of large contrast: the coloured bands and the edges between the resistor and the background.  

![Screenshot](https://raw.githubusercontent.com/dishonesthips/OhmVision/master/images/thresh%2020.jpg)<br />
Above is an example of an adaptive threshold. See below for corresponding colour detection.

### Detecting the Colours
After converting to the HSV colour space, the bilaterally filtered image is scanned for pre-defined colour ranges. These ranges may need to be tweaked depending on the white-balance of the camera in use. A mask is created for each colour that is ANDed with the adaptive threshold. The resulting mask is then filtered based on size and shape constraints to determine if the validity of this colour band. The order of the detected bands and thus the value of the resistor are then calculated. The tolerance band should be placed on the right for a 'forwards' reading. 

![Screenshot](https://raw.githubusercontent.com/dishonesthips/OhmVision/master/images/full%20contour%2020.jpg)<br />

![Screenshot](https://raw.githubusercontent.com/dishonesthips/OhmVision/master/images/full.jpg)<br />
Above is the final result.

## Limitations
Cameras have a hard time picking up the reflective bands. Depending on the lighting, the bands may look white or even the colour of nearby objects. It is for this reason that the tolerance is not calculated.

![Screenshot](https://raw.githubusercontent.com/dishonesthips/OhmVision/master/images/error.jpg)<br />
A resistor will be highlighted red if too many or too few colours are detected. This indicates that either the pre-defined colour ranges  or the camera white balance needs to be tweaked.
