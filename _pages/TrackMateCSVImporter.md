= CSV to TrackMate importer. =

This plugin allows for importing detections and tracks contained in CSV files into TrackMate, or to export them as TrackMate XML file in headless mode.


== Installation. ==

This plugin lives on a separate {{ListOfUpdateSites|update site}}. If you want to use it, you first need to subscribe to the update site named <code>TrackMateCSVImporter</code>, as explained [[Following_an_update_site|here]].

The importer can be found in the {{bc|  Plugins | Tracking | TrackMate CSV importer}} menu.


== Using the GUI. ==

The example below shows a capture of the GUI when re-importing a CSV file created by TrackMate itself (from the <code>Analysis</code> button).

[[File:TrackMateCSVImporter 01.png|800px]]

Open the target image in Fiji, and browse to the CSV file from the GUI. It will be parsed and the parameter lists will be populated with the headers of the CSV file. Some columns are mandatory (X, Y, frame). If you uncheck <code>Compute all features?</code> box, only a minimal set of features will be declared and computed.

Depending on whether you specify to import the track values or not, the TrackMate GUI will be created at a different stage. 


== Running the exporter from the command line. ==

After installation, a Jython script called <code>CsvToTrackMate.py</code> will be addede to the <code>scripts</code> folder of your Fiji installation. It is meant to be called in [[Headless|headless mode]] to directly convert a CSV file and an image file into a TrackMate file.

You can use Fiji in headless mode, to call the Jython script 
<code>CsvToTrackMate.py</code> that will parse arguments and configure the 
importer properly. Here is an example:

<source lang=sh>
./ImageJ-macosx --headless   /path/to/scripts/CsvToTrackMate.py 
	--csvFilePath="/path/to/MyCsvFile.csv" 
	--imageFilePath="/path/to/MyImage.tif"
	 --xCol=1 
	 --radius=2 
	 --yCol=2 
	 --zCol=3 
	 --frameCol=0
	 --targetFilePath="/path/to/TrackMateFile.xml"
</source>

This will create a new TrackMate file <code>/path/to/TrackMateFile.xml</code> with detections created from the CSV file <code>/path/to/MyCsvFile.csv</code> and reading the image metadata from image file <code>/path/to/MyImage.tif</code>.
