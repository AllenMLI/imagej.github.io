<includeonly>[https://github.com/{{{org|{{{repo|imagej}}}}}}/{{{repo|{{{org|imagej}}}}}}/{{#if:{{{source|}}}|blob/{{{tag|master}}}/src/main/java/{{{source|}}}|{{#if:{{{path|}}}|blob/{{{tag|master}}}/{{{path|}}}|{{#if:{{{commit|}}}|commit/{{{commit|}}}|{{#if:{{{issue|}}}|issues/{{{issue|}}}|{{#if:{{{pr|}}}|pull/{{{pr|}}}|{{#if:{{{tag|}}}|releases/tag/{{{tag}}}|}}}}}}}}}}}} {{{label|on GitHub}}}]<nowiki /></includeonly><noinclude>
==Usage==
<pre>
{{GitHub
| org    =
| repo   =
| tag    =
| path   =
| source =
| commit =
| issue  =
| pr     =
| label  =
}}
</pre>

==Examples==

Use <code>path</code> to link to a file off the repository root:
<pre>
{{GitHub|path=WELCOME.md}}
</pre>
{{GitHub|path=WELCOME.md}}

Use <code>source</code> to link to a source file within the repository's <code>src/main/java</code> subtree:
<pre>
{{GitHub|source=net/imagej/ImageJ.java}}
</pre>
{{GitHub|source=net/imagej/ImageJ.java}}

Use <code>org</code> and <code>repo</code> to link to a different repository than <code>imagej/imagej</code>:
<pre>
{{GitHub|org=fiji|repo=TrackMate|path=README.md}}
</pre>
{{GitHub|org=fiji|repo=TrackMate|path=README.md}}

You can give just a <code>repo</code> (or just an <code>org</code>), and it will be used for both <code>org</code> and <code>repo</code>:
<pre>
{{GitHub|repo=fiji|path=plugins/Examples/Fiji_Cube.ijm}}
</pre>
{{GitHub|repo=fiji|path=plugins/Examples/Fiji_Cube.ijm}}

If you give neither a <code>path</code> nor a <code>source</code> then it links to the repository as a whole:
<pre>
{{GitHub|repo=fiji}}
</pre>
{{GitHub|repo=fiji}}

Use <code>tag</code> to specify a tag (rather than <code>master</code>):
<pre>
{{GitHub|tag=imagej-2.0.0-beta-7.9|path=app/src/test/java/imagej/debug/TypeHierarchy.java}}
</pre>
{{GitHub|tag=imagej-2.0.0-beta-7.9|path=app/src/test/java/imagej/debug/TypeHierarchy.java}}

Specifying <code>tag</code> alone links to the tag description:
<pre>
{{GitHub|tag=imagej-2.0.0-rc-44}}
</pre>
{{GitHub|tag=imagej-2.0.0-rc-44}}

Use <code>commit</code> to specify a commit hash:
<pre>
{{GitHub|commit=7a10880d485a13fc449d84c7e2eca3e1481064ee|label=imagej@7a10880d}}
</pre>
{{GitHub|commit=7a10880d485a13fc449d84c7e2eca3e1481064ee|label=imagej@7a10880d}}

Use <code>issue</code> or <code>pr</code> to specify an issue or PR number:
<pre>
{{GitHub|issue=83|label=imagej#83}}
{{GitHub|pr=88|label=imagej#88}}
</pre>
{{GitHub|issue=83|label=imagej#83}}
{{GitHub|pr=88|label=imagej#88}}

Use <code>label</code> to override the label:
<pre>
{{GitHub|repo=fiji|path=plugins/Examples/Fiji_Logo_3D.js|label=Fiji... in 3D!}}
</pre>
{{GitHub|repo=fiji|path=plugins/Examples/Fiji_Logo_3D.js|label=Fiji... in 3D!}}
</noinclude>
