=== Introduction ===
From version 3.7.2, Weka has a '''package manager''' (understanding package not in the java sense but as a bundle of additional functionality, separate from that supplied in the main weka.jar file). Many learning algorithms and tools that were present in earlier versions of Weka have become separate packages from version 3.7.2 and need to be installed via the package manager.

In this tutorial we describe step by step '''how to add new classifiers''' to the [[Trainable Weka Segmentation]] plugin using the [http://weka.wikispaces.com/How+do+I+use+the+package+manager%3F Weka package manager].

=== Calling the package manager ===

We assume you are familiar with the [[Trainable Weka Segmentation]] plugin and know why and how to use it, otherwise, please go back to a more basic tutorial.


To get started, from the main plugin GUI click on the Weka button (on the bottom left corner of the window):

[[Image:TWS-GUI-ubuntu.png|700px]]


This will launch the Weka GUI Chooser and, if it is the first time, a warning dialog will be displayed to warn you about the package manager:

[[Image:Weka-GUI-Chooser-package-warning-ubuntu.png]]


We can just click OK, go the Weka GUI Chooser window and look for the package manager under the menu {{bc | Tools | Package manager}}, or by pressing {{key|Ctrl|U}}:

[[Image:Weka-GUI-Chooser-package-manager.png]]


This will open the Weka Package Manager window:

[[Image:Weka-package-manager-window.png|700px]]


=== Adding a new classifier ===
Once the package manager is open, we can select any "package" from the displayed list to install it or to remove it from our working Weka toolbox.

In this tutorial we show how to install a new classifier, in particular the ''ClassificationViaClustering'' classifier. For that reason, we scroll down until we find the packaged called "classificationViaClustering" and click on "Install":

[[Image:Weka-package-manager-classificationViaClustering.png|700px]]


A confirmation dialog will pop up:

[[Image:Weka-package-manager-confirmation.png]]


We click on "Yes" and another dialog will pop up to warn us about closing any active Weka application window that we might have open:

[[Image:Weka-warning-install-package.png]]


We click OK and the package will be installed. If everything goes right, the package should be listed with a "Yes" in the "Loaded" column:

[[Image:Weka-package-manager-loaded-package.png|700px]]


This means the ''ClassificationViaClustering'' classifier '''is now installed''' and ready to use. We can check it out by going to the settings dialog of the [[Trainable Weka Segmentation]] plugin and confirming the classifier is now listed:

[[Image:TWS-settings-new-classifier.png]]



[[Category:Tutorials]]
[[Category:Segmentation]]
[[Category:Machine Learning]]
