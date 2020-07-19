{{ComponentStats:sc.fiji:3D_Viewer}}This plugin offers hardware-accelerated visualization possibilities for image stacks, using the [[Java 3D]] library. Stacks can be displayed as texture-based volume renderings, surfaces or orthoslices. 

[[Image:3D_Viewer_overview.png|center]]

== Screencasts ==
Here you are a demo screencast separated into two different videos (~15 min in total) showing many of the features of the 3D viewer:

{{#widget:YouTube|id=cD3Hc3NYkaU|width=420}}  {{#widget:YouTube|id=GqG_RcK3kYg|width=420}}

Beyond this, a lot more screencasts can be found [[3D_Viewer:_Screencasts|here]], covering the following topics:

* Display stacks
* Rendering modes and attributes
* Adjusting the transfer functions
* Editing volumes
* Point lists
* Landmark-based registration
* Transformations
* 3D Content in PDFs

== For users ==
Comprehensive usage guidelines in form of FAQs and tutorials can be found [[3D_Viewer:_User_FAQs|here]].

== For developers ==

A lot of functions of the 3D Viewer are macro-recordable. However, if that is not enough (or if the function is not recorded properly), it is better to [[Introduction into Developing Plugins|write a plugin]]. In the latter case no macros should be called from Java, as that would limit the code to work with the currently active 3D Viewer (even if the user clicked somewhere else).

This code snippet should get you started:

<source lang="java">
Image3DUniverse univ = new Image3DUniverse();
univ.show();
univ.addMesh(yourImagePlus, null, "somename", 50, new boolean[] {true, true, true}, 2);
...
</source>

Full documentation for developers with tutorials and explained code snippets can be found [[3D_Viewer:_Developer_Documentation|here]].

=== Javadocs === 

The Fiji Javadocs provide detailed information about the {{Javadoc | package = ij3d | class = package-summary | label = 3D Viewer API}}.

== FAQ ==

=== The 3D Viewer opens a window saying ''An unexpected exception occurred.'' ===

If in the same window, it also says:

 java.lang.NullPointerException:Canvas3D: null GraphicsConfiguration

the reason is most likely that your graphics setup does not have any hardware 3D acceleration. This can happen e.g. when you run ImageJ via a remote X11 connection (3D acceleration works only when the graphics are displayed on the same machine as the program runs).

Unfortunately, there is not workaround/fix for this situation yet, except to use ImageJ locally when you want to use the 3D Viewer.

=== Problem with Intel graphics cards ===

There is a known problem with older Windows drivers for some Intel graphics cards. Usually, this is fixed by installing new drivers. If you would like to help make ImageJ nicer by detecting faulty driver versions, please [[Contact|contact us]].

=== The 3D Viewer simply crashes ===

Unfortunately, there are quite a large number of possible reasons. Please help us by [[Debugging_intro#Debugging_Java3D_issues|debugging the issues]] and [[Contact|contacting us]] with the information. You can also [[Report a Bug|report a bug]], which will provide a lot of additional, potentially useful information.

=== Only a gray rectangle is shown by the 3D Viewer ===

As with 3D Viewer crashes, there are quite a large number of possible reasons. Please help us by [[Debugging_intro#Debugging_Java3D_issues|debugging the issues]] and [[Contact|contacting us]] with the information. You can also [[Report a Bug|report a bug]], which will provide a lot of addidtional, potentially useful information.

== Publication ==
* {{Publication | 3D Viewer}}

[[Category:Plugins]]
[[Category:Visualization]]
[[Category:Citable]]
