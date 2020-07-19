{{Infobox
| software               = ImageJ
| name                   = Auto Crop
| maintainer             = {{Person|Schindelin}}
| author                 = Johannes Schindelin
| source                 = {{GitHub|org=fiji|repo=Fiji_Plugins|source=fiji/selection/Select_Bounding_Box.java}}
| released               = 40/02/2010
| latest version         = 30/04/2011
| status                 = stable
| category               = [[:Category:Plugins]]
}}

This plugin can find the smallest bounding box of an image (or a rectangular part defined by a selection) by cropping as much background as possible.

In the version labeled with ''(guess background color)'', the plugin tries to determine the background color by looking at the pixels on the border of the image, while ''Auto Crop'' uses the currently selected background color which can be changed by double-clicking on the pipette symbol or using the {{bc | Image | Color | Color Picker...}}.

Instead of cropping right away, you can set the selection to the rectangle that would be cropped to using {{bc | Edit | Selection | Select Bounding Box}}.

[[Category:Tutorials]]
[[Category:Plugins]]
