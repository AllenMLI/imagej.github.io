---
mediawiki: Series_Labeler
title: Series Labeler
project: /software/fiji
categories: [Image Annotation]
artifact: sc.fiji:Series_Labeler
---

This plugin is inspired by the [Time Stamper](/plugins/time-stamper) plugins from ImageJ and from Tony Collins' [MBF Plugin Collection](/software/mbf-imagej), as well as the imageJ built-in {% include bc path='Image|Stack|Label'%} command.

It aims to combine the functionality of these plugins and refine and enhance the functionality for instance by adding the preview functionality suggested by Michael Weber. Series Labeler is not a drop in replacement for Time Stamper, or Label... since it does not work with hyperstacks or allow macro recording.

## Screen Shot of features

![](/media/plugins/series-labeler-screen-shot.png)

## What it works with

Series Labeler handles any kind of stack: time (t), z (3rd spatial dimension) or channel (c, eg. spectral series). It does not work with hyperstacks, see below.

## It reads in image metadata for default settings

When meta data is available in the image, ie. for calibration of time interval, z spacing, etc. it is read into the GUI as default values, so you have a better chance of getting the values and units correct!

## How to use Series Labeler

Generally speaking, just work your way down the GUI, with preview on if you like.

-   Open an image stack. It can be a z, time or channel stack, but not a hyper stack.

Make sure its the active image by clicking on it, then....

-   Open the Series Labeler (its in Image-Stacks-Series Labeler)
-   Choose the type of stack in the General Settings - Stack Type
-   Choose the type of units and how to format them in the Units\_Formatting panel.
    -   Choose the label unit from the drop down list or choose your own custom one.
        -   Custom labels can be anything you like! If you don't see the one you want in the drop down list, just add it here.
    -   "Time series / movie" can have Decimal (0.45 sec) or Digital (00:00:00) formats, as well as a custom format the you can type in the given text field.
        -   Custom formats follow patterns as explained at Java SimpleDateFormat [Java SimpleDateFormat](http://download.oracle.com/docs/cd/E17476_01/javase/1.4.2/docs/api/java/text/SimpleDateFormat.html). H is hours, m is minutes, s is seconds, and S is ms.Try custom formats like HH:mm:ss or ss.SSS etc.
    -   "Z-stack" can have length units or a custom format.
    -   "Spectral" can have units of length or inverse length, or a custom format.
-   Give the Startup value, the Interval value (time/distance/etc between slices/frames), which frames/slices to work on (from First to Last), and how often to label a slice/frame using the Every n-th option. Every n-th of 1 labels every slice, 2 labels every other, 3 every third, etc.
-   In the location & Font pane, choose the location in x and y (Custom location) or choose a location preset for eg Top Left, and the font characteristics.
    -   If you drew a rectangle RIO in the image before launching Series Labeler, it will try to fit the label into that ROI.
    -   To change the font, font size, Series Labeler uses the ImageJ foreground/background color and Fonts tools. Click on the buttons to launch those tools, or double click the Text Tool (A icon) or Color Picker tool (dropper icon) in the Main Fiji/ImageJ tool bar.
    -   Turn the background on and off with the checkbook. The background uses the color set in the imageJ built in Color Picker (dropper icon), which is also launched by the Font Color button here.
-   With preview on, you can move the slider in the image to be labeled, and get a preview of what the label/stamp will look like through the stamp. Changes you make in the Series Labeler GUI should be sent to the preview you you can see what you are going to get.
-   Click OK to burn the labels/stamps into the image stack (maybe work on a duplicate stack - there is no undo!). The cancel button removes the label preview and gives you back the original stack, and closes the Series Labeler. The help button sends you here.

## It doesn't work with hyperstacks or macros...

Hyperstacks and macro recording are not supported yet.

### why not?

Hyperstacks can be labeled with the imageJ builtin Label command, if you want to do that. Series Labeler doesn't work with them, as there are many tricky cases to include when you have all the extra gadgets you have in this plugin. What if you want to label time, z and channel all at the same time? Hmmmm...

Macro recording is broken due to the use of a non blocking (non modal) GUI... which is needed for the preview functionality and being able to use built in imageJ font and color choosers. To fix this we need to make some new infrastructure in ImageJ/Fiji.... or wait for imageJ2 n-dimensional data support.
