{{UpdateSitesMenu}}{{TOC}}

== Introduction ==

This tutorial explains how to upload changes to core [[ImageJ]] and [[Fiji]] libraries.

The typical workflow is:

# Update {{GitHub | org=fiji | repo=fiji | label=Fiji.git}} to depend on the latest {{GitHub | org=scijava | repo=pom-scijava | label=pom-scijava}}
# Use <code>mvn -Dimagej.app.directory=$HOME/Desktop/Fiji.app/ -Ddelete.other.versions=true</code> to install into an existing (up-to-date) Fiji installation
# NOTE: if there are any version downgrades at this point, this indicates dependency skew. Fiji.git should be updated appropriately, restarting from step 1
# Upload changes to ImageJ update site
# Upload changes to Fiji update site

{{Warning | There is a known issue where '''bio-formats_plugins.jar''' is placed in <code>/jars/bio-formats</code> by this maven job. It should be manually moved to <code>/plugins/</code>}}

== Responsibility of uploaders ==

To facilitate [[Architecture#Reproducible_builds|reproducibility]] and present a unified application to both users and developers, uploaders should strive to keep each core update site synchronized with its corresponding source code.

Because releases are tied to the source code (and the update site contents are not explicitly versioned), the order of update should always be:

# Source code
# Update site

Source repository for each core update site:

{| class="wikitable nicetable"
|'''Update Site'''
|'''Source'''
|-
|ImageJ
|''master'' branch of [https://github.com/imagej/imagej ImageJ.git]
|-
|Fiji
|''master'' branch of [https://github.com/fiji/fiji Fiji.git]
|}

== Getting started ==

First of all, start the [[updater]] with {{bc | Help | Update}} and click on the ''Manage update sites'' button:

[[File:Mamed-3.jpg|770px]]

From this dialog, you can edit the desired update site(s) to add your authentication information.

== Configuring Fiji update site ==

The Fiji update site uses [[wikipedia:WebDAV|webDAV]] authentication. To upload something, you will need to:
* [https://imagej.net/index.php?title=Special:UserLogin&type=signup Create a wiki account]
* Ask an administrator has to add you to the [https://imagej.net/Special:ListUsers?group=uploaders ''uploaders'' group]
* Once you have been added, [[Special:ChangeUploadPassword|initialize your upload password for "Fiji's main update site"]].

In the ''Manage update sites'' dialog, on the Fiji update site line, add the following information:

* Host = '''webdav:YourFijiWikiLogin'''
* Directory on host = '''./'''

Note that your username will always start with an upper case letter. It should look like this:

[[File:Update_site_fiji_creds.png|770px]]

You can now close the ''Manage update sites'' window and go on to [[#Uploading your resources | Uploading your resources]].

== Configuring the ImageJ update site ==

The ImageJ update site uses [[wikipedia:Secure Shell|ssh]] authentication. You will need a login with [http://update.imagej.net the imageJ update site] that some administrator will have to add to the ''ij-update'' group.

In the ''Manage update sites'' dialog, on the ImageJ update site line, add the following information:

* Host = <code>yourImageJLogin@update.imagej.net</code>
* Directory on host = <code>/home/imagej/update-site/</code>

It should look like this:

[[File:Update_site_ij2_creds.png|770px]]

You can now close the ''Manage update sites'' window and go on to [[#Uploading your resources | Uploading your resources]].

== Uploading your resources ==

See the [[How_to_set_up_and_populate_an_update_site#Uploading_files_to_your_update_site | Uploading files to your update site]] section of the set up and populate tutorial.
