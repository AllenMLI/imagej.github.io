---
title: Sholl Analysis
name: Sholl plugins
categories: [Analysis,Neuroanatomy]
tags: [sholl,sholl analysis,snt,arbor,neuron,morphometry,dendrite,neuroanatomy]
forum-tag: sholl-analysis
artifact: org.morphonets:SNT
icon: /media/icons/snt.png
doi: 10.1038/nmeth.3125
---

Automated and multithreaded Sholl for direct analysis of fluorescent images and/or traced morphologies. Features powerful quantifications based on curve fitting. Analyis of profiles obtained by other software is also possible. Originally released as a stand-alone Fiji plugin, this program is now part of [SNT](/plugins/snt).

{% capture text%}
***Sholl Analysis* is based on a [publication](#publication). If you use it successfully for your research please be so kind as to cite our work!**

The [manuscript](http://www.nature.com/nmeth/journal/v11/n10/full/nmeth.3125.html) is accompanied by a [Supplementary Note](https://www.nature.com/articles/nmeth.3125#Sec2) that presents the software in further detail. 
{% endcapture %}
{% include notice icon="info" content=text %}


## Introduction

<span id="ca1-cell-mask"></span>
{% include img align="right" name="ca1-cell-mask" src="bitmapsholl-ca1mask" caption="[Skeletonized](/plugins/skeletonize3d) hippocampal [CA1 cell](https://doi.org/10.1111/j.1460-9568.2010.07283.x) (juvenile mouse) in which apical and basal dendrites have been analyzed [separately](#ca1-cell-plot) and [color coded](#output-options) according to their Sholl profile. Warmer hues indicate higher number of Intersections (*N*). [Critical radius](#critical-radius) (*r<sub>c</sub>*) and [Mean value](#mean-value-of-function) (*N<sub>av</sub>*) are indicated." %}

The Sholl technique[^2] is used to describe neuronal arbors. This plugin can perform Sholl directly on 2D and 3D grayscale images of isolated neurons. Its internal algorithm to collect data is based upon how Sholl analysis is done by hand — it creates a series of concentric *shells* (circles or spheres) around the focus of a neuronal arbor, and counts how many times connected voxels defining the arbor intersect the sampling shells. The major advantages of this plugin over other implementations are:

-   When analyzing images directly, it does not require previous tracing of the arbor (although it can also analyze [traced arbors](#analysis-of-traced-cells))
-   It combines [curve fitting](#methods-table) with several [methods](#sholl-plots) to automatically retrieve [quantitative descriptors](#metrics) from sampled data, which allows direct statistical comparisons between arbors
-   It allows [continuous and repeated sampling](#multiple-samples-and-noise-reduction) around user-defined foci
-   It allows [batch processing](#batch-processing)


## Installation

After [installing SNT](/plugins/snt/#installation), Sholl commands can be accessed through the {% include bc path='Plugins|Neuroanatomy|Neuroanatomy Shortcut Window'%}, or the SNT icon in the ImageJ toolbar.


## Direct Analysis of Images

In this mode (bitmap analysis), the plugin requires a [binary image or a segmented](#faq:threshold) [grayscale image](#faq:image-types) (2D or 3D) containing a single neuron.

1.  Segment the neuronal arbor using {% include bc path='Image|Adjust|Threshold...'%} (shortcut: {% include key keys='Shift|T' %}). When using multichannel images, you will have to set the its display mode to *Grayscale* using {% include bc path='Image|Color|Channels Tool...'%} ({% include key keys='Shift|Z' %}), because images displayed as *Composites* cannot be thresholded.
2.  Define the center of analysis using a valid [startup ROI](#startup-roi).
3.  Run {% include bc path='Analysis|Sholl|Sholl Analysis...'%}, adjusting the default [Parameters](#parameters) in the dialog prompt.
4.  Press *More » [Cf. Segmentation](#cf-segmentation)* to visually inspect the two thresholded phases: *arbor* and *background*.
5.  Problems? Read the [FAQs](#faq).

{% capture text%}
Some of the illustrations of this manual pertain to older versions of the software. Most prompts have meanwhile been simplified. However, descriptions remain relevant to current use.
{% endcapture %}
{% include notice icon="info" content=text %}

### Startup ROI

The center of analysis can be specified using one of three possibilities:

1. Straight line: A Straight line from the focus of the arbor to its most distal point using the Straight Line Tool. The advantages of using line selections are twofold: 1) Center of analysis and [Ending radius](#end-radius) are automatically set, and 2) Horizontal/vertical lines (created by holding {% include key key='Shift' %} while using the Straight Line Selection Tool) can be used to [restrict analysis to sub-regions](#restrict) of the image.  

2. Single point: A single point marking the focus of the arbor using the Point Selection Tool. With single point selections, only the center of analysis is defined. Thus, this option is suitable for [batch processing](#batch-processing) of images with different dimensions with undefined [Ending radius](#end-radius).  

3. Multi-point selection:A Multi-point selection (multi-point counter) in which the first point marks the center of analysis while the remaining points mark (count) the number of primary branches required for the calculation of [ramification indices](#schoenen-sampled)). Suitable for cases in which [inference from starting radius](#primary-branches) is not effective.  

{% include img align="center" name="sholl plots" src="/media/plugins/shollanalysisstartuprois.png" %}

Three types of ROIs expected by the plugin when analyzing images directly. <b>Left</b>: Line defining center of analysis (focal point), hemisphere restriction and ending radius. <b>Middle</b>: Single point defining center of analysis. <b>Right</b>: Multi-point selection in which the first point defines the focal point while the remaining points (2 to 5) serve as counters for primary neurites.


### Cf. Segmentation

Press *More» Cf. Segmentation* to visually confirm which phase of the segmented image will be sampled. This command highlights foreground from background pixels and is particularly useful when analyzing black and white (binary) images or when using the *B&W* lookup table in the Threshold Widget ({% include bc path="Image | Adjust | Threshold..." %} {% include key keys='Shift|T' %}). *Cf. Segmentation* allows you to ensure that you are measuring neuronal processes and not the interstitial spaces between them. Here is an example using an axonal arbor of a Drosophila olfactory neuron from the [DIADEM](http://diademchallenge.org) dataset[^3]:

<table>
  <tbody>
    <tr>
      <td>
        <center>
          <p><span style="display:inline-block;text-align:center;width:230px">Segmented image</span> <span style="display:inline-block;text-align:center;width:230px"><em>Cf. Segmentation output</em></span> <span style="display:inline-block;text-align:center;width:230px"><em>Intersections mask</em></span></p>
        </center>
      </td>
    </tr>
    <tr>
      <td>
        <center>
          <figure>
            <img src="/media/plugins/cfsegmentation.png" width="700">
          </figure>
        </center>
      </td>
    </tr>
    <tr>
      <td>
        <center>
          <p><strong>Top row:</strong> Image properly segmented: Arbor is sampled. <strong>Bottom row:</strong> Aberrant segmentation (inverted image): Background is sampled.</p>
        </center>
        <p>Note the reversal of <em><a href="#cf-segmentation">Cf. Segmentation</a> output</em> and how the <em><a href="#output-options">intersections mask</a></em> no longer decorates the axonal processes but the interstitial spaces between them. The consequences of the phase inversion are twofold: 1) the program oversamples (cf. hue ramps on upper left of <em>Intersections mask</em>) and 2) the program detects artifacts induced by the edges of the image (cf. top-right and bottom-right corners of mask where intersections are sampled in the absence of real axons at those locations). Also, note that the initial black and white image would <em>look the same</em> under an inverted lookup table ({% include bc path='Image|Lookup Tables|Invert LUT'%}).</p>
      </td>
    </tr>
  </tbody>
</table>


{%- capture binary-tip -%}
With binary images, *Sholl Analysis* treats zero intensities as the background, independently of the image lookup table or the state of the *Black background option* in {% include bc path="Process | Binary | Options..." %}. As with any other [ImageJ routine](https://imagej.nih.gov/ij/docs/guide/146-29.html#infobox:blackBackground) , confusing background with foreground pixels will lead to aberrant results, including: 1) overestimation of branches and 2) artifacts at distances intersecting the boundaries of the image canvas.
{%- endcapture -%}
{% include notice icon="tip" content=binary-tip %}

<span id="traces"></span>


## Analysis of Traced Cells

{% include img align="right" width="300px" src="/media/sholltracingsprompt.png" caption="Main prompt (v3.6.8) when input is traced data" %}

In this mode the plugin analyzes reconstructed arbors, which is particularly relevant for stainings that do not allow single-cell resolution or proper segmentation. Traced cells are better analyzed through [SNT](/plugins/snt/analysis#sholl-analysis)'s main interface, as it offers more options for defining the center of analysis, tagged branches, etc.  However, dedicated prompts are also available ({% include bc path='Plugins|Sh|Sholl Analysis (Tracings)...'%}) for cases in which viewing traces is not necessary, or when [batch processing](#batch-processing) is needed, since such prompts are macro recordable and/or themselves created by scripts. 

> You can use analyze reconstructed data from any software capable of [SWC](/plugins/snt/faq#swc), not just [SNT](/plugins/snt). Third-party software includes [Py3DN](https://sourceforge.net/projects/py3dn/), [Neuromantic](http://www.reading.ac.uk/neuromantic/) , [NeuronStudio](http://research.mssm.edu/cnic/tools-ns.html) , [neuTube](http://www.neutracing.com) , or [Vaa3D](http://www.vaa3d.org/) ). In addition, [L-Measure](http://cng.gmu.edu:8080/Lm/) , [NLMorphologyConverter](http://neuronland.org/NL.html) or [Neuron](http://www.neuron.yale.edu/neuron/) can also be used to convert several file formats (including proprietary formats from closed-source commercial software such as Neurolucida®, MicroBrightField, Inc.) into SWC.

1.  Run {% include bc path='Sholl Analysis (Tracings)...'%} and specify a [tracing file](/plugins/snt/faq#file-format) (`swc`, `eswc` or `traces` extension). If needed, you can also specify the image associated with the reconstruction. This will allow the plugin to use the image's metadata to determine spatial units and x,y,z spacing.

2.  Choose the center of analysis using the drop down menu in the main prompt listing SWC tags (*axon*, *dendrite*, *soma*, etc.). Note that if your tracings are not tagged you can do so in [SNT](/plugins/snt/analysis#sholl-analysis)

3.  Adjust the default [Parameters](#parameters)

4.  Problems? Read the [FAQs](#faq)


## Analysis of Existing Profiles
<span id="importing"></span>

<span id="ca1-cell-plot"></span>
{% include img align="right" name="ca1-linear-plot" src="bitmapsholl-ca1compartment" caption="Linear plot for CA1 cell [described above](#ca1-cell-mask). Using the soma as center, image was sampled twice using the [Restrict analysis to hemicircle/hemisphere](#restrict) option in order to segregate apical from basal dendrites. For convenience, distances for basal branches were assigned negative values. For clarity, the binary image of the arbor was rotated, scaled and overlaid (in green) over the plot canvas. Note that it is also possible to restrict [curve fitting](#methods-table) to a sub-range of distances once [data is collected](#importing)." %}


- **Input data**: Any tab or comma delimited text file (.csv, .txt, .xls, .ods) can be used. You can drag & drop these files into the main ImageJ window, import data from the clipboard, or use data from any other table already opened by ImageJ.
- **Restricting input data**: To restrict measurements to a range of distances ([see related example](#ca1-cell-plot)), select the range of distances you want analyze. You can click the first row in the range, and then drag the mouse to the last row, or by holding down {% include key key='Shift' %} while selecting the last row in the range. Then, in the prompt, activate the *Restrict analysis to selected rows only* checkbox.
- **Calculation of *Radius step size***: [Radius step size](#step-size) is calculated from the difference between the first two rows in *Distance column*. This is mainly relevant when choosing *Annulus/Spherical shell* as [normalizer](#normalizer).



## Parameters

The majority of parameters is shared by all the Analysis commands in the{% include bc path='Analysis|Sholl| '%} submenu. However, some settings are specific to the type of data used as input: A segmented image, a tracing, or a previously obtained profile. When analyzing images, input values take into account the scale information of the image (which can be set using the {% include bc path='Analyze|Set Scale...'%} or {% include bc path='Image|Properties...'%} ({% include key keys='Shift|P' %}), the type of image (2D or 3D), and its [active ROI](#startup-roi).


#### Definition of Shells


- <span id="start-radius"></span>**Starting radius** The radius of the smallest sampling circle/sphere, i.e., the first distance to be sampled.

- <span id="end-radius"></span>**Ending radius** The radius of the largest (last) sampling circle/sphere. It is automatically calculated if a [line ROI is used](#usage). Note that the specified distance may not be actually sampled, if *Radius step size* is not a divisor of *Ending radius*-*Starting radius*. In this case, the program will choose the largest possible distance smaller than the specified value.
  
  You can clear *Ending radius* or set it to *NaN* ("Not a Number") to sample the entire image. This is particularly useful when [batch processing](#batch-processing) images with different dimensions.

- <span id="step-size"></span>**Radius step size** The sampling interval between radii of consecutive sampling circles/spheres. This value may be set to zero for continuous (1-voxel increment) measurements.

  For stacks with anisotropic voxel size, setting *Radius step size* to zero, sets the step length to the dimension of the matching isotropic voxel, i.e., the cube root of the product of the voxel dimensions (3D images) or the square root of the product of the pixel dimensions (2D images).

- <span id="restrict"></span>**Restrict analysis to hemicircle/hemisphere** This option is only available when an orthogonal radius has been created (by holding {% include key key='Shift' %} while using the <span style="border-bottom:1px dotted #ccc;">Straight Line Selection Tool</span>). It can be used to limit the analysis to sub-compartments of the arbor.

  For horizontal lines, this option instructs the algorithm to measure intersections at sites equidistant from the center that have y-coordinates above/below the drawn line. For vertical lines, it instructs the plugin to measure intersections at sites equidistant from the center that have x-coordinates to the left/right of the drawn line.

{% include img align="right" width="320px" src="bitmapsholl-prompt-v3.png" caption="Main prompt (v3.4.1) when input is a segmented image" %}


#### Multiple Samples/Noise Reduction

- **Samples per radius *(2D images only)*** Defines the number of measurements to be performed at each sampling circumference. These measurements are then combined into a single value according to the chosen [integration method](#samples-integration). This strategy, a break from previous approaches, increases the accuracy of non-continuos profiles by diluting out the effect of processes extending tangent to the sampling circumference.

  Visually, this option can be imagined as the "thickness" of the sampling circumference: e.g., for a radius of 100 pixels and a value of 3 *Samples per radius*, the final number of intersections would integrate the measurements sampled at distances 99, 100 and 101. Note that it would not make sense to increase the number of samples beyond the length (in pixels) of *Radius step size*. For this reason, this option is limited to a draconian (and arbitrary) maximum of 10 samples.

- <span id="samples-integration"></span>**Samples integration *(2D images only)*** The measure of central tendency used to combine intersection counts when multiple *Samples per radius* are used. Options are *Mean* (the default), *Median* or *Mode*.

- **Ignore isolated (6-connected) voxels *(3D images only)*** - If checked, single isolated voxels intersecting the surface of sampling spheres are not taken into account, which may allow for smoother profiles on noisy image stacks. However, it should be noted that connectivity in the stack volume may not reflect connectivity on the surface of a digitized sphere. Indeed, in certain contexts, it is possible (though unlikely) to obtain higher intersection counts when this filtering option is active.

  Keep in mind that this is just a refinement feature, and you should not expect it to mitigate major artifacts derived from poor segmentation.


#### Descriptors and Curve Fitting

- <span id="enclosing-radius-cutoff"></span>**Enclosing radius cutoff** The number of intersections to be used in the definition of [Enclosing radius](#enclosing-radius).

- <span id="primary-branches"></span>**Number of primary branches** The number of primary branches (i.e., those originating directly from cell soma when the center of analysis is the perikaryon) to be used in the calculation of [Schoenen ramification indices](#schoenen-sampled). It is automatically populated using multi-point counts if a multi-point ROI is [detected at startup](#startup-roi). Set it to zero (or *NaN*) to disable calculations of ramification indices.

    -   **Infer from starting radius** If checked, the *Number of Primary branches* is inferred from the count of intersections at [Starting radius](#start-radius).

{% include img align="right" width="320px" src="bitmapsholl-tabularprompt.png" caption="Main prompt (v3.4.3) when input is tabular data" %}

-   **Fit profile and compute descriptors** If checked, data is fitted according to the chosen [method](#choice-of-methods) and appropriate [metrics](#metrics) calculated automatically. If unchecked, only sampled data is plotted.

    -   **Show fitting details** - Choose this option to have all of the parameters of the simplex fitting printed to the Log window. The {% include wikipedia title='Coefficient of determination' text='coefficient of determination'%} (*R<sup>2</sup>*, a measure of goodness of fit) is always stored in the *Sholl Results* table even when this option is not selected.



#### Choice of Methods

<span id="sholl-methods"></span>The [type of profile(s)](#methods-table) to be obtained. *Linear* (profile without normalization), or normalized profiles: *Linear-norm*, *Semi-log*, or *Log-log*.

- **Polynomial** Specifies the degree of the [polynomial](#methods-table) to be fitted to the *Linear* profile[^4]. While the polynomial of best approximation, or "best fit", should be empirically determined for each analyzed cell type, it is possible to ask the plugin to predict the order of the fitting polynomial (or at least try) using the choice *Best fitting degree*. In this case, the plugin will loop through all the available choices of polynomials, perform each fit in the background and choose the one with the highest coefficient of determination.

- **Most informative** Select this option when you cannot predict which type of normalized profile best describes the dataset. If chosen, the plugin will use the [Determination ratio](#dratio) to determine which of *Semi-log* or *Log-log* methods is more appropriate. *Linear-norm* is not performed. *Best fitting degree* and *Most informative* choices are obviously more computer-intensive and can be monitored by activating the *Show fitting details* checkbox.

- <span id="normalizer"></span>**Normalizer** The property of the sampling shell to be used in the normalization of *Linear-norm*, *Semi-log*, and *Log-log* profiles. It is [described below](#methods-table).



#### Output Options

- **Create intersections mask** If checked, a 16/32–bit maximum intensity projection of the analyzed image is generated in which the measured arbor is painted according to its Sholl profile. The type of data (*Raw*, i.e., sampled or *Fitted*) is displayed in the image subtitle and can be specified in {% include bc path='Analysis|Sholl|Metrics & Options...'%} or using the *Options...* command in the *More»* drop-down menu.
 
  The default Lookup Table (LUT) used by the mask can be changed using {% include bc path='Image|Lookup Tables|'%}. The background color \[gray level: 0 (black) to 255 (white)\] can also be set in {% include bc path='Metrics & Options...'%}, or at any later point using {% include bc path='Image|Color|Edit Lut...'%} WYSIWYG versions (RGB images) of these masks can be otained using by pressing {% include key keys='Shift|F' %} ({% include bc path='Image|Overlay|Flatten'%}) or by running {% include bc path='Analyze|Tools|Calibration Bar...'%}

{% include img align="right" src="/media/plugins/shollresultasrois.png" width="400" caption="Intersection points and sampling shells can be retrieved as ROIs. Intersection points are placed at edges of detected clusters of foreground pixels, not their center." %}

- **Overlay sampling shells and intersection points (2D images only)** If checked, two sets of ROIS are added to the image overlay: 1) concentric shells matching sampled distances (circular ROIs or composite ROIs when using hemicircles); and 2) Multipoint ROIs at intersection sites between shells and clusters of foreground pixels.

- **Save results to** If checked, all the results (with the exception of the *[Sholl Table](#metrics)*) are saved to the specified directory. These include: 1) Sholl plots (saved as PNG images), 2) A table containing detailed data and 3) The Sholl mask. Files are named after the image filename and analysis method. Saving options can be specified in {% include bc path='Analysis|Sholl|Metrics & Options...'%} (*Options...* command in the *More»* drop-down menu).

    -   **Do not display saved files** If checked, saved files are directly saved to disk and are not displayed. Activate this option when [batch processing](#batch-processing) files.



## Sholl Plots

{% include img align="center" name="sholl plots" src="shollplots" %}

***Linear*, *Linear-norm*, *Semi-log* and *Log-log* profiles for the ddaC cell ({% include bc path='File|Open Samples|ddaC Neuron'%}), version 3.0**. Most of the retrieved [metrics](#metrics-based-on-fitted-data) are automatically highlighted by the plugin. *Linear profile*: [Mean value](#mean-value-of-function) (horizontal grid line) and [Centroid](#centroid) (colored mark). Logarithmic profiles: The [Sholl regression coefficient](#sholl-decay) (also known as Sholl decay) can be retrieved by linear regression using either the full range of data (blue line) or data within percentiles 10–90 (red line). For this particular cell type, the Semi-log method is more [informative](#dratio) when compared to the Log-log method.

<span id="methods-table"></span>
<table>
  <thead>
    <tr class="header">
      <th></th>
      <th>
        <p>Method</p>
      </th>
      <th>
        <p>Fit</p>
      </th>
      <th>
        <p>Description</p>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <p><span id="eq1">(1)</span></p>
      </td>
      <td>
        <p>Linear</p>
      </td>
      <td>
        <p><i>N</i> = a + br + cr<sup>2</sup> + dr<sup>3</sup> + er<sup>4</sup> + fr<sup>5</sup> + ... + xr<sup>n</sup></p>
      </td>
      <td>
        <p><br>
        Outputs a <em>N vs Distance</em> profile. Data is fitted to a polynomial function<a href="#fn:4" class="footnote-ref" id="fnref4" role="doc-noteref"><sup>4</sup></a>. <a href="#critical-radius">Critical radius</a>, <a href="#critical-value">Critical value</a> and <a href="#mean-value-of-function">Mean value of function</a> are calculated<br></p>
      </td>
    </tr>
    <tr>
      <td>
        <p><span id="eq2">(2)</span></p>
      </td>
      <td>
        <p>Linear-norm</p>
      </td>
      <td>
        <p><em>N/S</em> = a r<sup>b</sup></p>
      </td>
      <td>
        <p><br>
        Outputs a <i>N/S vs Distance</i> profile. Points are fitted to a power function. It is an intermediate representation of the data that can be used to gauge the choice of <a href="#normalizer">normalizer</a>. Once plotted under a logarithmic scale the Linear-norm curve is similar to the Semi-log profile<br></p>
      </td>
    </tr>
    <tr>
      <td>
        <p><span id="eq3">(3)</span></p>
      </td>
      <td>
        <p>Semi-log</p>
      </td>
      <td>
        <p>log(<em>N/S</em>) = -k r + m</p>
      </td>
      <td>
        <p><br>
        Outputs a <i>log(N/S) vs Distance</i> profile. A linear regression is fitted to the sampled data. The <a href="#sholl-decay">Sholl regression coefficient</a> (<em>k</em>) is calculated<br></p>
      </td>
    </tr>
    <tr>
      <td>
        <p><span id="eq4">(4)</span></p>
      </td>
      <td>
        <p>Log-log</p>
      </td>
      <td>
        <p>log(<em>N/S</em>) = -k × log(r) + m</p>
      </td>
      <td>
        <p><br>
        Outputs a <i>log(N/S) vs log(Distance)</i> profile. Data is also fitted to a straight line. This is an alternative approach<a href="#fn:5" class="footnote-ref" id="fnref5" role="doc-noteref"><sup>5</sup></a> of obtaining a relevant <a href="#sholl-decay">regression coefficient</a>, when the Semi-log method returns a poor fit<br></p>
      </td>
    </tr>
  </tbody>
</table>

<span style="display: inline-block; width: 25px">***N***</span> For 2D images, the <u>N</u>umber of clusters of pixels (8–connected) intersecting the circumference of radius *r*  
<span style="display: inline-block; width: 25px"> </span> For 3D images, the <u>N</u>umber of clusters of voxels (26-connected) intersecting the surface of the sphere of radius *r*

<span style="display: inline-block; width: 25px">***r***</span> Distance from center of analysis (<u>r</u>adius of Sholl circle/sphere)

<span style="display: inline-block; width: 25px">***log***</span> Natural logarithm, the logarithm to the base *e*

<span style="display: inline-block; width: 25px">***S***</span>The [chosen property ](#choice-of-methods) of the sampling shell to be used in the normalization of *Linear-norm*, *Semi-log*, and *Log-log* profiles.  
<span style="display: inline-block; width: 25px"> </span>For 2D images, the *Perimeter* of the sampling circumference (2πr) or the *Area* of the corresponding circle (πr<sup>2</sup>)  
<span style="display: inline-block; width: 25px"> </span>For 3D images, the *Surface* of the sampling sphere (4πr<sup>2</sup>) or its respective *Volume* (4/3πr<sup>3</sup>)  

> A third [normalization option](#normalizer) is also available when performing [non-continuous sampling](#step-size): *Annulus*/*Spherical shell*. In this case, the normalization is performed against the area of the annulus formed between the circumferences at *r* ± *Radius step size*/2 (2D images), or against the volume between the two spheres at *r* ± *Radius step size*/2 (3D images).  


## Metrics

Morphometric descriptors and other properties of the arbor are printed to a dedicated table named *Sholl Results*. Output is fully customizable using {% include bc path='Analysis|Sholl|Metrics & Options...'%} or using the *Options...* command in the *More»* drop-down menu. The first columns log analysis parameters: *Image Directory*, *filename* and *voxel unit*, *Channel*, *Lower* and *Upper Threshold levels*, *X,Y* (in pixels) and *Z* (slice number) coordinates of center of analysis, *Starting* and *Ending radius*, *Radius step*, *Number of Samples per Radius*, etc. Other parameters are described below.

{% include img align="center" name="Descriptors and metrics are listed in the Sholl Table (v2.4)" src="bitmapsholl-table"%}

### Metrics based on sampled data

{% include img align="right" name="Metrics & Options prompt (version 3.6.4)" src="sholloptionsprompt" %}

- **Intersecting radii** The number of sampling radii intersecting the arbor at least once.

- <span id="sum"></span> **Sum of intersections (*Sum inters.*)** The sum of all intersections.

- <span id="mean-inters"></span> **Mean of intersections (*Mean inters.*)** *Sum inters.* divided by *Intersecting radii*. See also [Mean value](#mean-value-of-function), [FAQ](#faq:AUC)

- **Median of intersections (*Median Inters.*)**  The median value of sampled intersections.

- <span id="skewness"></span> **Skewness** The {% include wikipedia title='Skewness' text='skewness'%} of the sampled data, an indication of how symmetrical the distribution is around its mean. Positive values indicate an asymmetrical distribution with a longer tail to the right. Negative values indicate data with a longer tail to the left. A popular rule of thumb considers that if the skewness is greater than 1.0 (or less than -1.0), the distribution may be considered far from symmetrical. See also [Skewness (fitted data)](#skewness-fitted)

- <span id="kurtosis"></span> **Kurtosis** The {% include wikipedia title='Kurtosis' text='kurtosis'%} of the sampled data, which quantifies whether the shape of the distribution matches that of a Gaussian distribution, assuming that a Gaussian distribution has a kurtosis of 0. A distribution more peaked than a Gaussian has a positive kurtosis while a negative value indicates a flatter distribution. See also [Kurtosis (fitted data)](#kurtosis-fitted)

- <span id="max-inters"></span> **Highest count of intersections (*Max inters.*)**  The maximum value of sampled intersections, i.e., the maximum in a linear \[*N* vs *Distance*\] profile, reflecting the highest number of processes/branches in the arbor. See also [Critical value](#critical-value).
 
- <span id="max-inters-radius"></span> **Radius of highest count of intersections (*Max inters. radius*)** The distance at which the *Highest count of intersections* occurred, reflecting sites of highest branch density. Note that if the same maximum occurs multiple times, only the first distance is considered. See also [Critical radius](#critical-radius)

- <span id="schoenen-sampled"></span> **Schoenen Ramification index (*Ramification index (sampled)*)** A measure of ramification[^6]: the ratio between *Max inters.* and the number of [primary branches](#primary-branches). It is only calculated when [primary branches](#primary-branches) is valid and not zero. See also [Ramification index (fit)](#schoenen-fitted)

- <span id="centroid">**Centroid radius**  The abcissa of the {% include wikipedia title='Centroid' text='centroid'%} (i.e., the geometric center or barycenter) of the linear profile. It is [highlighted](#sholl-plots) on the *N* vs *Distance* plot.

- **Centroid value** The ordinate of the {% include wikipedia title='Centroid' text='centroid'%} (i.e., the geometric center or barycenter) of the linear profile. It is [highlighted](#sholl-plots) on the *N* vs *Distance* plot.

- <span id="enclosing-radius"></span> **Enclosing radius**  The last (thus, the widest) of *Intersecting radii* to be associated with the number of intersections specified by [Enclosing radius cutoff](#enclosing-radius-cutoff). For a cutoff of 1 (the default) *Enclosing radius* is the widest of *Intersecting radii*. It reflects the {% include wikipedia title='Feret diameter' text='Feret length'%} of the arbor.


### Metrics based on fitted data

- <span id="dratio"></span> **Determination ratio**  The ratio of the [coefficient of determination](#reg-r2) for the semi-log method and that for the log–log method[^5]. If the semi-log method is better relatively to the log–log method, the *Determination ratio* becomes larger than 1. It is the parameter used by the plugin to silently predict the normalization method that is the [most informative](#choice-of-methods). The prediction can be monitored by activating the [Show fitting details ](#descriptors-and-curve-fitting) checkbox.

- <span id="sholl-decay"></span> **Sholl regression coefficient (*Regression coefficient*)** The slope (multiplied by -1) of the linear regression described in [(3)](#eq3) and [(4)](#eq4), i.e., *k*, a measure of the rate of decay of the number of branches with distance from the center of analysis. Higher *k* values reflect larger changes in the function log(N/S). To optimize the fit, the plugin retrieves a [second linear regression](#sholl-plots) centered around the median distance, excluding distances at the edges of the profile. Details of this second fit are also registered on the [*Sholl table*](#metrics) under dedicated columns, e.g., *Sholl regression coefficient \[P10-P90\]*, when data within the 10<sup>th</sup>–90<sup>th</sup> percentile is used.

- **Regression intercept**  The y-coordinate *m* described in [(3)](#eq3) and [(4)](#eq4).

- <span id="reg-r2"></span> **Regression R<sup>2</sup> (*Regression R^2*)**  The coefficient of determination of the linear regression described in [(3)](#eq3) and [(4)](#eq4).

- <span id="critical-value"></span> **Critical value**  The local maximum of the polynomial fit, i.e, *N* at *Critical radius* in [(1)](#eq1). Abbreviation: *N<sub>m</sub>*. See also [Max inters.](#max-inters)

- <span id="critical-radius"></span> **Critical radius**  The distance at which *Critical value* occurs. By default (see [Advanced Usage](#advanced-usage)), it is calculated with a precision of 1/1000 of *Radius step size*. Abbreviation: *r<sub>c</sub>*. See also [Max inters. radius](#max-inters-radius)

{% include notice icon="tip" content='**Nomenclature**: [Previous authors](#references) have used different terms to describe the largest value taken by the Sholl profile, including *Dendrite maximum*. Since the Sholl technique is not restricted to dendritic arbors and can be applied to any tree-like structure such as axonal arbors, mammary ducts or blood vessels (cf. [List of citations](#publication)), Here we adopt the term [Critical radius](#critical-radius), renaming *Dendrite maximum* (*N<sub>m</sub>*) to [Critical value](#critical-value).' %}

- <span id="mean-value-of-function"></span> **Mean value**  The mean value[^4] of the fitted polynomial function [(1)](#eq1), representing the average of intersections over the whole area occupied by the arbor. Abbreviation *N<sub>av</sub>*. On the Sholl plot, it is [highlighted](#sholl-plots) as the height of the rectangle that has the width of *Enclosing radius* − *First intersecting radius* and the same area of the area under the fitted curve on that discrete interval. It is analogous to [Mean inters.](#mean-inters), the arithmetic mean of sampled intersections throughout the arbor (cf. [Metrics based on sampled data](#metrics-based-on-sampled-data)). By default (see [Advanced Usage](#advanced-usage)), it is calculated with a precision of 1/1000 of *Radius step size*.

- <span id="schoenen-fitted"></span> **Schoenen Ramification index (*Ramification index (fit)*)** Schoenen Ramification index retrieved from fitted profile: The ration between [Critical value](#critical-value) and [Number of primary branches](#primary-branches). See also [Ramification index (sampled)](#schoenen-sampled)

- <span id="skewness-fitted"></span> **Skewness (*Skewness (fit)*)**  The [skewness](#skewness) of the fitted polynomial distribution between [Starting radius](#start-radius) and [Ending radius](#end-radius).

- <span id="kurtosis-fitted"></span> **Kurtosis (*Kurtosis (fit)*)**  The [kurtosis](#kurtosis) of the fitted polynomial distribution between [Starting radius](#start-radius) and [Ending radius](#end-radius).

- **Polynomial R<sup>2</sup> (*Polyn. R^2*)** The coefficient of determination of the polynomial fit described in [(1)](#eq1).


## Complementary Tools

SNT provides several scripts and commands that faciliate all type of Sholl-related analyses. You'll find them through the {% include bc path='Plugins|Neuroanatomy|Neuroanatomy Shortcut Window'%}, as well as in Script Editor's {% include bc path='Templates|Neuroanatomy|'%} menu. Here are some examples:

{% include gallery align="center" content=
"
/media/plugins/animatedpolyfit.gif | Sampled data from the ddaC cell being fitted to polynomials of varying degree ([BAR](/plugins/bar) script)
/media/plugins/combineshollprofiles.png | *Combine Sholl Profiles...* aggregates individual profiles into a single plot, obtaining the average profile (with standard deviation) of a group of cells (v3.6.12 snapshot)
/media/plugins/snt/sholl-convex-hull.png | The *Convex Hull as Center* script exemplifies how to adopt arbitrary focal points, in this case the centroid of the neuron's convex hull (v4.1.2 snapshot)
"
%}

{% capture tip%}
For additional image processing tools have a look at the growing list of [update sites](/list-of-update-sites). For more information on image processing have a look at [tutorials](/tutorials), [segmentation](/imaging/segmentation) and the [ImageJ User Guide](https://imagej.nih.gov/ij/docs/guide/) .
{% endcapture %}
{% include notice icon="tip" content=tip %}


In addition, several [BAR](/plugins/bar) commands -- installed by subscribing to the [BAR update site](/plugins/bar#installation) -- were designed to complement the first release of *Sholl Analysis*. While, some of these tools have meanwhile become outdate, some remain relevant. These include:

- **[Segmentation](#pre-processing) tools**  Thresholding, shape-based masking and edge-detection routines ([full list](/plugins/bar#list-of-bars))

- **[Find Peaks](/plugins/find-peaks)**  Retrieves local maxima under several filtering options such as peak amplitude, peak height and peak width. Can be used to retrieve secondary sites of branch density

- **{% include github org='tferr' repo='Scripts' branch='master' path='BAR/src/main/resources/scripts/BAR/Data_Analysis/README.md#fit-polynomial' label='Fit Polynomial' %}** Fits a polynomial of any degree to sampled data. Features an heuristic algorithm for guessing a polynomial "best fit". Expands the built-in repertoire of polynomial fits up to 50<sup>th</sup> order functions.

- **{% include github org='tferr' repo='Scripts' branch='master' path='BAR/src/main/resources/scripts/BAR/Data_Analysis/README.md#create-boxplot' label='Create Boxplot' %}** Allows direct comparison of metrics between groups or sets of data (specially useful when tagging images with the *Comment* field in {% include bc path='Analysis|Sholl|Metrics & Options...'%})

- **{% include github org='tferr' repo='Scripts' branch='master' path='BAR/src/main/resources/scripts/BAR/Data_Analysis/README.md#interactive-plotting' label='Interactive Plotting' %}** Whole-purpose routine that plots data from imported spreadsheets.


## Pre-processing

This section discusses some aspects that should be taken into account when segmenting neuronal arbors to be processed by *Sholl Analysis*. Since *image segmentation* (i.e., the partitioning of images into analyzable parts) is vulnerable to noise and background fluorescence, it is not possible to generalize universal routines that efficiently binarize grayscale images. This means that any procedure that tries to appropriately describe the original fluorescence image with a binary mask must be tailored to the characteristics of individual datasets. As mentioned in [Complementary Tools](#complementary-tools), several routines listed here as distributed through the [BAR](/plugins/bar) [update site](/list-of-update-sites).


#### Noise  

Noise can be mitigated through the usage of processing filters. Specially useful are edge-preserving filters:

- [Rolling Ball](/plugins/rolling-ball-background-subtraction) or "Top hat" filters, e.g., {% include bc path="Process | Subtract Background..." %}
- Median Filtering (2D/3D), e.g., {% include bc path="Process | Filters |" %}, {% include bc path="Plugins | 3D |" %}
- [Anisotropic Diffusion](/plugins/anisotropic-diffusion-2d), {% include bc path="Plugins | Process | Anisotropic Diffusion 2D" %}
- Sobel Edge Detection, e.g., {% include bc path="Process | Find Edges" %}
- Shen-Castan Edge Detector ([BAR](/plugins/bar) plugin), {% include bc path="BAR | Segmentation |" %}
- Frequency filters, e.g., {% include bc path="Process | FFT | Bandpass Filter..." %}

#### Uneven Illumination  

Uneven illumination problems, typically associated with [wide field microscopy](http://imagejdocu.list.lu/doku.php?id=howto:working:how_to_correct_background_illumination_in_brightfield_microscopy), do occur in confocal microscopy when signal from deep layers of the tissue is not captured as bright as with superficial layers. This signal attenuation along the Z-axis will generate a shaded gradient across the stack that [histogram-based segmentation](#automated-segmentation) will need to take into account. While these problems are better tackled during acquisition (e.g., using laser ramping), it is possible to mitigate this effect using histogram-normalization techniques. Examples:

- [Bleach Correction](/plugins/bleach-correction), {% include bc path="Image | Adjust |" %}
- [Attenuation correction](http://imagejdocu.list.lu/doku.php?id=plugin:stacks:attenuation_correction:start)

#### Segmentation  

It is possible to adopt more sophisticated [segmentation algorithms](/imaging/segmentation) when [global thresholding methods](/plugins/auto-threshold) do not yield satisfactory results. Examples:

- [Local Threshold](/plugins/auto-local-threshold), {% include bc path="Image | Adjust |" %}
- [Robust Automatic Threshold Selection](/plugins/rats), {% include bc path="Plugins | Segmentation |" %}
- [Level Sets](/plugins/level-sets), {% include bc path="Plugins | Segmentation |" %}
- [Morphological Segmentation](/plugins/morphological-segmentation), {% include bc path="Plugins | Segmentation |" %}
- [Squassh](/plugins/squassh), split-Bregman Image Segmentation (Segmentation and Quantification of Sub-cellular Shapes, [MOSAIC ToolSuite](/plugins/mosaicsuite), {% include bc path="Plugins|Mosaic|Segmentation|" %}
- [MorphoLibJ](/plugins/morpholibj)
- [BAR](/plugins/bar) scripts for manual curation of thresholded images, {% include bc path="BAR | [Segmentation](/plugins/bar#segmentation) |" %}


## Batch Processing


The Script Editor's {% include bc path='Templates|Neuroanatomy|'%} menu lists demo scripts that perform batch operations. For sake of completeness, here is a small tutorial on how to write a macro from the ground up:

### Tutorial: Batch Analysis of Images using IJM Languages

Any macro must set a center, or allow the Sholl Analysis plugin to access a ROI marking it. One could instruct ImageJ to read the coordinates of pre-existing ROIs from a text file, store a list of line selections in the ROI Manager, or write a morphology-based routine that detects the center of the arbor. However, marking the center of analysis is probably something that you will want to do manually. Here is a workflow:

1.  Place all the .tif images to be processed in a single folder.
2.  Select the <span style="border-bottom:1px dotted #ccc;">Point Selection Tool</span> in the main ImageJ window. With 3D images, make sure *Set stack positions* is active in the {% include bc path="Image | Overlay | Overlay Options..." %} prompt.
3.  Open the first image and press {% include key keys='Shift|T' %} to activate the Threshold widget ({% include bc path="Image | Adjust | Threshold..." %}).
4.  Adjust threshold levels. Press the *Apply* button of the Threshold widget to create a binary image.
5.  Select the z-slice containing the center of analysis. Click over the center with the <span style="border-bottom:1px dotted #ccc;">Point Selection Tool</span> and press {% include key key='B' %} (shortcut for {% include bc path="Image | Overlay | Add Selection..." %}). This will add the point ROI to the image overlay. Save the image as .TIFF by pressing {% include key key='S' %} ({% include bc path="File | Save As... | Tiff..." %}).
6.  Repeat the last 2 steps until all images are marked, using {% include key keys='Shift|O' %} (shortcut for {% include bc path="File | Open Next" %}) to iterate through all the images.

{% capture roi-tiff-tip %}
When working with ROIs, it is critical that you work with .tif files because only this format keeps track of image overlays. IJ's {% include bc path="Process | Batch | Convert..." %} command allows bulk conversion between image formats.
{%- endcapture -%}
{% include notice icon="tip" content=roi-tiff-tip %}

Now that all the images are marked, we could use the Macro Recorder ({% include bc path="Plugins | Macros | Record..." %}) and run *Sholl Analysis* on one of the images to find out how to call the plugin with suitable parameters. In this tutorial, we will use the ImageJ macro language. The single line of code that appears in the recorder window will look something like this:

{% highlight javascript %}
// Recording Sholl Analysis version 3.4.3
run("Sholl Analysis...", "starting=10 ending=400 radius_step=0 infer fit linear polynomial=[8th degree] semi-log normalizer=Volume create save do");
{% endhighlight %}

As you may have noticed, ImageJ plugins are controlled by a single lowercase sentence in which arguments are separated by a space. Input fields and choice lists appear as *keyword=value* pairs, active checkboxes by a single keyword. Options that are not needed can be omitted. This makes it easier to edit code blocks:

{% highlight javascript %}
start = 10;  // variable that controls starting radius
end   = 200; // variable that controls ending radius
step  = 2;   // variable that controls step size

// Run the plugin
run("Sholl Analysis...", "starting="+ start +" ending="+ end +" radius_step="+ step +" infer linear save do");
{% endhighlight %}

Now we just need to assemble a working macro to be pasted in the {% include bc path="Process | Batch | Macro..." %} prompt:

{% highlight javascript %}
// Get the number of ROIs of the image overlay
nROIs = Overlay.size;
// We cannot proceed if no ROI is available (safety check)
if (nROIs==0)
    exit("No ROI was found. Cannot proceed.");
// Select the last ROI of the overlay. Because we have activated the "Set stack positions" option,
// this will automatically activate the Z-slice in which the ROI was created (3D images)
Overlay.activateSelection(nROIs-1);
// We now call the plugin as detailed by the Macro Recorder. We'll set 'Ending radius' to a non-numeric
// value (NaN, "Not a Value") to make sure the maximum length for each individual image is used
run("Sholl Analysis...", "starting=10 ending=NaN radius_step=0 infer fit linear polynomial=[8th degree] semi-log normalizer=Volume create save do");
{% endhighlight %}

Of course you can also automate any preceding steps. However, do not forget to ensure that the center of analysis will be available when the plugin is called:

{% highlight javascript %}
// Impose spatial calibration
run("Properties...", "unit=um pixel_width=1.5 pixel_height=1.5 voxel_depth=3.0");
// Subtract background
run("Subtract Background...", "rolling=50 sliding stack");
// Apply a favorite threshold
setAutoThreshold("Huang dark stack");
// >>>> Make sure the initial point selection remains available <<<<
Overlay.activateSelection( Overlay.size - 1 );
// Run the plugin
run("Sholl Analysis...", "starting=10 ending=NaN radius_step=0 infer fit linear polynomial=[8th degree] semi-log normalizer=Volume create save do");
{% endhighlight %}

That's it. Use the Macro Recorder to generate the customizations you will need before parsing the entire folder of images with {% include bc path="Process | Batch | Macro..." %} 


### Examples:

More complex scripts will take advantage of [SNT's API](/plugins/snt/scripting). Here are some examples:

{% include code org='morphonets' repo='SNT' branch='master' path='src/main/resources/script_templates/Neuroanatomy/Analysis/Sholl_Extensive_Stats_Demo.groovy' label='Extensive Stats Demo (Groovy)' %}

{% include code org='morphonets' repo='SNT' branch='master' path='src/main/resources/script_templates/Neuroanatomy/Analysis/Sholl_Extract_Profile_From_Image_Demo.py' label='Extract Profile from Image (Python)' %}



## FAQ

**General**

<ol>
<li markdown="1">

<span id="faq:citing"></span>**How do I cite *Sholl Analysis***?

</li>
<dl>
<dd markdown="1">

The authoritative reference for *Sholl Analysis* is:

-   Ferreira T, Blackman A, Oyrer J, Jayabal A, Chung A, Watt A, Sjöström J, van Meyel D. (**2014**), [Neuronal morphometry directly from bitmap images](http://www.nature.com/nmeth/journal/v11/n10/full/nmeth.3125.html), *Nature Methods* 11(10): 982–984.

The [authoritative reference](/contribute/citing) for Fiji:

-   Schindelin J, Arganda-Carreras I, Frise E, Kaynig V, Longair M, Pietzsch T, Preibisch S, Rueden C, Saalfeld S, Schmid B, Tinevez JY, White DJ, Hartenstein V, Eliceiri K, Tomancak P, Cardona A. (**2012**) [Fiji: an open-source platform for biological-image analysis](http://www.nature.com/nmeth/journal/v9/n7/full/nmeth.2019.html), *Nature Methods* 9(7): 676-682.

</dd>
</dl>
<li markdown="1">

<span id="faq:plugin"></span>**What is the difference between *Sholl Analysis* and an homonymous plugin released by the Ghosh laboratory in 2005**?

</li>
<dl>
<dd markdown="1">

The [original Sholl Analysis plugin](http://labs.biology.ucsd.edu/ghosh/software/) by Tom Maddock (version 1.0) was released for ImageJ 1.35 and is now deprecated, unmaintained software that behaves erratically in newer versions of ImageJ. The current implementation of *Sholl Analysis* inherits Tom's initial 2D algorithm, but has numerous [added features](#release-notes) to enhance its utility. Note that throughout 2012 the plugin was temporarily called *Advanced Sholl Analysis*. You can follow the entire history of the plugin using its [release notes](#release-notes)

</dd>
</dl>
<li markdown="1">

<span id="faq:threshold"></span>**Why do I need to threshold the cell?**

</li>
<dl>
<dd markdown="1">

Counting intersections is really a binary procedure: a shell either intercepts a branch or it doesn't. For this reason the image must be split in two phases: *arbor* and *background*.

</dd>
</dl>
<li markdown="1">

<span id="faq:threshold2"></span>**In version 1.0, it was not mandatory to adjust threshold values prior to analysis. Why is it now?**

</li>
<dl>
<dd markdown="1">

Image segmentation has always been [required](#faq:threshold). In its early implementations, the program dealt solely with binary images and used the intensity at the center of the analysis to decide how to segregate objects from background. This approach was very restringent: It assumed that the pixels representing the neuron would have the same (constant!) intensity that was not to be found in the remaining background. As the program became aware of grayscale images, this "feature" had to be removed because a single intensity can no longer be used to infer which parts of the image should be analyzed.

</dd>
</dl>
<li markdown="1">

<span id="faq:pre-processing"></span>**My images do not look that *great*. How can I treat them prior to analysis?**

</li>
<dl>
<dd markdown="1">

Have a look at [ Pre-processing](#pre-processing).

</dd>
</dl>
<li markdown="1">

<span id="faq:accuracy"></span>**My bitmap profiles are different from the ones obtained from tracings of the same cells. Why?**

</li>
<dl>
<dd markdown="1">

As mentioned several times, the quality of the analysis relies on how the arbor was segmented. If you are working with grayscale images you probably need to optimize your [segmentation routines](#pre-processing). On the other hand, if you already obtained binary images make sure you are [interpreting them properly](#cf-segmentation). You should also confirm that [Ending radius](#end-radius) does not intersect objects in the image canvas that extend beyond the analyzed arbor. As a rule of thumb, always refer to the [Sholl mask](#faq:sholl-mask) to visually inspect which regions of the image have been measured.

</dd>
</dl>
<li markdown="1">

<span id="faq:updates"></span>**My version is not the latest after running {% include bc path="Help | Update Fiji..." %} Why?**

</li>
<dl>
<dd markdown="1">
This may happen if you have manually installed/modified core files.. Run the [Updater](/plugins/updater), choose *Advanced Mode* then *View locally modified files* under *View Options*. Locate files from the *Neuroanatomy* update site. If the *Details pane* indicates an available update, click on *Locally modified* under *Status/Action* and choose *Install/Update*. The latest release version will be available once you press *Apply changes*. See [Installation FAQs](/learn/faq#installingupdating) for more details.
</dd>
</dl>
<li markdown="1">

<span id="faq:documentation"></span>**This documentation is not that useful. How long do I have to wait until it gets improved?**

</li>
<dl>
<dd markdown="1">

Use the Edit this page option on the [top](#top) of the page and edit its contents at will. Don’t be shy. All changes are undoable!

</dd>
</dl>
</ol>

**Analysis**

<ol>
<li markdown="1">

<span id="faq:image-types"></span>**The plugin complains about a wrong image type. Why?**

</li>
<dl>
<dd markdown="1">

The plugin does not parse RGB images, but will process any grayscale image (8/16-bit), including multi-channel (composite) images. This is intentional: RGB images are inflexible and images of fluorescence-labeled cells are typically non-RGB images. As explained in the [ImageJ User Guide](https://imagej.nih.gov/ij/docs/guide/), RGB images can be converted using {% include bc path='Image|Color|Channels Tool...'%} or {% include bc path='Image|Type|'%} commands.

</dd>
</dl>
<li markdown="1">

<span id="faq:compartments"></span>**I cannot see the hemicircle/hemisphere option. Why?**

</li>
<dl>
<dd markdown="1">

This option is only available if an orthogonal line has been created by holding {% include key key='Shift' %} when using the <span style="border-bottom:1px dotted #ccc;">Straight Line Selection Tool</span>. See the [ImageJ User Guide](https://imagej.nih.gov/ij/docs/guide/) for the full list of key modifiers that can be used while creating straight line ROIs.

</dd>
</dl>
<li markdown="1">

<span id="faq:z-position"></span>**With 3D and 4D images, how do I set the Z-position and of the center?**

</li>
<dl>
<dd markdown="1">

The Z-position (depth) of the center of analysis is the active Z-slice of the stack. With multichannel (composite) images, the active channel also defines the C-position. Both are reported in the *Sholl Results* table.

</dd>
</dl>
<li markdown="1">

<span id="faq:saving"></span>**I cannot see the option to save the results. Why?**

</li>
<dl>
<dd markdown="1">

The image you are trying to analyze is not saved locally. Saving it to a local directory (e.g., your Desktop or Home folder) should re-enable it.

</dd>
</dl>
<li markdown="1">

<span id="faq:parameters"></span>**Why so many parameters?**

</li>
<dl>
<dd markdown="1">

The plugin is designed for the analysis of a wide diversity of arbors and it is not biased to any particular cell type. The only way to ensure this broad applicability is to give users full control over the mathematical techniques used by the plugin to analyze sampled data.

</dd>
</dl>
</ol>

**Results**

<ol>
<li markdown="1">

<span id="faq:Sholl-table"></span>**How can I save/edit the *Sholl Results* table?**

</li>
<dl>
<dd markdown="1">

Select the table, then choose {% include bc path="File | Save As..." %}The filename extension can be specified using the *More » Options...* command (see the [ImageJ User Guide](https://imagej.nih.gov/ij/docs/guide/) for details). Single cells cannot be modified from within ImageJ, but custom extensions (e.g., .csv, .xls or .ods) will allow the table to be imported by other spreadsheet applications.

</dd>
</dl>
<li markdown="1">

<span id="faq:precision"></span>**Can I modify the way data is displayed?**

</li>
<dl>
<dd markdown="1">

Mostly, using {% include bc path='Analysis|Sholl|Metrics & Options'%} (also listed in the *More » Options...* shortcut), including the number of decimal places reported by the *Sholl Results* table or the usage of scientific notation. To resize plots, use the *More »* dropdown menu in the *Options* dialog.

</dd>
</dl>
<li markdown="1">

<span id="faq:sholl-mask"></span>**What is the *Sholl mask*?**

</li>
<dl>
<dd markdown="1">

The Sholl mask ([see example of CA1 cell](#ca1-cell-mask)) is simply an illustration: a maximum intensity projection of the analyzed cell in which [intersection counts](##output-options) are used as pixel intensities. As explained in [Output Options](#output-options), its LUT can be modified, and intensities calibrated using ImageJ default commands. As mentioned, it can also be used to visually inspect for [segmentation artifacts](#cf-segmentation).

</dd>
</dl>
<li markdown="1">

<span id="faq:3Dvs2D"></span>**The 3D profile looks *worse* than the 2D profile of the Maximum Intensity Projection of the same cell. Why?**

</li>
<dl>
<dd markdown="1">

An anisotropic voxel size will have a strong impact on [step size](#step-size). On the other hand, 2D and 3D images can be sampled differently depending on the [options chosen](#parameters). If {% include bc path="Image | Properties..." %} ({% include key keys='Shift|P' %}) reports the appropriate spatial calibration, make sure to read [Multiple Samples and Noise Reduction](#multiple-samples-and-noise-reduction) before deciding which type of images to use.

</dd>
</dl>
<li markdown="1">

<span id="faq:no-output"></span>**The program terminates without warnings. What am I doing wrong?**

</li>
<dl>
<dd markdown="1">

The program should not terminate without throwing an error message. However, do note that some exiting messages are displayed in the often overlooked status bar of the main ImageJ window. This is intentional, as it minimizes the frequency of modal windows popping up for each failed operation.

</dd>
</dl>
</ol>

**Metrics and Curve Fitting**
<ol>
<li markdown="1">

<span id="faq:AUC"></span>**Would it be possible to retrieve the Area Under the Curve (linear Sholl plot)?**

</li>
<dl>
<dd markdown="1">

Sure. But it would hardly be relevant for data sampled at fixed intervals. The area under the curve (AUC, the area between the sampled curve and the horizontal axis, i.e., its definite integral) could be estimated using, e.g., the {% include wikipedia title='Trapezoidal rule' text='trapezoidal rule'%}. However, because data is always sampled at equally spaced intervals, doing so would be the same as multiplying [Mean inters.](#mean-inters) by the distance between [Ending radius](#end-radius) and [Starting radius](#start-radius). Thus, effectively, AUC is redundant with [Mean inters.](#mean-inters), that is already an integrated measurement of the sampled data. On the other hand, one could retrieve the AUC of the polynomial fit, but such property is already covered by [Mean value](#mean-value-of-function).

</dd>
</dl>
<li markdown="1">

<span id="faq:inflection-points"></span>**The shape of the polynomial changes at the edges of the profile. Why?**

</li>
<dl>
<dd markdown="1">

Inflection points at [starting/ending radius](#start-radius) are usually associated with a poor fit and/or the fact that all the radii in which no intersections were counted are ignored. The latter is required to calculate the [Sholl regression coefficient](#sholl-decay), as *log(0)* is undefined.

</dd>
</dl>
<li markdown="1">

<span id="faq:poor-fitting"></span>**None of the fitting options is suitable for my datasets. What should I do?**

</li>
<dl>
<dd markdown="1">

Have a look at [Complementary Tools](#complementary-tools).

</dd>
</dl>
</ol>

**Batch Processing**
<ol>
<li markdown="1">

<span id="faq:recorder"></span>**The code that the Macro Recorder produced does not seem to work. What am I doing wrong?**

</li>
<dl>
<dd markdown="1">

It is likely that frequent interactions with the dialog prompt(s) (from which the Recorder retrieves user-specified parameters) have "confused" ImageJ. While this process is usually flawless, it may happen that repeated triggering of GUI-specific commands that are not recordable (e.g., *[Cf. Segmentation](#cf-segmentation)* or *[Import Other Data](#importing)* buttons) may lead to an incomplete recording. The solution is to repeat the recording, while minimizing such interactions.

</dd>
</dl>
</ol>

**Development**
<ol>
<li markdown="1">

<span id="faq:bug-report"></span>**I found a bug. How do I report it?**

</li>
<dl>
<dd markdown="1">
Report it in the [ImageJ Forum](http://forum.imagej.net).
</dd>
</dl>
</ol>

## Release Notes

Release notes are available on the {% include github org='morphonets' repo='SNT' label='SNT repository' %}. Releases notes up to version 3.7.3 are available on the {% include github org='tferr' repo='ASA' branch='master' path='Notes.md#release-notes-for-sholl-analysis' label='initial project repository' %}.


## Related Resources

- [SNT](/plugins/snt)

- [List of ImageJ extensions](/list-of-extensions#neuroanatomy), which you can filter by the Neuroanatomy category. These include [NeuronJ](/plugins/neuronj), an alternative to SNT. NeuronJ features a friendly interface but is restricted to 2D images, is not capable of SWC export and is no longer under active development

- [Neuroanatomy update site](/update-sites/neuroanatomy), distributing SNT, Sholl and related plugins, namely [Strahler_Analysis](/plugins/strahler-analysis)


## Publication

{% include citation %}

*Citting manuscripts can be retrieved using [Scholar](https://scholar.google.com/scholar?cites=15441574333602897335&as_sdt=2005&sciodt=1,5&hl=en) or [PubMed](http://www.ncbi.nlm.nih.gov/pmc/articles/pmid/25264773/citedby/?tool=pubmed), although many [more publications seem to be using it](https://scholar.google.com/scholar?hl=en&as_sdt=1%2C5&as_vis=1&q=fiji+AND+%22Sholl+Analysis%22&btnG=). See [here](/plugins/snt/faq#how-do-i-cite-snt) for details on how to cite SNT.*


## References

[^2]: Sholl DA. Dendritic organization in the neurons of the visual and motor cortices of the cat. J Anat. 1953 Oct;87(4):387-406. [PMID: 13117757](http://www.ncbi.nlm.nih.gov/pubmed?term=13117757)

{% include citation fn=3 doi='10.1016/j.jneumeth.2006.05.030' %}

{% include citation fn=4 doi='10.1016/0306-4522(82)90120-8' %}

{% include citation fn=5 doi='10.1016/j.jtbi.2006.09.022' %}

{% include citation fn=6 doi='10.1007/s12021-010-9095-5' %}
