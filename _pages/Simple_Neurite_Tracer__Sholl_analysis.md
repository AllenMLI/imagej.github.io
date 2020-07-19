__NOTOC__
{{ambox
| text = This is an old tutorial on how to call the [[Sholl Analysis]] plugin from [[Simple Neurite Tracer]]. '''It is rather outdated. More up-to-date information is provided in [[SNT:_Analysis#Sholl_Analysis|SNT: Analysis]].''' For an overview of the technique refer to the [[Sholl Analysis]] documentation page.
}}

= Introduction =
This tutorial assumes that you've already traced an image with Simple Neurite Tracer (SNT) and that you are familiar with the [[Sholl_Analysis#MethodsTable|variants of Sholl methods]] and the [[Sholl Analysis]] plugin. This tutorial will use an olfactory projection fibre image, freely available from the [http://www.diademchallenge.org/olfactory_projection_fibers_readme.html Diadem challenge data set].

= Retrieving Profiles =
When you've loaded the traces, that should look something like this:
[[Image:Simple Neurite Tracer- Sholl analysis-1.jpg|500px|center|]]


Now you have to pick a centre point for the analysis. This might be the soma or a [http://forum.imagej.net/t/sholl-analysis-validation-parameters/3065/2 relevant focal point].  The centre point must be on one of your existing paths.  First, select the path on which your centre point lies:
[[Image:Simple Neurite Tracer- Sholl analysis-2.jpg|500px|center|]]


Now hold down {{key|Ctrl}}+{{key|Shift}} (on Windows or Linux) or {{key|Alt}}+{{key|Shift}} (on Mac) and move the mouse along the path.  A red cross-hair should track along the path:
[[Image:Simple Neurite Tracer- Sholl analysis-3.jpg|500px|center|]]


... when you've got the red cross-hairs at a suitable point, <i>still</i> holding down {{key|Ctrl}} / {{key|Alt}} and {{key|Shift}}, press the {{key|A}} key.  Then you can release the other keys.  You should see the Sholl analysis interface appear like this:
[[Image:Simple Neurite Tracer- Sholl analysis-4.jpg|500px|center|]]


Consider the first two options: you should probably select the top option ''Use all N paths in analysis?'' unless you're only wanting to include a subset of the paths, for example if your image stack contains multiple separate neurons. Next you can click on ''Plot Profile'' so that you can visualize how the intersections with concentric spheres vary with distance from your centre point:
[[Image:Simple Neurite Tracer- Sholl analysis-6.jpg|500px|center|]]


This graph shows exactly how many times a sphere of a particular radius will intersect with paths (i.e., ''continuos sampling''). To consider spheres of evenly spaced radii (see definition of [[Sholl_Analysis#StepSize|Step size]]), you have to enter a value into the ''Radius step size'' box. E.g.,
[[Image:Simple Neurite Tracer- Sholl analysis-7.jpg|500px|center|]]


= Data Normalization =
If you need to preview the effect of [[Sholl_Analysis#MethodsTable|normalizing]] the number of intersections by the volume (or area) enclosed by the sphere (or circle) - you can do that by selecting the ''Normalize for volume enclosed by circle'' option:
[[Image:Simple Neurite Tracer- Sholl analysis-8.jpg|500px|center|]]


The ''Use standard axes'' / ''Use semi-log axes'' / ''Use log-log axes'' controls whether the analysis is based on the log of normalized intersections and distance (''log-log''), the log of normalized intersections (''semi-log'') or unmodified values (linear axes) (see [[Sholl Analysis#Sholl Plots|Sholl Plots]] for details. Note that the [[Sholl_Analysis#ShollDecay|Regression coefficient]] is always calculated in real time even if no normalization options are chosen.

= Exporting Profiles =
You can export profiles by clicking on "Save Profile" which will prompt for a CSV filename to save to. If you want to export the profile, so that you can edit in some other software or include it in a presentation, you can select ''Export graph as SVG'' in the graph window.  You can then load the SVG file (e.g., in Inkscape):
<div style="width:800px">
[[Image:Simple Neurite Tracer- Sholl analysis-10.jpg|350px|left]][[Image:Simple Neurite Tracer- Sholl analysis-11.jpg|350px|right]]
</div>
{{Clear}}

= Analyzing Profiles =
Press ''Analyze Profile'' to run the [[Sholl Analysis]] plugin. Once [[Sholl_Analysis#Parameters|Parameters]] have been specified, the plugin will [[Sholl_Analysis#Dratio|automatically calculate]] the normalization method thought to be the most informative. Metrics will be displayed in a [[Sholl_Analysis#Metrics|detailed table]]. 
{{Tip
| tip = You can perform batch analysis using [[Sholl_Analysis#Analysis_of_Existing_Profiles|{{bc|color=white|Analysis|Sholl|Sholl Analysis (Existing Profiles)...}}]] or [[Sholl_Analysis#Analysis_of_Traced_Cells|{{bc|color=white|Analysis|Sholl|Sholl Analysis (Tracings)...}}]]
}}
= Sholl Image =
Another option that might be useful is ''Make Sholl image'', equivalent to the [[Sholl#Output_Options|Intersections mask]] created by the [[Sholl Analysis]] plugin when parsing images directly.  This will produce a stack which shows the number of intersections at each distance from the centre point on a colour scale.  You can see the exact number of intersections corresponding to a colour by mousing over that region and looking in the status bar.  For example, this shows you that the orange colour corresponds to 2 intersections ("value=2"):
[[Image:Simple Neurite Tracer- Sholl analysis-12.jpg|500px|center|]]


If you go back to the main tracer interface, keeping that ''Sholl image stack'' open, you can visualize those colours on the traces by switching the ''Use colors / labels from'' option to ''Sholl analysis of all paths'' (volume of original image made transparent for clarity):
[[Image:Simple Neurite Tracer- Sholl analysis-13.jpg|500px|center|]]


[[Category:Tutorials]]
