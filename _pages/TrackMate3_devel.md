This page is for developer only. It is used to store various metrics related to the {{GitHub|org=fiji|repo=TrackMate3}} project.


== Model creation time and space used ==

From the branch {{GitHub|org=fiji|repo=TrackMate3|tag=trackmate-model}} branch, on Feb 13th, 2015. Using the net.trackmate.CreateLargeModelExample and the [http://www.javamex.com/classmexer/ ClassMexer] agent.

<pre>

initial capacity = 2866900 

1. 50e3 spots.

Model created in 306 ms.
Total number of spots: 57338
Total number of cells in the last frame: 8192
Total number of cells in the last two frames: 12288
Total memory used by the model: 256.0 MB


2. About 3 million spots. TrackMate 2 is already beaten there.

Model created in 1952 ms.
Total number of spots: 2866900
Total number of cells in the last frame: 409600
Total number of cells in the last two frames: 614400
Total memory used by the model: 256.0 MB

3. 5 million spots. Things start to get comfortable for biologists.

Model created in 3767 ms.
Total number of spots: 5733800
Total number of cells in the last frame: 819200
Total number of cells in the last two frames: 1228800
Total memory used by the model: 511.9 MB

4. 11 million spots.

Model created in 7517 ms.
Total number of spots: 11467600
Total number of cells in the last frame: 1638400
Total number of cells in the last two frames: 2457600
Total memory used by the model: 1.0 GB

5. About 30 million spots!!

Model created in 21311 ms.
Total number of spots: 28669000
Total number of cells in the last frame: 4096000
Total number of cells in the last two frames: 6144000
Total memory used by the model: 3.6 GB

</pre>

It seems that the memory used while creating the model is much higher than the final one, but it's properly garbage collected at the end. Also notice the impact of the initial capacity: the model is never below 256 MB.

{{Person|JeanYvesTinevez}} ([[User talk:JeanYvesTinevez|talk]]) 10:16, 13 February 2015 (CST)
