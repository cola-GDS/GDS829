cola Report for GDS829
==================

**Date**: 2019-12-25 22:20:05 CET, **cola version**: 1.3.2

----------------------------------------------------------------

<style type='text/css'>

body, td, th {
   font-family: Arial,Helvetica,sans-serif;
   background-color: white;
   font-size: 13px;
  max-width: 800px;
  margin: auto;
  margin-left:210px;
  padding: 0px 10px 0px 10px;
  border-left: 1px solid #EEEEEE;
  line-height: 150%;
}

tt, code, pre {
   font-family: 'DejaVu Sans Mono', 'Droid Sans Mono', 'Lucida Console', Consolas, Monaco, 

monospace;
}

h1 {
   font-size:2.2em;
}

h2 {
   font-size:1.8em;
}

h3 {
   font-size:1.4em;
}

h4 {
   font-size:1.0em;
}

h5 {
   font-size:0.9em;
}

h6 {
   font-size:0.8em;
}

a {
  text-decoration: none;
  color: #0366d6;
}

a:hover {
  text-decoration: underline;
}

a:visited {
   color: #0366d6;
}

pre, img {
  max-width: 100%;
}
pre {
  overflow-x: auto;
}
pre code {
   display: block; padding: 0.5em;
}

code {
  font-size: 92%;
  border: 1px solid #ccc;
}

code[class] {
  background-color: #F8F8F8;
}

table, td, th {
  border: 1px solid #ccc;
}

blockquote {
   color:#666666;
   margin:0;
   padding-left: 1em;
   border-left: 0.5em #EEE solid;
}

hr {
   height: 0px;
   border-bottom: none;
   border-top-width: thin;
   border-top-style: dotted;
   border-top-color: #999999;
}

@media print {
   * {
      background: transparent !important;
      color: black !important;
      filter:none !important;
      -ms-filter: none !important;
   }

   body {
      font-size:12pt;
      max-width:100%;
   }

   a, a:visited {
      text-decoration: underline;
   }

   hr {
      visibility: hidden;
      page-break-before: always;
   }

   pre, blockquote {
      padding-right: 1em;
      page-break-inside: avoid;
   }

   tr, img {
      page-break-inside: avoid;
   }

   img {
      max-width: 100% !important;
   }

   @page :left {
      margin: 15mm 20mm 15mm 10mm;
   }

   @page :right {
      margin: 15mm 10mm 15mm 20mm;
   }

   p, h2, h3 {
      orphans: 3; widows: 3;
   }

   h2, h3 {
      page-break-after: avoid;
   }
}
</style>




## Summary





All available functions which can be applied to this `res_list` object:


```r
res_list
```

```
#> A 'ConsensusPartitionList' object with 24 methods.
#>   On a matrix with 21452 rows and 54 columns.
#>   Top rows are extracted by 'SD, CV, MAD, ATC' methods.
#>   Subgroups are detected by 'hclust, kmeans, skmeans, pam, mclust, NMF' method.
#>   Number of partitions are tried for k = 2, 3, 4, 5, 6.
#>   Performed in total 30000 partitions by row resampling.
#> 
#> Following methods can be applied to this 'ConsensusPartitionList' object:
#>  [1] "cola_report"           "collect_classes"       "collect_plots"         "collect_stats"        
#>  [5] "colnames"              "functional_enrichment" "get_anno_col"          "get_anno"             
#>  [9] "get_classes"           "get_matrix"            "get_membership"        "get_stats"            
#> [13] "is_best_k"             "is_stable_k"           "ncol"                  "nrow"                 
#> [17] "rownames"              "show"                  "suggest_best_k"        "test_to_known_factors"
#> [21] "top_rows_heatmap"      "top_rows_overlap"     
#> 
#> You can get result for a single method by, e.g. object["SD", "hclust"] or object["SD:hclust"]
#> or a subset of methods by object[c("SD", "CV")], c("hclust", "kmeans")]
```

The call of `run_all_consensus_partition_methods()` was:


```
#> run_all_consensus_partition_methods(data = mat, mc.cores = 4, anno = anno)
```

Dimension of the input matrix:


```r
mat = get_matrix(res_list)
dim(mat)
```

```
#> [1] 21452    54
```

### Density distribution

The density distribution for each sample is visualized as in one column in the
following heatmap. The clustering is based on the distance which is the
Kolmogorov-Smirnov statistic between two distributions.




```r
library(ComplexHeatmap)
densityHeatmap(mat, top_annotation = HeatmapAnnotation(df = get_anno(res_list), 
    col = get_anno_col(res_list)), ylab = "value", cluster_columns = TRUE, show_column_names = FALSE,
    mc.cores = 4)
```

![plot of chunk density-heatmap](figure_cola/density-heatmap-1.png)





### Suggest the best k



Folowing table shows the best `k` (number of partitions) for each combination
of top-value methods and partition methods. Clicking on the method name in
the table goes to the section for a single combination of methods.

[The cola vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13)
explains the definition of the metrics used for determining the best
number of partitions.


```r
suggest_best_k(res_list)
```


|                            | The best k| 1-PAC| Mean silhouette| Concordance|   |Optional k |
|:---------------------------|----------:|-----:|---------------:|-----------:|:--|:----------|
|[SD:mclust](#SD-mclust)     |          3| 1.000|           0.981|       0.990|** |           |
|[MAD:mclust](#MAD-mclust)   |          3| 1.000|           0.951|       0.980|** |           |
|[ATC:kmeans](#ATC-kmeans)   |          3| 1.000|           1.000|       1.000|** |           |
|[ATC:mclust](#ATC-mclust)   |          3| 1.000|           0.989|       0.994|** |           |
|[MAD:kmeans](#MAD-kmeans)   |          3| 0.997|           0.938|       0.968|** |           |
|[MAD:skmeans](#MAD-skmeans) |          4| 0.973|           0.945|       0.972|** |2,3        |
|[SD:NMF](#SD-NMF)           |          3| 0.971|           0.928|       0.975|** |           |
|[ATC:hclust](#ATC-hclust)   |          5| 0.961|           0.947|       0.976|** |2,3        |
|[SD:skmeans](#SD-skmeans)   |          4| 0.943|           0.928|       0.965|*  |2,3        |
|[ATC:skmeans](#ATC-skmeans) |          4| 0.941|           0.921|       0.964|*  |2,3        |
|[ATC:NMF](#ATC-NMF)         |          3| 0.941|           0.937|       0.974|*  |           |
|[MAD:NMF](#MAD-NMF)         |          4| 0.931|           0.898|       0.947|*  |3          |
|[CV:NMF](#CV-NMF)           |          4| 0.916|           0.894|       0.949|*  |3          |
|[SD:pam](#SD-pam)           |          3| 0.915|           0.941|       0.975|*  |2          |
|[SD:kmeans](#SD-kmeans)     |          3| 0.915|           0.957|       0.982|*  |           |
|[ATC:pam](#ATC-pam)         |          4| 0.909|           0.918|       0.964|*  |2,3        |
|[CV:pam](#CV-pam)           |          6| 0.905|           0.887|       0.931|*  |           |
|[CV:skmeans](#CV-skmeans)   |          4| 0.900|           0.865|       0.944|*  |3          |
|[CV:kmeans](#CV-kmeans)     |          3| 0.861|           0.910|       0.959|   |           |
|[MAD:hclust](#MAD-hclust)   |          4| 0.821|           0.792|       0.913|   |           |
|[CV:mclust](#CV-mclust)     |          3| 0.791|           0.902|       0.947|   |           |
|[MAD:pam](#MAD-pam)         |          2| 0.762|           0.942|       0.972|   |           |
|[SD:hclust](#SD-hclust)     |          4| 0.747|           0.789|       0.897|   |           |
|[CV:hclust](#CV-hclust)     |          4| 0.695|           0.792|       0.883|   |           |

\*\*: 1-PAC > 0.95, \*: 1-PAC > 0.9




### CDF of consensus matrices

Cumulative distribution function curves of consensus matrix for all methods.




```r
collect_plots(res_list, fun = plot_ecdf)
```

![plot of chunk collect-plots](figure_cola/collect-plots-1.png)



### Consensus heatmap

Consensus heatmaps for all methods. ([What is a consensus heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_9))


<style type='text/css'>



.ui-helper-hidden {
	display: none;
}
.ui-helper-hidden-accessible {
	border: 0;
	clip: rect(0 0 0 0);
	height: 1px;
	margin: -1px;
	overflow: hidden;
	padding: 0;
	position: absolute;
	width: 1px;
}
.ui-helper-reset {
	margin: 0;
	padding: 0;
	border: 0;
	outline: 0;
	line-height: 1.3;
	text-decoration: none;
	font-size: 100%;
	list-style: none;
}
.ui-helper-clearfix:before,
.ui-helper-clearfix:after {
	content: "";
	display: table;
	border-collapse: collapse;
}
.ui-helper-clearfix:after {
	clear: both;
}
.ui-helper-zfix {
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	position: absolute;
	opacity: 0;
	filter:Alpha(Opacity=0); 
}

.ui-front {
	z-index: 100;
}



.ui-state-disabled {
	cursor: default !important;
	pointer-events: none;
}



.ui-icon {
	display: inline-block;
	vertical-align: middle;
	margin-top: -.25em;
	position: relative;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
}

.ui-widget-icon-block {
	left: 50%;
	margin-left: -8px;
	display: block;
}




.ui-widget-overlay {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
}
.ui-accordion .ui-accordion-header {
	display: block;
	cursor: pointer;
	position: relative;
	margin: 2px 0 0 0;
	padding: .5em .5em .5em .7em;
	font-size: 100%;
}
.ui-accordion .ui-accordion-content {
	padding: 1em 2.2em;
	border-top: 0;
	overflow: auto;
}
.ui-autocomplete {
	position: absolute;
	top: 0;
	left: 0;
	cursor: default;
}
.ui-menu {
	list-style: none;
	padding: 0;
	margin: 0;
	display: block;
	outline: 0;
}
.ui-menu .ui-menu {
	position: absolute;
}
.ui-menu .ui-menu-item {
	margin: 0;
	cursor: pointer;
	
	list-style-image: url("data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7");
}
.ui-menu .ui-menu-item-wrapper {
	position: relative;
	padding: 3px 1em 3px .4em;
}
.ui-menu .ui-menu-divider {
	margin: 5px 0;
	height: 0;
	font-size: 0;
	line-height: 0;
	border-width: 1px 0 0 0;
}
.ui-menu .ui-state-focus,
.ui-menu .ui-state-active {
	margin: -1px;
}


.ui-menu-icons {
	position: relative;
}
.ui-menu-icons .ui-menu-item-wrapper {
	padding-left: 2em;
}


.ui-menu .ui-icon {
	position: absolute;
	top: 0;
	bottom: 0;
	left: .2em;
	margin: auto 0;
}


.ui-menu .ui-menu-icon {
	left: auto;
	right: 0;
}
.ui-button {
	padding: .4em 1em;
	display: inline-block;
	position: relative;
	line-height: normal;
	margin-right: .1em;
	cursor: pointer;
	vertical-align: middle;
	text-align: center;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;

	
	overflow: visible;
}

.ui-button,
.ui-button:link,
.ui-button:visited,
.ui-button:hover,
.ui-button:active {
	text-decoration: none;
}


.ui-button-icon-only {
	width: 2em;
	box-sizing: border-box;
	text-indent: -9999px;
	white-space: nowrap;
}


input.ui-button.ui-button-icon-only {
	text-indent: 0;
}


.ui-button-icon-only .ui-icon {
	position: absolute;
	top: 50%;
	left: 50%;
	margin-top: -8px;
	margin-left: -8px;
}

.ui-button.ui-icon-notext .ui-icon {
	padding: 0;
	width: 2.1em;
	height: 2.1em;
	text-indent: -9999px;
	white-space: nowrap;

}

input.ui-button.ui-icon-notext .ui-icon {
	width: auto;
	height: auto;
	text-indent: 0;
	white-space: normal;
	padding: .4em 1em;
}



input.ui-button::-moz-focus-inner,
button.ui-button::-moz-focus-inner {
	border: 0;
	padding: 0;
}
.ui-controlgroup {
	vertical-align: middle;
	display: inline-block;
}
.ui-controlgroup > .ui-controlgroup-item {
	float: left;
	margin-left: 0;
	margin-right: 0;
}
.ui-controlgroup > .ui-controlgroup-item:focus,
.ui-controlgroup > .ui-controlgroup-item.ui-visual-focus {
	z-index: 9999;
}
.ui-controlgroup-vertical > .ui-controlgroup-item {
	display: block;
	float: none;
	width: 100%;
	margin-top: 0;
	margin-bottom: 0;
	text-align: left;
}
.ui-controlgroup-vertical .ui-controlgroup-item {
	box-sizing: border-box;
}
.ui-controlgroup .ui-controlgroup-label {
	padding: .4em 1em;
}
.ui-controlgroup .ui-controlgroup-label span {
	font-size: 80%;
}
.ui-controlgroup-horizontal .ui-controlgroup-label + .ui-controlgroup-item {
	border-left: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label + .ui-controlgroup-item {
	border-top: none;
}
.ui-controlgroup-horizontal .ui-controlgroup-label.ui-widget-content {
	border-right: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label.ui-widget-content {
	border-bottom: none;
}


.ui-controlgroup-vertical .ui-spinner-input {

	
	width: 75%;
	width: calc( 100% - 2.4em );
}
.ui-controlgroup-vertical .ui-spinner .ui-spinner-up {
	border-top-style: solid;
}

.ui-checkboxradio-label .ui-icon-background {
	box-shadow: inset 1px 1px 1px #ccc;
	border-radius: .12em;
	border: none;
}
.ui-checkboxradio-radio-label .ui-icon-background {
	width: 16px;
	height: 16px;
	border-radius: 1em;
	overflow: visible;
	border: none;
}
.ui-checkboxradio-radio-label.ui-checkboxradio-checked .ui-icon,
.ui-checkboxradio-radio-label.ui-checkboxradio-checked:hover .ui-icon {
	background-image: none;
	width: 8px;
	height: 8px;
	border-width: 4px;
	border-style: solid;
}
.ui-checkboxradio-disabled {
	pointer-events: none;
}
.ui-datepicker {
	width: 17em;
	padding: .2em .2em 0;
	display: none;
}
.ui-datepicker .ui-datepicker-header {
	position: relative;
	padding: .2em 0;
}
.ui-datepicker .ui-datepicker-prev,
.ui-datepicker .ui-datepicker-next {
	position: absolute;
	top: 2px;
	width: 1.8em;
	height: 1.8em;
}
.ui-datepicker .ui-datepicker-prev-hover,
.ui-datepicker .ui-datepicker-next-hover {
	top: 1px;
}
.ui-datepicker .ui-datepicker-prev {
	left: 2px;
}
.ui-datepicker .ui-datepicker-next {
	right: 2px;
}
.ui-datepicker .ui-datepicker-prev-hover {
	left: 1px;
}
.ui-datepicker .ui-datepicker-next-hover {
	right: 1px;
}
.ui-datepicker .ui-datepicker-prev span,
.ui-datepicker .ui-datepicker-next span {
	display: block;
	position: absolute;
	left: 50%;
	margin-left: -8px;
	top: 50%;
	margin-top: -8px;
}
.ui-datepicker .ui-datepicker-title {
	margin: 0 2.3em;
	line-height: 1.8em;
	text-align: center;
}
.ui-datepicker .ui-datepicker-title select {
	font-size: 1em;
	margin: 1px 0;
}
.ui-datepicker select.ui-datepicker-month,
.ui-datepicker select.ui-datepicker-year {
	width: 45%;
}
.ui-datepicker table {
	width: 100%;
	font-size: .9em;
	border-collapse: collapse;
	margin: 0 0 .4em;
}
.ui-datepicker th {
	padding: .7em .3em;
	text-align: center;
	font-weight: bold;
	border: 0;
}
.ui-datepicker td {
	border: 0;
	padding: 1px;
}
.ui-datepicker td span,
.ui-datepicker td a {
	display: block;
	padding: .2em;
	text-align: right;
	text-decoration: none;
}
.ui-datepicker .ui-datepicker-buttonpane {
	background-image: none;
	margin: .7em 0 0 0;
	padding: 0 .2em;
	border-left: 0;
	border-right: 0;
	border-bottom: 0;
}
.ui-datepicker .ui-datepicker-buttonpane button {
	float: right;
	margin: .5em .2em .4em;
	cursor: pointer;
	padding: .2em .6em .3em .6em;
	width: auto;
	overflow: visible;
}
.ui-datepicker .ui-datepicker-buttonpane button.ui-datepicker-current {
	float: left;
}


.ui-datepicker.ui-datepicker-multi {
	width: auto;
}
.ui-datepicker-multi .ui-datepicker-group {
	float: left;
}
.ui-datepicker-multi .ui-datepicker-group table {
	width: 95%;
	margin: 0 auto .4em;
}
.ui-datepicker-multi-2 .ui-datepicker-group {
	width: 50%;
}
.ui-datepicker-multi-3 .ui-datepicker-group {
	width: 33.3%;
}
.ui-datepicker-multi-4 .ui-datepicker-group {
	width: 25%;
}
.ui-datepicker-multi .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-multi .ui-datepicker-group-middle .ui-datepicker-header {
	border-left-width: 0;
}
.ui-datepicker-multi .ui-datepicker-buttonpane {
	clear: left;
}
.ui-datepicker-row-break {
	clear: both;
	width: 100%;
	font-size: 0;
}


.ui-datepicker-rtl {
	direction: rtl;
}
.ui-datepicker-rtl .ui-datepicker-prev {
	right: 2px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next {
	left: 2px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-prev:hover {
	right: 1px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next:hover {
	left: 1px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane {
	clear: right;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button {
	float: left;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button.ui-datepicker-current,
.ui-datepicker-rtl .ui-datepicker-group {
	float: right;
}
.ui-datepicker-rtl .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-rtl .ui-datepicker-group-middle .ui-datepicker-header {
	border-right-width: 0;
	border-left-width: 1px;
}


.ui-datepicker .ui-icon {
	display: block;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
	left: .5em;
	top: .3em;
}
.ui-dialog {
	position: absolute;
	top: 0;
	left: 0;
	padding: .2em;
	outline: 0;
}
.ui-dialog .ui-dialog-titlebar {
	padding: .4em 1em;
	position: relative;
}
.ui-dialog .ui-dialog-title {
	float: left;
	margin: .1em 0;
	white-space: nowrap;
	width: 90%;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-dialog .ui-dialog-titlebar-close {
	position: absolute;
	right: .3em;
	top: 50%;
	width: 20px;
	margin: -10px 0 0 0;
	padding: 1px;
	height: 20px;
}
.ui-dialog .ui-dialog-content {
	position: relative;
	border: 0;
	padding: .5em 1em;
	background: none;
	overflow: auto;
}
.ui-dialog .ui-dialog-buttonpane {
	text-align: left;
	border-width: 1px 0 0 0;
	background-image: none;
	margin-top: .5em;
	padding: .3em 1em .5em .4em;
}
.ui-dialog .ui-dialog-buttonpane .ui-dialog-buttonset {
	float: right;
}
.ui-dialog .ui-dialog-buttonpane button {
	margin: .5em .4em .5em 0;
	cursor: pointer;
}
.ui-dialog .ui-resizable-n {
	height: 2px;
	top: 0;
}
.ui-dialog .ui-resizable-e {
	width: 2px;
	right: 0;
}
.ui-dialog .ui-resizable-s {
	height: 2px;
	bottom: 0;
}
.ui-dialog .ui-resizable-w {
	width: 2px;
	left: 0;
}
.ui-dialog .ui-resizable-se,
.ui-dialog .ui-resizable-sw,
.ui-dialog .ui-resizable-ne,
.ui-dialog .ui-resizable-nw {
	width: 7px;
	height: 7px;
}
.ui-dialog .ui-resizable-se {
	right: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-sw {
	left: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-ne {
	right: 0;
	top: 0;
}
.ui-dialog .ui-resizable-nw {
	left: 0;
	top: 0;
}
.ui-draggable .ui-dialog-titlebar {
	cursor: move;
}
.ui-draggable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable {
	position: relative;
}
.ui-resizable-handle {
	position: absolute;
	font-size: 0.1px;
	display: block;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable-disabled .ui-resizable-handle,
.ui-resizable-autohide .ui-resizable-handle {
	display: none;
}
.ui-resizable-n {
	cursor: n-resize;
	height: 7px;
	width: 100%;
	top: -5px;
	left: 0;
}
.ui-resizable-s {
	cursor: s-resize;
	height: 7px;
	width: 100%;
	bottom: -5px;
	left: 0;
}
.ui-resizable-e {
	cursor: e-resize;
	width: 7px;
	right: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-w {
	cursor: w-resize;
	width: 7px;
	left: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-se {
	cursor: se-resize;
	width: 12px;
	height: 12px;
	right: 1px;
	bottom: 1px;
}
.ui-resizable-sw {
	cursor: sw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	bottom: -5px;
}
.ui-resizable-nw {
	cursor: nw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	top: -5px;
}
.ui-resizable-ne {
	cursor: ne-resize;
	width: 9px;
	height: 9px;
	right: -5px;
	top: -5px;
}
.ui-progressbar {
	height: 2em;
	text-align: left;
	overflow: hidden;
}
.ui-progressbar .ui-progressbar-value {
	margin: -1px;
	height: 100%;
}
.ui-progressbar .ui-progressbar-overlay {
	background: url("data:image/gif;base64,R0lGODlhKAAoAIABAAAAAP///yH/C05FVFNDQVBFMi4wAwEAAAAh+QQJAQABACwAAAAAKAAoAAACkYwNqXrdC52DS06a7MFZI+4FHBCKoDeWKXqymPqGqxvJrXZbMx7Ttc+w9XgU2FB3lOyQRWET2IFGiU9m1frDVpxZZc6bfHwv4c1YXP6k1Vdy292Fb6UkuvFtXpvWSzA+HycXJHUXiGYIiMg2R6W459gnWGfHNdjIqDWVqemH2ekpObkpOlppWUqZiqr6edqqWQAAIfkECQEAAQAsAAAAACgAKAAAApSMgZnGfaqcg1E2uuzDmmHUBR8Qil95hiPKqWn3aqtLsS18y7G1SzNeowWBENtQd+T1JktP05nzPTdJZlR6vUxNWWjV+vUWhWNkWFwxl9VpZRedYcflIOLafaa28XdsH/ynlcc1uPVDZxQIR0K25+cICCmoqCe5mGhZOfeYSUh5yJcJyrkZWWpaR8doJ2o4NYq62lAAACH5BAkBAAEALAAAAAAoACgAAAKVDI4Yy22ZnINRNqosw0Bv7i1gyHUkFj7oSaWlu3ovC8GxNso5fluz3qLVhBVeT/Lz7ZTHyxL5dDalQWPVOsQWtRnuwXaFTj9jVVh8pma9JjZ4zYSj5ZOyma7uuolffh+IR5aW97cHuBUXKGKXlKjn+DiHWMcYJah4N0lYCMlJOXipGRr5qdgoSTrqWSq6WFl2ypoaUAAAIfkECQEAAQAsAAAAACgAKAAAApaEb6HLgd/iO7FNWtcFWe+ufODGjRfoiJ2akShbueb0wtI50zm02pbvwfWEMWBQ1zKGlLIhskiEPm9R6vRXxV4ZzWT2yHOGpWMyorblKlNp8HmHEb/lCXjcW7bmtXP8Xt229OVWR1fod2eWqNfHuMjXCPkIGNileOiImVmCOEmoSfn3yXlJWmoHGhqp6ilYuWYpmTqKUgAAIfkECQEAAQAsAAAAACgAKAAAApiEH6kb58biQ3FNWtMFWW3eNVcojuFGfqnZqSebuS06w5V80/X02pKe8zFwP6EFWOT1lDFk8rGERh1TTNOocQ61Hm4Xm2VexUHpzjymViHrFbiELsefVrn6XKfnt2Q9G/+Xdie499XHd2g4h7ioOGhXGJboGAnXSBnoBwKYyfioubZJ2Hn0RuRZaflZOil56Zp6iioKSXpUAAAh+QQJAQABACwAAAAAKAAoAAACkoQRqRvnxuI7kU1a1UU5bd5tnSeOZXhmn5lWK3qNTWvRdQxP8qvaC+/yaYQzXO7BMvaUEmJRd3TsiMAgswmNYrSgZdYrTX6tSHGZO73ezuAw2uxuQ+BbeZfMxsexY35+/Qe4J1inV0g4x3WHuMhIl2jXOKT2Q+VU5fgoSUI52VfZyfkJGkha6jmY+aaYdirq+lQAACH5BAkBAAEALAAAAAAoACgAAAKWBIKpYe0L3YNKToqswUlvznigd4wiR4KhZrKt9Upqip61i9E3vMvxRdHlbEFiEXfk9YARYxOZZD6VQ2pUunBmtRXo1Lf8hMVVcNl8JafV38aM2/Fu5V16Bn63r6xt97j09+MXSFi4BniGFae3hzbH9+hYBzkpuUh5aZmHuanZOZgIuvbGiNeomCnaxxap2upaCZsq+1kAACH5BAkBAAEALAAAAAAoACgAAAKXjI8By5zf4kOxTVrXNVlv1X0d8IGZGKLnNpYtm8Lr9cqVeuOSvfOW79D9aDHizNhDJidFZhNydEahOaDH6nomtJjp1tutKoNWkvA6JqfRVLHU/QUfau9l2x7G54d1fl995xcIGAdXqMfBNadoYrhH+Mg2KBlpVpbluCiXmMnZ2Sh4GBqJ+ckIOqqJ6LmKSllZmsoq6wpQAAAh+QQJAQABACwAAAAAKAAoAAAClYx/oLvoxuJDkU1a1YUZbJ59nSd2ZXhWqbRa2/gF8Gu2DY3iqs7yrq+xBYEkYvFSM8aSSObE+ZgRl1BHFZNr7pRCavZ5BW2142hY3AN/zWtsmf12p9XxxFl2lpLn1rseztfXZjdIWIf2s5dItwjYKBgo9yg5pHgzJXTEeGlZuenpyPmpGQoKOWkYmSpaSnqKileI2FAAACH5BAkBAAEALAAAAAAoACgAAAKVjB+gu+jG4kORTVrVhRlsnn2dJ3ZleFaptFrb+CXmO9OozeL5VfP99HvAWhpiUdcwkpBH3825AwYdU8xTqlLGhtCosArKMpvfa1mMRae9VvWZfeB2XfPkeLmm18lUcBj+p5dnN8jXZ3YIGEhYuOUn45aoCDkp16hl5IjYJvjWKcnoGQpqyPlpOhr3aElaqrq56Bq7VAAAOw==");
	height: 100%;
	filter: alpha(opacity=25); 
	opacity: 0.25;
}
.ui-progressbar-indeterminate .ui-progressbar-value {
	background-image: none;
}
.ui-selectable {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-selectable-helper {
	position: absolute;
	z-index: 100;
	border: 1px dotted black;
}
.ui-selectmenu-menu {
	padding: 0;
	margin: 0;
	position: absolute;
	top: 0;
	left: 0;
	display: none;
}
.ui-selectmenu-menu .ui-menu {
	overflow: auto;
	overflow-x: hidden;
	padding-bottom: 1px;
}
.ui-selectmenu-menu .ui-menu .ui-selectmenu-optgroup {
	font-size: 1em;
	font-weight: bold;
	line-height: 1.5;
	padding: 2px 0.4em;
	margin: 0.5em 0 0 0;
	height: auto;
	border: 0;
}
.ui-selectmenu-open {
	display: block;
}
.ui-selectmenu-text {
	display: block;
	margin-right: 20px;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-selectmenu-button.ui-button {
	text-align: left;
	white-space: nowrap;
	width: 14em;
}
.ui-selectmenu-icon.ui-icon {
	float: right;
	margin-top: 0;
}
.ui-slider {
	position: relative;
	text-align: left;
}
.ui-slider .ui-slider-handle {
	position: absolute;
	z-index: 2;
	width: 1.2em;
	height: 1.2em;
	cursor: default;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-slider .ui-slider-range {
	position: absolute;
	z-index: 1;
	font-size: .7em;
	display: block;
	border: 0;
	background-position: 0 0;
}


.ui-slider.ui-state-disabled .ui-slider-handle,
.ui-slider.ui-state-disabled .ui-slider-range {
	filter: inherit;
}

.ui-slider-horizontal {
	height: .8em;
}
.ui-slider-horizontal .ui-slider-handle {
	top: -.3em;
	margin-left: -.6em;
}
.ui-slider-horizontal .ui-slider-range {
	top: 0;
	height: 100%;
}
.ui-slider-horizontal .ui-slider-range-min {
	left: 0;
}
.ui-slider-horizontal .ui-slider-range-max {
	right: 0;
}

.ui-slider-vertical {
	width: .8em;
	height: 100px;
}
.ui-slider-vertical .ui-slider-handle {
	left: -.3em;
	margin-left: 0;
	margin-bottom: -.6em;
}
.ui-slider-vertical .ui-slider-range {
	left: 0;
	width: 100%;
}
.ui-slider-vertical .ui-slider-range-min {
	bottom: 0;
}
.ui-slider-vertical .ui-slider-range-max {
	top: 0;
}
.ui-sortable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-spinner {
	position: relative;
	display: inline-block;
	overflow: hidden;
	padding: 0;
	vertical-align: middle;
}
.ui-spinner-input {
	border: none;
	background: none;
	color: inherit;
	padding: .222em 0;
	margin: .2em 0;
	vertical-align: middle;
	margin-left: .4em;
	margin-right: 2em;
}
.ui-spinner-button {
	width: 1.6em;
	height: 50%;
	font-size: .5em;
	padding: 0;
	margin: 0;
	text-align: center;
	position: absolute;
	cursor: default;
	display: block;
	overflow: hidden;
	right: 0;
}

.ui-spinner a.ui-spinner-button {
	border-top-style: none;
	border-bottom-style: none;
	border-right-style: none;
}
.ui-spinner-up {
	top: 0;
}
.ui-spinner-down {
	bottom: 0;
}
.ui-tabs {
	position: relative;
	padding: .2em;
}
.ui-tabs .ui-tabs-nav {
	margin: 0;
	padding: .2em .2em 0;
}
.ui-tabs .ui-tabs-nav li {
	list-style: none;
	float: left;
	position: relative;
	top: 0;
	margin: 1px .2em 0 0;
	border-bottom-width: 0;
	padding: 0;
	white-space: nowrap;
}
.ui-tabs .ui-tabs-nav .ui-tabs-anchor {
	float: left;
	padding: .5em 1em;
	text-decoration: none;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active {
	margin-bottom: -1px;
	padding-bottom: 1px;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-state-disabled .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-tabs-loading .ui-tabs-anchor {
	cursor: text;
}
.ui-tabs-collapsible .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor {
	cursor: pointer;
}
.ui-tabs .ui-tabs-panel {
	display: block;
	border-width: 0;
	padding: 1em 1.4em;
	background: none;
}
.ui-tooltip {
	padding: 8px;
	position: absolute;
	z-index: 9999;
	max-width: 300px;
}
body .ui-tooltip {
	border-width: 2px;
}

.ui-widget {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget .ui-widget {
	font-size: 1em;
}
.ui-widget input,
.ui-widget select,
.ui-widget textarea,
.ui-widget button {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget.ui-widget-content {
	border: 1px solid #c5c5c5;
}
.ui-widget-content {
	border: 1px solid #dddddd;
	background: #ffffff;
	color: #333333;
}
.ui-widget-content a {
	color: #333333;
}
.ui-widget-header {
	border: 1px solid #dddddd;
	background: #e9e9e9;
	color: #333333;
	font-weight: bold;
}
.ui-widget-header a {
	color: #333333;
}


.ui-state-default,
.ui-widget-content .ui-state-default,
.ui-widget-header .ui-state-default,
.ui-button,


html .ui-button.ui-state-disabled:hover,
html .ui-button.ui-state-disabled:active {
	border: 1px solid #c5c5c5;
	background: #f6f6f6;
	font-weight: normal;
	color: #454545;
}
.ui-state-default a,
.ui-state-default a:link,
.ui-state-default a:visited,
a.ui-button,
a:link.ui-button,
a:visited.ui-button,
.ui-button {
	color: #454545;
	text-decoration: none;
}
.ui-state-hover,
.ui-widget-content .ui-state-hover,
.ui-widget-header .ui-state-hover,
.ui-state-focus,
.ui-widget-content .ui-state-focus,
.ui-widget-header .ui-state-focus,
.ui-button:hover,
.ui-button:focus {
	border: 1px solid #cccccc;
	background: #ededed;
	font-weight: normal;
	color: #2b2b2b;
}
.ui-state-hover a,
.ui-state-hover a:hover,
.ui-state-hover a:link,
.ui-state-hover a:visited,
.ui-state-focus a,
.ui-state-focus a:hover,
.ui-state-focus a:link,
.ui-state-focus a:visited,
a.ui-button:hover,
a.ui-button:focus {
	color: #2b2b2b;
	text-decoration: none;
}

.ui-visual-focus {
	box-shadow: 0 0 3px 1px rgb(94, 158, 214);
}
.ui-state-active,
.ui-widget-content .ui-state-active,
.ui-widget-header .ui-state-active,
a.ui-button:active,
.ui-button:active,
.ui-button.ui-state-active:hover {
	border: 1px solid #003eff;
	background: #007fff;
	font-weight: normal;
	color: #ffffff;
}
.ui-icon-background,
.ui-state-active .ui-icon-background {
	border: #003eff;
	background-color: #ffffff;
}
.ui-state-active a,
.ui-state-active a:link,
.ui-state-active a:visited {
	color: #ffffff;
	text-decoration: none;
}


.ui-state-highlight,
.ui-widget-content .ui-state-highlight,
.ui-widget-header .ui-state-highlight {
	border: 1px solid #dad55e;
	background: #fffa90;
	color: #777620;
}
.ui-state-checked {
	border: 1px solid #dad55e;
	background: #fffa90;
}
.ui-state-highlight a,
.ui-widget-content .ui-state-highlight a,
.ui-widget-header .ui-state-highlight a {
	color: #777620;
}
.ui-state-error,
.ui-widget-content .ui-state-error,
.ui-widget-header .ui-state-error {
	border: 1px solid #f1a899;
	background: #fddfdf;
	color: #5f3f3f;
}
.ui-state-error a,
.ui-widget-content .ui-state-error a,
.ui-widget-header .ui-state-error a {
	color: #5f3f3f;
}
.ui-state-error-text,
.ui-widget-content .ui-state-error-text,
.ui-widget-header .ui-state-error-text {
	color: #5f3f3f;
}
.ui-priority-primary,
.ui-widget-content .ui-priority-primary,
.ui-widget-header .ui-priority-primary {
	font-weight: bold;
}
.ui-priority-secondary,
.ui-widget-content .ui-priority-secondary,
.ui-widget-header .ui-priority-secondary {
	opacity: .7;
	filter:Alpha(Opacity=70); 
	font-weight: normal;
}
.ui-state-disabled,
.ui-widget-content .ui-state-disabled,
.ui-widget-header .ui-state-disabled {
	opacity: .35;
	filter:Alpha(Opacity=35); 
	background-image: none;
}
.ui-state-disabled .ui-icon {
	filter:Alpha(Opacity=35); 
}




.ui-icon {
	width: 16px;
	height: 16px;
}
.ui-icon,
.ui-widget-content .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-widget-header .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-state-hover .ui-icon,
.ui-state-focus .ui-icon,
.ui-button:hover .ui-icon,
.ui-button:focus .ui-icon {
	background-image: url("images/ui-icons_555555_256x240.png");
}
.ui-state-active .ui-icon,
.ui-button:active .ui-icon {
	background-image: url("images/ui-icons_ffffff_256x240.png");
}
.ui-state-highlight .ui-icon,
.ui-button .ui-state-highlight.ui-icon {
	background-image: url("images/ui-icons_777620_256x240.png");
}
.ui-state-error .ui-icon,
.ui-state-error-text .ui-icon {
	background-image: url("images/ui-icons_cc0000_256x240.png");
}
.ui-button .ui-icon {
	background-image: url("images/ui-icons_777777_256x240.png");
}


.ui-icon-blank { background-position: 16px 16px; }
.ui-icon-caret-1-n { background-position: 0 0; }
.ui-icon-caret-1-ne { background-position: -16px 0; }
.ui-icon-caret-1-e { background-position: -32px 0; }
.ui-icon-caret-1-se { background-position: -48px 0; }
.ui-icon-caret-1-s { background-position: -65px 0; }
.ui-icon-caret-1-sw { background-position: -80px 0; }
.ui-icon-caret-1-w { background-position: -96px 0; }
.ui-icon-caret-1-nw { background-position: -112px 0; }
.ui-icon-caret-2-n-s { background-position: -128px 0; }
.ui-icon-caret-2-e-w { background-position: -144px 0; }
.ui-icon-triangle-1-n { background-position: 0 -16px; }
.ui-icon-triangle-1-ne { background-position: -16px -16px; }
.ui-icon-triangle-1-e { background-position: -32px -16px; }
.ui-icon-triangle-1-se { background-position: -48px -16px; }
.ui-icon-triangle-1-s { background-position: -65px -16px; }
.ui-icon-triangle-1-sw { background-position: -80px -16px; }
.ui-icon-triangle-1-w { background-position: -96px -16px; }
.ui-icon-triangle-1-nw { background-position: -112px -16px; }
.ui-icon-triangle-2-n-s { background-position: -128px -16px; }
.ui-icon-triangle-2-e-w { background-position: -144px -16px; }
.ui-icon-arrow-1-n { background-position: 0 -32px; }
.ui-icon-arrow-1-ne { background-position: -16px -32px; }
.ui-icon-arrow-1-e { background-position: -32px -32px; }
.ui-icon-arrow-1-se { background-position: -48px -32px; }
.ui-icon-arrow-1-s { background-position: -65px -32px; }
.ui-icon-arrow-1-sw { background-position: -80px -32px; }
.ui-icon-arrow-1-w { background-position: -96px -32px; }
.ui-icon-arrow-1-nw { background-position: -112px -32px; }
.ui-icon-arrow-2-n-s { background-position: -128px -32px; }
.ui-icon-arrow-2-ne-sw { background-position: -144px -32px; }
.ui-icon-arrow-2-e-w { background-position: -160px -32px; }
.ui-icon-arrow-2-se-nw { background-position: -176px -32px; }
.ui-icon-arrowstop-1-n { background-position: -192px -32px; }
.ui-icon-arrowstop-1-e { background-position: -208px -32px; }
.ui-icon-arrowstop-1-s { background-position: -224px -32px; }
.ui-icon-arrowstop-1-w { background-position: -240px -32px; }
.ui-icon-arrowthick-1-n { background-position: 1px -48px; }
.ui-icon-arrowthick-1-ne { background-position: -16px -48px; }
.ui-icon-arrowthick-1-e { background-position: -32px -48px; }
.ui-icon-arrowthick-1-se { background-position: -48px -48px; }
.ui-icon-arrowthick-1-s { background-position: -64px -48px; }
.ui-icon-arrowthick-1-sw { background-position: -80px -48px; }
.ui-icon-arrowthick-1-w { background-position: -96px -48px; }
.ui-icon-arrowthick-1-nw { background-position: -112px -48px; }
.ui-icon-arrowthick-2-n-s { background-position: -128px -48px; }
.ui-icon-arrowthick-2-ne-sw { background-position: -144px -48px; }
.ui-icon-arrowthick-2-e-w { background-position: -160px -48px; }
.ui-icon-arrowthick-2-se-nw { background-position: -176px -48px; }
.ui-icon-arrowthickstop-1-n { background-position: -192px -48px; }
.ui-icon-arrowthickstop-1-e { background-position: -208px -48px; }
.ui-icon-arrowthickstop-1-s { background-position: -224px -48px; }
.ui-icon-arrowthickstop-1-w { background-position: -240px -48px; }
.ui-icon-arrowreturnthick-1-w { background-position: 0 -64px; }
.ui-icon-arrowreturnthick-1-n { background-position: -16px -64px; }
.ui-icon-arrowreturnthick-1-e { background-position: -32px -64px; }
.ui-icon-arrowreturnthick-1-s { background-position: -48px -64px; }
.ui-icon-arrowreturn-1-w { background-position: -64px -64px; }
.ui-icon-arrowreturn-1-n { background-position: -80px -64px; }
.ui-icon-arrowreturn-1-e { background-position: -96px -64px; }
.ui-icon-arrowreturn-1-s { background-position: -112px -64px; }
.ui-icon-arrowrefresh-1-w { background-position: -128px -64px; }
.ui-icon-arrowrefresh-1-n { background-position: -144px -64px; }
.ui-icon-arrowrefresh-1-e { background-position: -160px -64px; }
.ui-icon-arrowrefresh-1-s { background-position: -176px -64px; }
.ui-icon-arrow-4 { background-position: 0 -80px; }
.ui-icon-arrow-4-diag { background-position: -16px -80px; }
.ui-icon-extlink { background-position: -32px -80px; }
.ui-icon-newwin { background-position: -48px -80px; }
.ui-icon-refresh { background-position: -64px -80px; }
.ui-icon-shuffle { background-position: -80px -80px; }
.ui-icon-transfer-e-w { background-position: -96px -80px; }
.ui-icon-transferthick-e-w { background-position: -112px -80px; }
.ui-icon-folder-collapsed { background-position: 0 -96px; }
.ui-icon-folder-open { background-position: -16px -96px; }
.ui-icon-document { background-position: -32px -96px; }
.ui-icon-document-b { background-position: -48px -96px; }
.ui-icon-note { background-position: -64px -96px; }
.ui-icon-mail-closed { background-position: -80px -96px; }
.ui-icon-mail-open { background-position: -96px -96px; }
.ui-icon-suitcase { background-position: -112px -96px; }
.ui-icon-comment { background-position: -128px -96px; }
.ui-icon-person { background-position: -144px -96px; }
.ui-icon-print { background-position: -160px -96px; }
.ui-icon-trash { background-position: -176px -96px; }
.ui-icon-locked { background-position: -192px -96px; }
.ui-icon-unlocked { background-position: -208px -96px; }
.ui-icon-bookmark { background-position: -224px -96px; }
.ui-icon-tag { background-position: -240px -96px; }
.ui-icon-home { background-position: 0 -112px; }
.ui-icon-flag { background-position: -16px -112px; }
.ui-icon-calendar { background-position: -32px -112px; }
.ui-icon-cart { background-position: -48px -112px; }
.ui-icon-pencil { background-position: -64px -112px; }
.ui-icon-clock { background-position: -80px -112px; }
.ui-icon-disk { background-position: -96px -112px; }
.ui-icon-calculator { background-position: -112px -112px; }
.ui-icon-zoomin { background-position: -128px -112px; }
.ui-icon-zoomout { background-position: -144px -112px; }
.ui-icon-search { background-position: -160px -112px; }
.ui-icon-wrench { background-position: -176px -112px; }
.ui-icon-gear { background-position: -192px -112px; }
.ui-icon-heart { background-position: -208px -112px; }
.ui-icon-star { background-position: -224px -112px; }
.ui-icon-link { background-position: -240px -112px; }
.ui-icon-cancel { background-position: 0 -128px; }
.ui-icon-plus { background-position: -16px -128px; }
.ui-icon-plusthick { background-position: -32px -128px; }
.ui-icon-minus { background-position: -48px -128px; }
.ui-icon-minusthick { background-position: -64px -128px; }
.ui-icon-close { background-position: -80px -128px; }
.ui-icon-closethick { background-position: -96px -128px; }
.ui-icon-key { background-position: -112px -128px; }
.ui-icon-lightbulb { background-position: -128px -128px; }
.ui-icon-scissors { background-position: -144px -128px; }
.ui-icon-clipboard { background-position: -160px -128px; }
.ui-icon-copy { background-position: -176px -128px; }
.ui-icon-contact { background-position: -192px -128px; }
.ui-icon-image { background-position: -208px -128px; }
.ui-icon-video { background-position: -224px -128px; }
.ui-icon-script { background-position: -240px -128px; }
.ui-icon-alert { background-position: 0 -144px; }
.ui-icon-info { background-position: -16px -144px; }
.ui-icon-notice { background-position: -32px -144px; }
.ui-icon-help { background-position: -48px -144px; }
.ui-icon-check { background-position: -64px -144px; }
.ui-icon-bullet { background-position: -80px -144px; }
.ui-icon-radio-on { background-position: -96px -144px; }
.ui-icon-radio-off { background-position: -112px -144px; }
.ui-icon-pin-w { background-position: -128px -144px; }
.ui-icon-pin-s { background-position: -144px -144px; }
.ui-icon-play { background-position: 0 -160px; }
.ui-icon-pause { background-position: -16px -160px; }
.ui-icon-seek-next { background-position: -32px -160px; }
.ui-icon-seek-prev { background-position: -48px -160px; }
.ui-icon-seek-end { background-position: -64px -160px; }
.ui-icon-seek-start { background-position: -80px -160px; }

.ui-icon-seek-first { background-position: -80px -160px; }
.ui-icon-stop { background-position: -96px -160px; }
.ui-icon-eject { background-position: -112px -160px; }
.ui-icon-volume-off { background-position: -128px -160px; }
.ui-icon-volume-on { background-position: -144px -160px; }
.ui-icon-power { background-position: 0 -176px; }
.ui-icon-signal-diag { background-position: -16px -176px; }
.ui-icon-signal { background-position: -32px -176px; }
.ui-icon-battery-0 { background-position: -48px -176px; }
.ui-icon-battery-1 { background-position: -64px -176px; }
.ui-icon-battery-2 { background-position: -80px -176px; }
.ui-icon-battery-3 { background-position: -96px -176px; }
.ui-icon-circle-plus { background-position: 0 -192px; }
.ui-icon-circle-minus { background-position: -16px -192px; }
.ui-icon-circle-close { background-position: -32px -192px; }
.ui-icon-circle-triangle-e { background-position: -48px -192px; }
.ui-icon-circle-triangle-s { background-position: -64px -192px; }
.ui-icon-circle-triangle-w { background-position: -80px -192px; }
.ui-icon-circle-triangle-n { background-position: -96px -192px; }
.ui-icon-circle-arrow-e { background-position: -112px -192px; }
.ui-icon-circle-arrow-s { background-position: -128px -192px; }
.ui-icon-circle-arrow-w { background-position: -144px -192px; }
.ui-icon-circle-arrow-n { background-position: -160px -192px; }
.ui-icon-circle-zoomin { background-position: -176px -192px; }
.ui-icon-circle-zoomout { background-position: -192px -192px; }
.ui-icon-circle-check { background-position: -208px -192px; }
.ui-icon-circlesmall-plus { background-position: 0 -208px; }
.ui-icon-circlesmall-minus { background-position: -16px -208px; }
.ui-icon-circlesmall-close { background-position: -32px -208px; }
.ui-icon-squaresmall-plus { background-position: -48px -208px; }
.ui-icon-squaresmall-minus { background-position: -64px -208px; }
.ui-icon-squaresmall-close { background-position: -80px -208px; }
.ui-icon-grip-dotted-vertical { background-position: 0 -224px; }
.ui-icon-grip-dotted-horizontal { background-position: -16px -224px; }
.ui-icon-grip-solid-vertical { background-position: -32px -224px; }
.ui-icon-grip-solid-horizontal { background-position: -48px -224px; }
.ui-icon-gripsmall-diagonal-se { background-position: -64px -224px; }
.ui-icon-grip-diagonal-se { background-position: -80px -224px; }





.ui-corner-all,
.ui-corner-top,
.ui-corner-left,
.ui-corner-tl {
	border-top-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-top,
.ui-corner-right,
.ui-corner-tr {
	border-top-right-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-left,
.ui-corner-bl {
	border-bottom-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-right,
.ui-corner-br {
	border-bottom-right-radius: 3px;
}


.ui-widget-overlay {
	background: #aaaaaa;
	opacity: .3;
	filter: Alpha(Opacity=30); 
}
.ui-widget-shadow {
	-webkit-box-shadow: 0px 0px 5px #666666;
	box-shadow: 0px 0px 5px #666666;
} 
</style>
<script src='js/jquery-1.12.4.js'></script>
<script src='js/jquery-ui.js'></script>

<script>
$( function() {
	$( '#tabs-collect-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-consensus-heatmap'>
<ul>
<li><a href='#tab-collect-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-consensus-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-1-1.png" alt="plot of chunk tab-collect-consensus-heatmap-1"/></p>

</div>
<div id='tab-collect-consensus-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-2-1.png" alt="plot of chunk tab-collect-consensus-heatmap-2"/></p>

</div>
<div id='tab-collect-consensus-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-3-1.png" alt="plot of chunk tab-collect-consensus-heatmap-3"/></p>

</div>
<div id='tab-collect-consensus-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-4-1.png" alt="plot of chunk tab-collect-consensus-heatmap-4"/></p>

</div>
<div id='tab-collect-consensus-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-5-1.png" alt="plot of chunk tab-collect-consensus-heatmap-5"/></p>

</div>
</div>



### Membership heatmap

Membership heatmaps for all methods. ([What is a membership heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_12))


<script>
$( function() {
	$( '#tabs-collect-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-membership-heatmap'>
<ul>
<li><a href='#tab-collect-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-membership-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-1-1.png" alt="plot of chunk tab-collect-membership-heatmap-1"/></p>

</div>
<div id='tab-collect-membership-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-2-1.png" alt="plot of chunk tab-collect-membership-heatmap-2"/></p>

</div>
<div id='tab-collect-membership-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-3-1.png" alt="plot of chunk tab-collect-membership-heatmap-3"/></p>

</div>
<div id='tab-collect-membership-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-4-1.png" alt="plot of chunk tab-collect-membership-heatmap-4"/></p>

</div>
<div id='tab-collect-membership-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-5-1.png" alt="plot of chunk tab-collect-membership-heatmap-5"/></p>

</div>
</div>



### Signature heatmap

Signature heatmaps for all methods. ([What is a signature heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_22))


Note in following heatmaps, rows are scaled.



<script>
$( function() {
	$( '#tabs-collect-get-signatures' ).tabs();
} );
</script>
<div id='tabs-collect-get-signatures'>
<ul>
<li><a href='#tab-collect-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-collect-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-collect-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-collect-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-collect-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-collect-get-signatures-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-1-1.png" alt="plot of chunk tab-collect-get-signatures-1"/></p>

</div>
<div id='tab-collect-get-signatures-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-2-1.png" alt="plot of chunk tab-collect-get-signatures-2"/></p>

</div>
<div id='tab-collect-get-signatures-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-3-1.png" alt="plot of chunk tab-collect-get-signatures-3"/></p>

</div>
<div id='tab-collect-get-signatures-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-4-1.png" alt="plot of chunk tab-collect-get-signatures-4"/></p>

</div>
<div id='tab-collect-get-signatures-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-5-1.png" alt="plot of chunk tab-collect-get-signatures-5"/></p>

</div>
</div>



### Statistics table

The statistics used for measuring the stability of consensus partitioning.
([How are they
defined?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13))


<script>
$( function() {
	$( '#tabs-get-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-get-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-get-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-get-stats-from-consensus-partition-list-1'>
<pre><code class="r">get_stats(res_list, k = 2)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      2 0.562           0.872       0.899          0.406 0.628   0.628
#&gt; CV:NMF      2 0.520           0.791       0.906          0.467 0.535   0.535
#&gt; MAD:NMF     2 0.634           0.766       0.907          0.431 0.575   0.575
#&gt; ATC:NMF     2 0.859           0.941       0.971          0.489 0.516   0.516
#&gt; SD:skmeans  2 1.000           0.987       0.993          0.494 0.508   0.508
#&gt; CV:skmeans  2 0.499           0.870       0.907          0.495 0.508   0.508
#&gt; MAD:skmeans 2 1.000           0.998       0.999          0.493 0.508   0.508
#&gt; ATC:skmeans 2 1.000           0.993       0.997          0.486 0.516   0.516
#&gt; SD:mclust   2 0.476           0.690       0.847          0.418 0.648   0.648
#&gt; CV:mclust   2 0.527           0.896       0.928          0.366 0.648   0.648
#&gt; MAD:mclust  2 0.523           0.898       0.938          0.356 0.669   0.669
#&gt; ATC:mclust  2 0.788           0.856       0.935          0.432 0.535   0.535
#&gt; SD:kmeans   2 0.342           0.735       0.839          0.393 0.516   0.516
#&gt; CV:kmeans   2 0.293           0.472       0.686          0.406 0.628   0.628
#&gt; MAD:kmeans  2 0.353           0.457       0.716          0.414 0.628   0.628
#&gt; ATC:kmeans  2 0.484           0.750       0.820          0.348 0.693   0.693
#&gt; SD:pam      2 1.000           0.968       0.988          0.467 0.535   0.535
#&gt; CV:pam      2 0.744           0.802       0.929          0.415 0.591   0.591
#&gt; MAD:pam     2 0.762           0.942       0.972          0.457 0.535   0.535
#&gt; ATC:pam     2 1.000           0.993       0.997          0.349 0.648   0.648
#&gt; SD:hclust   2 0.560           0.741       0.898          0.398 0.648   0.648
#&gt; CV:hclust   2 0.627           0.790       0.905          0.357 0.693   0.693
#&gt; MAD:hclust  2 0.325           0.688       0.864          0.415 0.648   0.648
#&gt; ATC:hclust  2 1.000           1.000       1.000          0.308 0.693   0.693
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-2'>
<pre><code class="r">get_stats(res_list, k = 3)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      3 0.971           0.928       0.975          0.511 0.718   0.564
#&gt; CV:NMF      3 0.998           0.960       0.983          0.363 0.701   0.501
#&gt; MAD:NMF     3 0.970           0.957       0.981          0.444 0.743   0.574
#&gt; ATC:NMF     3 0.941           0.937       0.974          0.283 0.653   0.438
#&gt; SD:skmeans  3 0.970           0.939       0.977          0.318 0.715   0.499
#&gt; CV:skmeans  3 1.000           0.971       0.988          0.304 0.757   0.561
#&gt; MAD:skmeans 3 0.999           0.959       0.983          0.319 0.715   0.499
#&gt; ATC:skmeans 3 1.000           0.959       0.982          0.365 0.795   0.614
#&gt; SD:mclust   3 1.000           0.981       0.990          0.280 0.778   0.670
#&gt; CV:mclust   3 0.791           0.902       0.947          0.538 0.778   0.670
#&gt; MAD:mclust  3 1.000           0.951       0.980          0.525 0.804   0.708
#&gt; ATC:mclust  3 1.000           0.989       0.994          0.366 0.835   0.703
#&gt; SD:kmeans   3 0.915           0.957       0.982          0.465 0.893   0.796
#&gt; CV:kmeans   3 0.861           0.910       0.959          0.464 0.753   0.620
#&gt; MAD:kmeans  3 0.997           0.938       0.968          0.402 0.725   0.590
#&gt; ATC:kmeans  3 1.000           1.000       1.000          0.657 0.732   0.613
#&gt; SD:pam      3 0.915           0.941       0.975          0.334 0.770   0.596
#&gt; CV:pam      3 0.835           0.891       0.957          0.474 0.709   0.541
#&gt; MAD:pam     3 0.866           0.891       0.958          0.333 0.770   0.596
#&gt; ATC:pam     3 1.000           0.991       0.997          0.643 0.776   0.655
#&gt; SD:hclust   3 0.484           0.569       0.785          0.486 0.665   0.512
#&gt; CV:hclust   3 0.427           0.457       0.733          0.584 0.622   0.480
#&gt; MAD:hclust  3 0.536           0.702       0.824          0.464 0.762   0.632
#&gt; ATC:hclust  3 1.000           0.981       0.993          0.841 0.746   0.634
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-3'>
<pre><code class="r">get_stats(res_list, k = 4)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      4 0.896           0.905       0.946          0.213 0.825   0.569
#&gt; CV:NMF      4 0.916           0.894       0.949          0.178 0.841   0.588
#&gt; MAD:NMF     4 0.931           0.898       0.947          0.201 0.830   0.570
#&gt; ATC:NMF     4 0.854           0.865       0.934          0.197 0.837   0.582
#&gt; SD:skmeans  4 0.943           0.928       0.965          0.132 0.887   0.688
#&gt; CV:skmeans  4 0.900           0.865       0.944          0.148 0.888   0.694
#&gt; MAD:skmeans 4 0.973           0.945       0.972          0.141 0.871   0.646
#&gt; ATC:skmeans 4 0.941           0.921       0.964          0.106 0.931   0.797
#&gt; SD:mclust   4 0.767           0.877       0.915          0.325 0.807   0.593
#&gt; CV:mclust   4 0.710           0.795       0.855          0.297 0.767   0.519
#&gt; MAD:mclust  4 0.819           0.780       0.890          0.323 0.795   0.566
#&gt; ATC:mclust  4 0.752           0.808       0.876          0.140 0.901   0.769
#&gt; SD:kmeans   4 0.798           0.899       0.927          0.261 0.811   0.568
#&gt; CV:kmeans   4 0.698           0.812       0.869          0.220 0.806   0.558
#&gt; MAD:kmeans  4 0.798           0.905       0.920          0.251 0.818   0.586
#&gt; ATC:kmeans  4 0.780           0.918       0.932          0.255 0.806   0.558
#&gt; SD:pam      4 0.765           0.801       0.885          0.141 0.901   0.745
#&gt; CV:pam      4 0.780           0.825       0.915          0.194 0.868   0.662
#&gt; MAD:pam     4 0.892           0.902       0.944          0.185 0.865   0.658
#&gt; ATC:pam     4 0.909           0.918       0.964          0.236 0.798   0.555
#&gt; SD:hclust   4 0.747           0.789       0.897          0.229 0.758   0.469
#&gt; CV:hclust   4 0.695           0.792       0.883          0.272 0.729   0.421
#&gt; MAD:hclust  4 0.821           0.792       0.913          0.201 0.810   0.554
#&gt; ATC:hclust  4 0.803           0.916       0.931          0.262 0.839   0.634
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-4'>
<pre><code class="r">get_stats(res_list, k = 5)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      5 0.817           0.790       0.876         0.0575 0.929   0.732
#&gt; CV:NMF      5 0.852           0.809       0.897         0.0548 0.944   0.786
#&gt; MAD:NMF     5 0.842           0.752       0.885         0.0566 0.941   0.769
#&gt; ATC:NMF     5 0.746           0.692       0.836         0.0455 0.952   0.811
#&gt; SD:skmeans  5 0.797           0.668       0.810         0.0841 0.943   0.787
#&gt; CV:skmeans  5 0.798           0.655       0.846         0.0840 0.908   0.666
#&gt; MAD:skmeans 5 0.821           0.728       0.861         0.0792 0.914   0.681
#&gt; ATC:skmeans 5 0.881           0.838       0.861         0.0701 0.950   0.814
#&gt; SD:mclust   5 0.728           0.741       0.859         0.1006 0.903   0.661
#&gt; CV:mclust   5 0.748           0.799       0.852         0.0704 0.895   0.627
#&gt; MAD:mclust  5 0.749           0.714       0.844         0.0717 0.905   0.674
#&gt; ATC:mclust  5 0.758           0.819       0.881         0.1521 0.824   0.530
#&gt; SD:kmeans   5 0.763           0.641       0.824         0.0764 0.964   0.871
#&gt; CV:kmeans   5 0.729           0.593       0.780         0.0831 0.923   0.712
#&gt; MAD:kmeans  5 0.777           0.691       0.840         0.0858 0.929   0.734
#&gt; ATC:kmeans  5 0.795           0.722       0.878         0.0714 0.987   0.948
#&gt; SD:pam      5 0.772           0.677       0.821         0.0937 0.877   0.607
#&gt; CV:pam      5 0.857           0.880       0.915         0.0895 0.899   0.645
#&gt; MAD:pam     5 0.830           0.735       0.878         0.0855 0.864   0.560
#&gt; ATC:pam     5 0.784           0.804       0.888         0.0790 0.917   0.717
#&gt; SD:hclust   5 0.728           0.698       0.769         0.0515 0.943   0.819
#&gt; CV:hclust   5 0.712           0.723       0.826         0.0683 0.958   0.851
#&gt; MAD:hclust  5 0.828           0.731       0.811         0.0753 0.969   0.880
#&gt; ATC:hclust  5 0.961           0.947       0.976         0.0834 0.947   0.809
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-5'>
<pre><code class="r">get_stats(res_list, k = 6)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      6 0.819           0.754       0.859        0.03522 0.959   0.811
#&gt; CV:NMF      6 0.836           0.733       0.861        0.04348 0.945   0.753
#&gt; MAD:NMF     6 0.805           0.792       0.864        0.03409 0.964   0.829
#&gt; ATC:NMF     6 0.758           0.627       0.779        0.03116 0.922   0.683
#&gt; SD:skmeans  6 0.802           0.665       0.813        0.04075 0.939   0.730
#&gt; CV:skmeans  6 0.788           0.651       0.816        0.03459 0.909   0.595
#&gt; MAD:skmeans 6 0.826           0.671       0.820        0.03575 0.924   0.652
#&gt; ATC:skmeans 6 0.868           0.772       0.866        0.03746 0.973   0.879
#&gt; SD:mclust   6 0.783           0.790       0.864        0.02895 0.955   0.796
#&gt; CV:mclust   6 0.751           0.510       0.740        0.05019 0.886   0.529
#&gt; MAD:mclust  6 0.818           0.784       0.812        0.04888 0.929   0.702
#&gt; ATC:mclust  6 0.785           0.737       0.877        0.06281 0.889   0.560
#&gt; SD:kmeans   6 0.712           0.486       0.746        0.04605 0.933   0.740
#&gt; CV:kmeans   6 0.733           0.639       0.725        0.04426 0.912   0.615
#&gt; MAD:kmeans  6 0.766           0.673       0.783        0.04695 0.945   0.759
#&gt; ATC:kmeans  6 0.766           0.762       0.851        0.03997 0.915   0.685
#&gt; SD:pam      6 0.775           0.669       0.833        0.02975 0.857   0.504
#&gt; CV:pam      6 0.905           0.887       0.931        0.03131 0.976   0.884
#&gt; MAD:pam     6 0.844           0.704       0.856        0.03060 0.897   0.607
#&gt; ATC:pam     6 0.749           0.770       0.870        0.04482 0.926   0.700
#&gt; SD:hclust   6 0.800           0.702       0.848        0.04603 0.890   0.643
#&gt; CV:hclust   6 0.727           0.653       0.819        0.03981 0.946   0.779
#&gt; MAD:hclust  6 0.809           0.659       0.778        0.04471 0.909   0.626
#&gt; ATC:hclust  6 0.925           0.913       0.963        0.00707 0.994   0.972
</code></pre>

</div>
</div>

Following heatmap plots the partition for each combination of methods and the
lightness correspond to the silhouette scores for samples in each method. On
top the consensus subgroup is inferred from all methods by taking the mean
silhouette scores as weight.


<script>
$( function() {
	$( '#tabs-collect-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-stats-from-consensus-partition-list-1'>
<pre><code class="r">collect_stats(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-2'>
<pre><code class="r">collect_stats(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-3'>
<pre><code class="r">collect_stats(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-4'>
<pre><code class="r">collect_stats(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-5'>
<pre><code class="r">collect_stats(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-5"/></p>

</div>
</div>

### Partition from all methods



Collect partitions from all methods:


<script>
$( function() {
	$( '#tabs-collect-classes-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-classes-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-classes-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-classes-from-consensus-partition-list-1'>
<pre><code class="r">collect_classes(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-2'>
<pre><code class="r">collect_classes(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-3'>
<pre><code class="r">collect_classes(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-4'>
<pre><code class="r">collect_classes(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-5'>
<pre><code class="r">collect_classes(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-5"/></p>

</div>
</div>



### Top rows overlap


Overlap of top rows from different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-euler' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-euler'>
<ul>
<li><a href='#tab-top-rows-overlap-by-euler-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-euler-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-5"/></p>

</div>
</div>

Also visualize the correspondance of rankings between different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-correspondance' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-correspondance'>
<ul>
<li><a href='#tab-top-rows-overlap-by-correspondance-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-correspondance-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-5"/></p>

</div>
</div>


Heatmaps of the top rows:



<script>
$( function() {
	$( '#tabs-top-rows-heatmap' ).tabs();
} );
</script>
<div id='tabs-top-rows-heatmap'>
<ul>
<li><a href='#tab-top-rows-heatmap-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-heatmap-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-heatmap-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-heatmap-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-heatmap-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-heatmap-1'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 1000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-1-1.png" alt="plot of chunk tab-top-rows-heatmap-1"/></p>

</div>
<div id='tab-top-rows-heatmap-2'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 2000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-2-1.png" alt="plot of chunk tab-top-rows-heatmap-2"/></p>

</div>
<div id='tab-top-rows-heatmap-3'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 3000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-3-1.png" alt="plot of chunk tab-top-rows-heatmap-3"/></p>

</div>
<div id='tab-top-rows-heatmap-4'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 4000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-4-1.png" alt="plot of chunk tab-top-rows-heatmap-4"/></p>

</div>
<div id='tab-top-rows-heatmap-5'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 5000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-5-1.png" alt="plot of chunk tab-top-rows-heatmap-5"/></p>

</div>
</div>




### Test to known annotations



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.


<script>
$( function() {
	$( '#tabs-test-to-known-factors-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-test-to-known-factors-from-consensus-partition-list'>
<ul>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-test-to-known-factors-from-consensus-partition-list-1'>
<pre><code class="r">test_to_known_factors(res_list, k = 2)
</code></pre>

<pre><code>#&gt;              n tissue(p) k
#&gt; SD:NMF      54     0.398 2
#&gt; CV:NMF      48     0.432 2
#&gt; MAD:NMF     49     0.393 2
#&gt; ATC:NMF     53     0.397 2
#&gt; SD:skmeans  54     0.398 2
#&gt; CV:skmeans  54     0.398 2
#&gt; MAD:skmeans 54     0.398 2
#&gt; ATC:skmeans 54     0.398 2
#&gt; SD:mclust   39     0.425 2
#&gt; CV:mclust   53     0.397 2
#&gt; MAD:mclust  54     0.398 2
#&gt; ATC:mclust  53     0.397 2
#&gt; SD:kmeans   41     0.383 2
#&gt; CV:kmeans   35     0.373 2
#&gt; MAD:kmeans  38     0.378 2
#&gt; ATC:kmeans  54     0.398 2
#&gt; SD:pam      53     0.397 2
#&gt; CV:pam      47     0.391 2
#&gt; MAD:pam     53     0.397 2
#&gt; ATC:pam     54     0.398 2
#&gt; SD:hclust   45     0.388 2
#&gt; CV:hclust   47     0.391 2
#&gt; MAD:hclust  46     0.389 2
#&gt; ATC:hclust  54     0.398 2
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-2'>
<pre><code class="r">test_to_known_factors(res_list, k = 3)
</code></pre>

<pre><code>#&gt;              n tissue(p) k
#&gt; SD:NMF      52     0.372 3
#&gt; CV:NMF      54     0.374 3
#&gt; MAD:NMF     54     0.374 3
#&gt; ATC:NMF     53     0.373 3
#&gt; SD:skmeans  52     0.372 3
#&gt; CV:skmeans  53     0.373 3
#&gt; MAD:skmeans 54     0.374 3
#&gt; ATC:skmeans 53     0.373 3
#&gt; SD:mclust   54     0.374 3
#&gt; CV:mclust   53     0.373 3
#&gt; MAD:mclust  52     0.372 3
#&gt; ATC:mclust  54     0.374 3
#&gt; SD:kmeans   53     0.373 3
#&gt; CV:kmeans   52     0.372 3
#&gt; MAD:kmeans  52     0.372 3
#&gt; ATC:kmeans  54     0.374 3
#&gt; SD:pam      52     0.372 3
#&gt; CV:pam      52     0.372 3
#&gt; MAD:pam     49     0.368 3
#&gt; ATC:pam     54     0.374 3
#&gt; SD:hclust   29     0.401 3
#&gt; CV:hclust   19     0.392 3
#&gt; MAD:hclust  46     0.364 3
#&gt; ATC:hclust  53     0.373 3
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-3'>
<pre><code class="r">test_to_known_factors(res_list, k = 4)
</code></pre>

<pre><code>#&gt;              n tissue(p) k
#&gt; SD:NMF      52     0.352 4
#&gt; CV:NMF      51     0.350 4
#&gt; MAD:NMF     52     0.352 4
#&gt; ATC:NMF     52     0.352 4
#&gt; SD:skmeans  54     0.355 4
#&gt; CV:skmeans  49     0.415 4
#&gt; MAD:skmeans 53     0.353 4
#&gt; ATC:skmeans 53     0.353 4
#&gt; SD:mclust   54     0.355 4
#&gt; CV:mclust   49     0.348 4
#&gt; MAD:mclust  51     0.350 4
#&gt; ATC:mclust  50     0.349 4
#&gt; SD:kmeans   52     0.417 4
#&gt; CV:kmeans   50     0.416 4
#&gt; MAD:kmeans  53     0.353 4
#&gt; ATC:kmeans  53     0.353 4
#&gt; SD:pam      49     0.348 4
#&gt; CV:pam      48     0.346 4
#&gt; MAD:pam     53     0.353 4
#&gt; ATC:pam     51     0.350 4
#&gt; SD:hclust   45     0.411 4
#&gt; CV:hclust   50     0.349 4
#&gt; MAD:hclust  48     0.346 4
#&gt; ATC:hclust  53     0.353 4
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-4'>
<pre><code class="r">test_to_known_factors(res_list, k = 5)
</code></pre>

<pre><code>#&gt;              n tissue(p) k
#&gt; SD:NMF      48     0.476 5
#&gt; CV:NMF      50     0.525 5
#&gt; MAD:NMF     44     0.410 5
#&gt; ATC:NMF     43     0.338 5
#&gt; SD:skmeans  39     0.405 5
#&gt; CV:skmeans  40     0.397 5
#&gt; MAD:skmeans 43     0.319 5
#&gt; ATC:skmeans 49     0.406 5
#&gt; SD:mclust   45     0.419 5
#&gt; CV:mclust   50     0.434 5
#&gt; MAD:mclust  43     0.487 5
#&gt; ATC:mclust  52     0.334 5
#&gt; SD:kmeans   39     0.405 5
#&gt; CV:kmeans   38     0.404 5
#&gt; MAD:kmeans  44     0.321 5
#&gt; ATC:kmeans  46     0.343 5
#&gt; SD:pam      38     0.394 5
#&gt; CV:pam      52     0.334 5
#&gt; MAD:pam     46     0.324 5
#&gt; ATC:pam     51     0.457 5
#&gt; SD:hclust   44     0.401 5
#&gt; CV:hclust   44     0.401 5
#&gt; MAD:hclust  48     0.328 5
#&gt; ATC:hclust  53     0.336 5
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-5'>
<pre><code class="r">test_to_known_factors(res_list, k = 6)
</code></pre>

<pre><code>#&gt;              n tissue(p) k
#&gt; SD:NMF      47     0.438 6
#&gt; CV:NMF      46     0.455 6
#&gt; MAD:NMF     51     0.452 6
#&gt; ATC:NMF     38     0.308 6
#&gt; SD:skmeans  41     0.389 6
#&gt; CV:skmeans  38     0.394 6
#&gt; MAD:skmeans 39     0.293 6
#&gt; ATC:skmeans 46     0.403 6
#&gt; SD:mclust   49     0.426 6
#&gt; CV:mclust   26     0.384 6
#&gt; MAD:mclust  48     0.398 6
#&gt; ATC:mclust  47     0.458 6
#&gt; SD:kmeans   25     0.394 6
#&gt; CV:kmeans   44     0.393 6
#&gt; MAD:kmeans  44     0.321 6
#&gt; ATC:kmeans  50     0.315 6
#&gt; SD:pam      41     0.389 6
#&gt; CV:pam      53     0.320 6
#&gt; MAD:pam     43     0.392 6
#&gt; ATC:pam     51     0.426 6
#&gt; SD:hclust   42     0.391 6
#&gt; CV:hclust   44     0.393 6
#&gt; MAD:hclust  45     0.394 6
#&gt; ATC:hclust  51     0.333 6
</code></pre>

</div>
</div>


 
## Results for each method


---------------------------------------------------




### SD:hclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "hclust"]
# you can also extract it by
# res = res_list["SD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21452 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-hclust-collect-plots](figure_cola/SD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-hclust-select-partition-number](figure_cola/SD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.560           0.741       0.898         0.3979 0.648   0.648
#> 3 3 0.484           0.569       0.785         0.4861 0.665   0.512
#> 4 4 0.747           0.789       0.897         0.2293 0.758   0.469
#> 5 5 0.728           0.698       0.769         0.0515 0.943   0.819
#> 6 6 0.800           0.702       0.848         0.0460 0.890   0.643
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-classes'>
<ul>
<li><a href='#tab-SD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-hclust-get-classes-1'>
<p><a id='tab-SD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28710     2   0.808      0.645 0.248 0.752
#&gt; GSM28711     2   0.808      0.645 0.248 0.752
#&gt; GSM28712     2   0.000      0.868 0.000 1.000
#&gt; GSM11222     1   0.388      0.851 0.924 0.076
#&gt; GSM28720     2   0.000      0.868 0.000 1.000
#&gt; GSM11217     2   0.000      0.868 0.000 1.000
#&gt; GSM28723     2   0.000      0.868 0.000 1.000
#&gt; GSM11241     2   0.000      0.868 0.000 1.000
#&gt; GSM28703     2   0.000      0.868 0.000 1.000
#&gt; GSM11227     2   0.000      0.868 0.000 1.000
#&gt; GSM28706     2   0.000      0.868 0.000 1.000
#&gt; GSM11229     2   0.000      0.868 0.000 1.000
#&gt; GSM11235     2   0.000      0.868 0.000 1.000
#&gt; GSM28707     2   0.000      0.868 0.000 1.000
#&gt; GSM11240     2   0.000      0.868 0.000 1.000
#&gt; GSM28714     2   0.000      0.868 0.000 1.000
#&gt; GSM11216     1   0.000      0.899 1.000 0.000
#&gt; GSM28715     2   0.000      0.868 0.000 1.000
#&gt; GSM11234     2   0.000      0.868 0.000 1.000
#&gt; GSM28699     2   0.000      0.868 0.000 1.000
#&gt; GSM11233     2   0.000      0.868 0.000 1.000
#&gt; GSM28718     2   0.000      0.868 0.000 1.000
#&gt; GSM11231     2   0.430      0.806 0.088 0.912
#&gt; GSM11237     2   0.000      0.868 0.000 1.000
#&gt; GSM11228     2   1.000      0.115 0.496 0.504
#&gt; GSM28697     2   1.000      0.115 0.496 0.504
#&gt; GSM28698     1   0.000      0.899 1.000 0.000
#&gt; GSM11238     1   0.000      0.899 1.000 0.000
#&gt; GSM11242     1   0.000      0.899 1.000 0.000
#&gt; GSM28719     2   1.000      0.115 0.496 0.504
#&gt; GSM28708     2   1.000      0.115 0.496 0.504
#&gt; GSM28722     2   0.697      0.719 0.188 0.812
#&gt; GSM11232     2   0.814      0.642 0.252 0.748
#&gt; GSM28709     1   0.000      0.899 1.000 0.000
#&gt; GSM11226     1   0.946      0.342 0.636 0.364
#&gt; GSM11239     1   0.000      0.899 1.000 0.000
#&gt; GSM11225     1   0.000      0.899 1.000 0.000
#&gt; GSM11220     1   0.000      0.899 1.000 0.000
#&gt; GSM28701     2   0.827      0.629 0.260 0.740
#&gt; GSM28721     2   1.000      0.115 0.496 0.504
#&gt; GSM28713     2   0.000      0.868 0.000 1.000
#&gt; GSM28716     2   0.000      0.868 0.000 1.000
#&gt; GSM11221     2   0.000      0.868 0.000 1.000
#&gt; GSM28717     2   0.000      0.868 0.000 1.000
#&gt; GSM11223     2   0.000      0.868 0.000 1.000
#&gt; GSM11218     1   0.946      0.342 0.636 0.364
#&gt; GSM11219     2   0.000      0.868 0.000 1.000
#&gt; GSM11236     2   0.929      0.492 0.344 0.656
#&gt; GSM28702     1   0.388      0.851 0.924 0.076
#&gt; GSM28705     2   1.000      0.115 0.496 0.504
#&gt; GSM11230     2   0.000      0.868 0.000 1.000
#&gt; GSM28704     2   0.000      0.868 0.000 1.000
#&gt; GSM28700     2   0.000      0.868 0.000 1.000
#&gt; GSM11224     2   0.000      0.868 0.000 1.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-1-a').click(function(){
  $('#tab-SD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-2'>
<p><a id='tab-SD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28710     2   0.397      0.585 0.132 0.860 0.008
#&gt; GSM28711     2   0.397      0.585 0.132 0.860 0.008
#&gt; GSM28712     2   0.631      0.323 0.492 0.508 0.000
#&gt; GSM11222     3   0.625      0.527 0.000 0.444 0.556
#&gt; GSM28720     1   0.000      0.819 1.000 0.000 0.000
#&gt; GSM11217     1   0.000      0.819 1.000 0.000 0.000
#&gt; GSM28723     1   0.000      0.819 1.000 0.000 0.000
#&gt; GSM11241     1   0.000      0.819 1.000 0.000 0.000
#&gt; GSM28703     1   0.000      0.819 1.000 0.000 0.000
#&gt; GSM11227     1   0.000      0.819 1.000 0.000 0.000
#&gt; GSM28706     1   0.000      0.819 1.000 0.000 0.000
#&gt; GSM11229     1   0.000      0.819 1.000 0.000 0.000
#&gt; GSM11235     1   0.000      0.819 1.000 0.000 0.000
#&gt; GSM28707     1   0.000      0.819 1.000 0.000 0.000
#&gt; GSM11240     2   0.615      0.493 0.408 0.592 0.000
#&gt; GSM28714     2   0.615      0.493 0.408 0.592 0.000
#&gt; GSM11216     3   0.000      0.908 0.000 0.000 1.000
#&gt; GSM28715     2   0.614      0.497 0.404 0.596 0.000
#&gt; GSM11234     2   0.631      0.323 0.492 0.508 0.000
#&gt; GSM28699     1   0.624     -0.148 0.560 0.440 0.000
#&gt; GSM11233     1   0.624     -0.148 0.560 0.440 0.000
#&gt; GSM28718     2   0.615      0.493 0.408 0.592 0.000
#&gt; GSM11231     2   0.568      0.525 0.316 0.684 0.000
#&gt; GSM11237     2   0.615      0.493 0.408 0.592 0.000
#&gt; GSM11228     2   0.341      0.475 0.000 0.876 0.124
#&gt; GSM28697     2   0.341      0.475 0.000 0.876 0.124
#&gt; GSM28698     3   0.000      0.908 0.000 0.000 1.000
#&gt; GSM11238     3   0.000      0.908 0.000 0.000 1.000
#&gt; GSM11242     3   0.000      0.908 0.000 0.000 1.000
#&gt; GSM28719     2   0.341      0.475 0.000 0.876 0.124
#&gt; GSM28708     2   0.341      0.475 0.000 0.876 0.124
#&gt; GSM28722     2   0.435      0.577 0.184 0.816 0.000
#&gt; GSM11232     2   0.530      0.577 0.156 0.808 0.036
#&gt; GSM28709     3   0.000      0.908 0.000 0.000 1.000
#&gt; GSM11226     2   0.525      0.235 0.000 0.736 0.264
#&gt; GSM11239     3   0.000      0.908 0.000 0.000 1.000
#&gt; GSM11225     3   0.000      0.908 0.000 0.000 1.000
#&gt; GSM11220     3   0.000      0.908 0.000 0.000 1.000
#&gt; GSM28701     2   0.375      0.584 0.120 0.872 0.008
#&gt; GSM28721     2   0.341      0.475 0.000 0.876 0.124
#&gt; GSM28713     2   0.630      0.364 0.472 0.528 0.000
#&gt; GSM28716     1   0.529      0.393 0.732 0.268 0.000
#&gt; GSM11221     2   0.614      0.497 0.404 0.596 0.000
#&gt; GSM28717     1   0.624     -0.148 0.560 0.440 0.000
#&gt; GSM11223     1   0.000      0.819 1.000 0.000 0.000
#&gt; GSM11218     2   0.525      0.235 0.000 0.736 0.264
#&gt; GSM11219     2   0.614      0.497 0.404 0.596 0.000
#&gt; GSM11236     2   0.376      0.571 0.068 0.892 0.040
#&gt; GSM28702     3   0.625      0.527 0.000 0.444 0.556
#&gt; GSM28705     2   0.341      0.475 0.000 0.876 0.124
#&gt; GSM11230     2   0.615      0.493 0.408 0.592 0.000
#&gt; GSM28704     2   0.610      0.505 0.392 0.608 0.000
#&gt; GSM28700     2   0.631      0.323 0.492 0.508 0.000
#&gt; GSM11224     2   0.631      0.323 0.492 0.508 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-2-a').click(function(){
  $('#tab-SD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-3'>
<p><a id='tab-SD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28710     2  0.4907      0.392 0.000 0.580 0.000 0.420
#&gt; GSM28711     2  0.4907      0.392 0.000 0.580 0.000 0.420
#&gt; GSM28712     2  0.0000      0.819 0.000 1.000 0.000 0.000
#&gt; GSM11222     4  0.4972      0.231 0.000 0.000 0.456 0.544
#&gt; GSM28720     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.2081      0.832 0.000 0.916 0.000 0.084
#&gt; GSM28714     2  0.2081      0.832 0.000 0.916 0.000 0.084
#&gt; GSM11216     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28715     2  0.2149      0.830 0.000 0.912 0.000 0.088
#&gt; GSM11234     2  0.0000      0.819 0.000 1.000 0.000 0.000
#&gt; GSM28699     2  0.2124      0.783 0.040 0.932 0.000 0.028
#&gt; GSM11233     2  0.2124      0.783 0.040 0.932 0.000 0.028
#&gt; GSM28718     2  0.2081      0.832 0.000 0.916 0.000 0.084
#&gt; GSM11231     2  0.4961      0.274 0.000 0.552 0.000 0.448
#&gt; GSM11237     2  0.2081      0.832 0.000 0.916 0.000 0.084
#&gt; GSM11228     4  0.1302      0.794 0.000 0.044 0.000 0.956
#&gt; GSM28697     4  0.1302      0.794 0.000 0.044 0.000 0.956
#&gt; GSM28698     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11238     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11242     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28719     4  0.0921      0.794 0.000 0.028 0.000 0.972
#&gt; GSM28708     4  0.0921      0.794 0.000 0.028 0.000 0.972
#&gt; GSM28722     2  0.4888      0.402 0.000 0.588 0.000 0.412
#&gt; GSM11232     4  0.4730      0.275 0.000 0.364 0.000 0.636
#&gt; GSM28709     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11226     4  0.2921      0.701 0.000 0.000 0.140 0.860
#&gt; GSM11239     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11225     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11220     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28701     2  0.4933      0.361 0.000 0.568 0.000 0.432
#&gt; GSM28721     4  0.1792      0.783 0.000 0.068 0.000 0.932
#&gt; GSM28713     2  0.1022      0.820 0.000 0.968 0.000 0.032
#&gt; GSM28716     2  0.4304      0.530 0.284 0.716 0.000 0.000
#&gt; GSM11221     2  0.2149      0.830 0.000 0.912 0.000 0.088
#&gt; GSM28717     2  0.2124      0.783 0.040 0.932 0.000 0.028
#&gt; GSM11223     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11218     4  0.2921      0.701 0.000 0.000 0.140 0.860
#&gt; GSM11219     2  0.2149      0.830 0.000 0.912 0.000 0.088
#&gt; GSM11236     4  0.4331      0.465 0.000 0.288 0.000 0.712
#&gt; GSM28702     4  0.4972      0.231 0.000 0.000 0.456 0.544
#&gt; GSM28705     4  0.1792      0.783 0.000 0.068 0.000 0.932
#&gt; GSM11230     2  0.2081      0.832 0.000 0.916 0.000 0.084
#&gt; GSM28704     2  0.2530      0.818 0.000 0.888 0.000 0.112
#&gt; GSM28700     2  0.0000      0.819 0.000 1.000 0.000 0.000
#&gt; GSM11224     2  0.0000      0.819 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-3-a').click(function(){
  $('#tab-SD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-4'>
<p><a id='tab-SD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28710     2  0.4384      0.408 0.000 0.660 0.000 0.016 0.324
#&gt; GSM28711     2  0.4384      0.408 0.000 0.660 0.000 0.016 0.324
#&gt; GSM28712     2  0.3366      0.674 0.000 0.768 0.232 0.000 0.000
#&gt; GSM11222     4  0.5222      0.115 0.000 0.000 0.196 0.680 0.124
#&gt; GSM28720     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.0000      0.718 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28714     2  0.0000      0.718 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11216     5  0.4182      1.000 0.000 0.000 0.400 0.000 0.600
#&gt; GSM28715     2  0.0162      0.718 0.000 0.996 0.000 0.000 0.004
#&gt; GSM11234     2  0.3366      0.674 0.000 0.768 0.232 0.000 0.000
#&gt; GSM28699     2  0.4235      0.558 0.000 0.576 0.424 0.000 0.000
#&gt; GSM11233     2  0.4235      0.558 0.000 0.576 0.424 0.000 0.000
#&gt; GSM28718     2  0.0000      0.718 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11231     2  0.5345      0.311 0.000 0.632 0.000 0.280 0.088
#&gt; GSM11237     2  0.0000      0.718 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11228     4  0.6072      0.682 0.000 0.124 0.000 0.484 0.392
#&gt; GSM28697     4  0.6072      0.682 0.000 0.124 0.000 0.484 0.392
#&gt; GSM28698     3  0.4235      1.000 0.000 0.000 0.576 0.000 0.424
#&gt; GSM11238     3  0.4235      1.000 0.000 0.000 0.576 0.000 0.424
#&gt; GSM11242     3  0.4235      1.000 0.000 0.000 0.576 0.000 0.424
#&gt; GSM28719     4  0.5889      0.688 0.000 0.104 0.000 0.504 0.392
#&gt; GSM28708     4  0.5876      0.689 0.000 0.104 0.000 0.512 0.384
#&gt; GSM28722     2  0.5223      0.420 0.000 0.672 0.000 0.108 0.220
#&gt; GSM11232     2  0.6645     -0.170 0.000 0.448 0.000 0.292 0.260
#&gt; GSM28709     5  0.4182      1.000 0.000 0.000 0.400 0.000 0.600
#&gt; GSM11226     4  0.0162      0.520 0.000 0.000 0.000 0.996 0.004
#&gt; GSM11239     3  0.4235      1.000 0.000 0.000 0.576 0.000 0.424
#&gt; GSM11225     3  0.4235      1.000 0.000 0.000 0.576 0.000 0.424
#&gt; GSM11220     5  0.4182      1.000 0.000 0.000 0.400 0.000 0.600
#&gt; GSM28701     2  0.4435      0.387 0.000 0.648 0.000 0.016 0.336
#&gt; GSM28721     4  0.6232      0.659 0.000 0.148 0.000 0.480 0.372
#&gt; GSM28713     2  0.3897      0.683 0.000 0.768 0.204 0.000 0.028
#&gt; GSM28716     2  0.6510      0.409 0.284 0.484 0.232 0.000 0.000
#&gt; GSM11221     2  0.0162      0.718 0.000 0.996 0.000 0.000 0.004
#&gt; GSM28717     2  0.4235      0.558 0.000 0.576 0.424 0.000 0.000
#&gt; GSM11223     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     4  0.0162      0.520 0.000 0.000 0.000 0.996 0.004
#&gt; GSM11219     2  0.0162      0.718 0.000 0.996 0.000 0.000 0.004
#&gt; GSM11236     2  0.6756     -0.327 0.000 0.368 0.000 0.264 0.368
#&gt; GSM28702     4  0.5222      0.115 0.000 0.000 0.196 0.680 0.124
#&gt; GSM28705     4  0.6243      0.657 0.000 0.148 0.000 0.472 0.380
#&gt; GSM11230     2  0.0000      0.718 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28704     2  0.0794      0.708 0.000 0.972 0.000 0.000 0.028
#&gt; GSM28700     2  0.3366      0.674 0.000 0.768 0.232 0.000 0.000
#&gt; GSM11224     2  0.3336      0.675 0.000 0.772 0.228 0.000 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-4-a').click(function(){
  $('#tab-SD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-5'>
<p><a id='tab-SD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28710     2  0.4004      0.255 0.000 0.620 0.000 0.368 0.012 0.000
#&gt; GSM28711     2  0.4004      0.255 0.000 0.620 0.000 0.368 0.012 0.000
#&gt; GSM28712     2  0.3409      0.429 0.000 0.700 0.000 0.000 0.300 0.000
#&gt; GSM11222     6  0.2219      0.692 0.000 0.000 0.136 0.000 0.000 0.864
#&gt; GSM28720     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.0146      0.699 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM28714     2  0.0146      0.699 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM11216     3  0.2219      0.741 0.000 0.000 0.864 0.000 0.136 0.000
#&gt; GSM28715     2  0.0000      0.699 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11234     2  0.3409      0.429 0.000 0.700 0.000 0.000 0.300 0.000
#&gt; GSM28699     5  0.2597      1.000 0.000 0.176 0.000 0.000 0.824 0.000
#&gt; GSM11233     5  0.2597      1.000 0.000 0.176 0.000 0.000 0.824 0.000
#&gt; GSM28718     2  0.0146      0.699 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM11231     2  0.4212      0.136 0.000 0.560 0.000 0.424 0.016 0.000
#&gt; GSM11237     2  0.0146      0.699 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM11228     4  0.1003      0.761 0.000 0.020 0.000 0.964 0.000 0.016
#&gt; GSM28697     4  0.1003      0.761 0.000 0.020 0.000 0.964 0.000 0.016
#&gt; GSM28698     3  0.2631      0.841 0.000 0.000 0.820 0.000 0.000 0.180
#&gt; GSM11238     3  0.2631      0.841 0.000 0.000 0.820 0.000 0.000 0.180
#&gt; GSM11242     3  0.2631      0.841 0.000 0.000 0.820 0.000 0.000 0.180
#&gt; GSM28719     4  0.0458      0.745 0.000 0.000 0.000 0.984 0.016 0.000
#&gt; GSM28708     4  0.0717      0.745 0.000 0.000 0.000 0.976 0.016 0.008
#&gt; GSM28722     2  0.4628      0.392 0.000 0.672 0.000 0.268 0.024 0.036
#&gt; GSM11232     4  0.5235      0.178 0.000 0.448 0.000 0.484 0.024 0.044
#&gt; GSM28709     3  0.2219      0.741 0.000 0.000 0.864 0.000 0.136 0.000
#&gt; GSM11226     6  0.2631      0.697 0.000 0.000 0.000 0.180 0.000 0.820
#&gt; GSM11239     3  0.2631      0.841 0.000 0.000 0.820 0.000 0.000 0.180
#&gt; GSM11225     3  0.2631      0.841 0.000 0.000 0.820 0.000 0.000 0.180
#&gt; GSM11220     3  0.2219      0.741 0.000 0.000 0.864 0.000 0.136 0.000
#&gt; GSM28701     2  0.4037      0.223 0.000 0.608 0.000 0.380 0.012 0.000
#&gt; GSM28721     4  0.3946      0.697 0.000 0.044 0.000 0.780 0.024 0.152
#&gt; GSM28713     2  0.3175      0.511 0.000 0.744 0.000 0.000 0.256 0.000
#&gt; GSM28716     2  0.6047     -0.166 0.284 0.416 0.000 0.000 0.300 0.000
#&gt; GSM11221     2  0.0000      0.699 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28717     5  0.2597      1.000 0.000 0.176 0.000 0.000 0.824 0.000
#&gt; GSM11223     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     6  0.2631      0.697 0.000 0.000 0.000 0.180 0.000 0.820
#&gt; GSM11219     2  0.0000      0.699 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11236     4  0.4335      0.475 0.000 0.324 0.000 0.644 0.024 0.008
#&gt; GSM28702     6  0.2219      0.692 0.000 0.000 0.136 0.000 0.000 0.864
#&gt; GSM28705     4  0.3795      0.709 0.000 0.044 0.000 0.796 0.024 0.136
#&gt; GSM11230     2  0.0146      0.699 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM28704     2  0.0632      0.689 0.000 0.976 0.000 0.000 0.024 0.000
#&gt; GSM28700     2  0.3409      0.429 0.000 0.700 0.000 0.000 0.300 0.000
#&gt; GSM11224     2  0.3390      0.435 0.000 0.704 0.000 0.000 0.296 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-5-a').click(function(){
  $('#tab-SD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-hclust-signature_compare](figure_cola/SD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-hclust-collect-classes](figure_cola/SD-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) k
#> SD:hclust 45     0.388 2
#> SD:hclust 29     0.401 3
#> SD:hclust 45     0.411 4
#> SD:hclust 44     0.401 5
#> SD:hclust 42     0.391 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:kmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "kmeans"]
# you can also extract it by
# res = res_list["SD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21452 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-kmeans-collect-plots](figure_cola/SD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-kmeans-select-partition-number](figure_cola/SD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.342           0.735       0.839         0.3926 0.516   0.516
#> 3 3 0.915           0.957       0.982         0.4652 0.893   0.796
#> 4 4 0.798           0.899       0.927         0.2605 0.811   0.568
#> 5 5 0.763           0.641       0.824         0.0764 0.964   0.871
#> 6 6 0.712           0.486       0.746         0.0461 0.933   0.740
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-classes'>
<ul>
<li><a href='#tab-SD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-kmeans-get-classes-1'>
<p><a id='tab-SD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28710     2   0.000      0.928 0.000 1.000
#&gt; GSM28711     2   0.000      0.928 0.000 1.000
#&gt; GSM28712     2   0.000      0.928 0.000 1.000
#&gt; GSM11222     1   0.795      0.583 0.760 0.240
#&gt; GSM28720     1   0.993      0.498 0.548 0.452
#&gt; GSM11217     1   0.993      0.498 0.548 0.452
#&gt; GSM28723     1   0.993      0.498 0.548 0.452
#&gt; GSM11241     1   0.993      0.498 0.548 0.452
#&gt; GSM28703     1   0.993      0.498 0.548 0.452
#&gt; GSM11227     1   0.993      0.498 0.548 0.452
#&gt; GSM28706     1   0.993      0.498 0.548 0.452
#&gt; GSM11229     1   0.993      0.498 0.548 0.452
#&gt; GSM11235     1   0.993      0.498 0.548 0.452
#&gt; GSM28707     1   0.993      0.498 0.548 0.452
#&gt; GSM11240     2   0.000      0.928 0.000 1.000
#&gt; GSM28714     2   0.000      0.928 0.000 1.000
#&gt; GSM11216     1   0.795      0.583 0.760 0.240
#&gt; GSM28715     2   0.000      0.928 0.000 1.000
#&gt; GSM11234     2   0.000      0.928 0.000 1.000
#&gt; GSM28699     2   0.000      0.928 0.000 1.000
#&gt; GSM11233     2   0.000      0.928 0.000 1.000
#&gt; GSM28718     2   0.000      0.928 0.000 1.000
#&gt; GSM11231     2   0.000      0.928 0.000 1.000
#&gt; GSM11237     2   0.000      0.928 0.000 1.000
#&gt; GSM11228     2   0.224      0.890 0.036 0.964
#&gt; GSM28697     2   0.000      0.928 0.000 1.000
#&gt; GSM28698     1   0.795      0.583 0.760 0.240
#&gt; GSM11238     1   0.795      0.583 0.760 0.240
#&gt; GSM11242     1   0.795      0.583 0.760 0.240
#&gt; GSM28719     2   0.000      0.928 0.000 1.000
#&gt; GSM28708     2   0.697      0.649 0.188 0.812
#&gt; GSM28722     2   0.000      0.928 0.000 1.000
#&gt; GSM11232     2   0.000      0.928 0.000 1.000
#&gt; GSM28709     1   0.795      0.583 0.760 0.240
#&gt; GSM11226     2   0.730      0.618 0.204 0.796
#&gt; GSM11239     1   0.795      0.583 0.760 0.240
#&gt; GSM11225     1   0.795      0.583 0.760 0.240
#&gt; GSM11220     1   0.795      0.583 0.760 0.240
#&gt; GSM28701     2   0.000      0.928 0.000 1.000
#&gt; GSM28721     2   0.760      0.584 0.220 0.780
#&gt; GSM28713     2   0.000      0.928 0.000 1.000
#&gt; GSM28716     2   0.795      0.447 0.240 0.760
#&gt; GSM11221     2   0.000      0.928 0.000 1.000
#&gt; GSM28717     2   0.000      0.928 0.000 1.000
#&gt; GSM11223     1   0.993      0.498 0.548 0.452
#&gt; GSM11218     2   0.827      0.493 0.260 0.740
#&gt; GSM11219     2   0.000      0.928 0.000 1.000
#&gt; GSM11236     2   0.644      0.618 0.164 0.836
#&gt; GSM28702     1   0.795      0.583 0.760 0.240
#&gt; GSM28705     2   0.224      0.890 0.036 0.964
#&gt; GSM11230     2   0.000      0.928 0.000 1.000
#&gt; GSM28704     2   0.000      0.928 0.000 1.000
#&gt; GSM28700     2   0.000      0.928 0.000 1.000
#&gt; GSM11224     2   0.000      0.928 0.000 1.000
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-1-a').click(function(){
  $('#tab-SD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-2'>
<p><a id='tab-SD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28710     2  0.0000      0.985 0.000 1.000 0.000
#&gt; GSM28711     2  0.0000      0.985 0.000 1.000 0.000
#&gt; GSM28712     2  0.0000      0.985 0.000 1.000 0.000
#&gt; GSM11222     3  0.0424      1.000 0.008 0.000 0.992
#&gt; GSM28720     1  0.0000      0.943 1.000 0.000 0.000
#&gt; GSM11217     1  0.0000      0.943 1.000 0.000 0.000
#&gt; GSM28723     1  0.0000      0.943 1.000 0.000 0.000
#&gt; GSM11241     1  0.0000      0.943 1.000 0.000 0.000
#&gt; GSM28703     1  0.0000      0.943 1.000 0.000 0.000
#&gt; GSM11227     1  0.0000      0.943 1.000 0.000 0.000
#&gt; GSM28706     1  0.0000      0.943 1.000 0.000 0.000
#&gt; GSM11229     1  0.0000      0.943 1.000 0.000 0.000
#&gt; GSM11235     1  0.0000      0.943 1.000 0.000 0.000
#&gt; GSM28707     1  0.0000      0.943 1.000 0.000 0.000
#&gt; GSM11240     2  0.0000      0.985 0.000 1.000 0.000
#&gt; GSM28714     2  0.0000      0.985 0.000 1.000 0.000
#&gt; GSM11216     3  0.0424      1.000 0.008 0.000 0.992
#&gt; GSM28715     2  0.0000      0.985 0.000 1.000 0.000
#&gt; GSM11234     2  0.0000      0.985 0.000 1.000 0.000
#&gt; GSM28699     2  0.0424      0.981 0.000 0.992 0.008
#&gt; GSM11233     2  0.0424      0.981 0.000 0.992 0.008
#&gt; GSM28718     2  0.0000      0.985 0.000 1.000 0.000
#&gt; GSM11231     2  0.0000      0.985 0.000 1.000 0.000
#&gt; GSM11237     2  0.0000      0.985 0.000 1.000 0.000
#&gt; GSM11228     2  0.0747      0.975 0.000 0.984 0.016
#&gt; GSM28697     2  0.0000      0.985 0.000 1.000 0.000
#&gt; GSM28698     3  0.0424      1.000 0.008 0.000 0.992
#&gt; GSM11238     3  0.0424      1.000 0.008 0.000 0.992
#&gt; GSM11242     3  0.0424      1.000 0.008 0.000 0.992
#&gt; GSM28719     2  0.0000      0.985 0.000 1.000 0.000
#&gt; GSM28708     2  0.3412      0.873 0.000 0.876 0.124
#&gt; GSM28722     2  0.0000      0.985 0.000 1.000 0.000
#&gt; GSM11232     2  0.0000      0.985 0.000 1.000 0.000
#&gt; GSM28709     3  0.0424      1.000 0.008 0.000 0.992
#&gt; GSM11226     2  0.0747      0.975 0.000 0.984 0.016
#&gt; GSM11239     3  0.0424      1.000 0.008 0.000 0.992
#&gt; GSM11225     3  0.0424      1.000 0.008 0.000 0.992
#&gt; GSM11220     3  0.0424      1.000 0.008 0.000 0.992
#&gt; GSM28701     2  0.0000      0.985 0.000 1.000 0.000
#&gt; GSM28721     2  0.3412      0.873 0.000 0.876 0.124
#&gt; GSM28713     2  0.0000      0.985 0.000 1.000 0.000
#&gt; GSM28716     1  0.6617      0.202 0.556 0.436 0.008
#&gt; GSM11221     2  0.0000      0.985 0.000 1.000 0.000
#&gt; GSM28717     2  0.0424      0.981 0.000 0.992 0.008
#&gt; GSM11223     1  0.0000      0.943 1.000 0.000 0.000
#&gt; GSM11218     2  0.3412      0.873 0.000 0.876 0.124
#&gt; GSM11219     2  0.0000      0.985 0.000 1.000 0.000
#&gt; GSM11236     2  0.1170      0.970 0.008 0.976 0.016
#&gt; GSM28702     3  0.0424      1.000 0.008 0.000 0.992
#&gt; GSM28705     2  0.0747      0.975 0.000 0.984 0.016
#&gt; GSM11230     2  0.0000      0.985 0.000 1.000 0.000
#&gt; GSM28704     2  0.0000      0.985 0.000 1.000 0.000
#&gt; GSM28700     2  0.0000      0.985 0.000 1.000 0.000
#&gt; GSM11224     2  0.0000      0.985 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-2-a').click(function(){
  $('#tab-SD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-3'>
<p><a id='tab-SD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28710     2  0.2647      0.861 0.000 0.880 0.000 0.120
#&gt; GSM28711     2  0.4925      0.311 0.000 0.572 0.000 0.428
#&gt; GSM28712     2  0.0000      0.880 0.000 1.000 0.000 0.000
#&gt; GSM11222     3  0.0000      0.975 0.000 0.000 1.000 0.000
#&gt; GSM28720     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0188      0.998 0.996 0.000 0.000 0.004
#&gt; GSM28703     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0188      0.998 0.996 0.000 0.000 0.004
#&gt; GSM11229     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0188      0.998 0.996 0.000 0.000 0.004
#&gt; GSM11240     2  0.1716      0.891 0.000 0.936 0.000 0.064
#&gt; GSM28714     2  0.1716      0.891 0.000 0.936 0.000 0.064
#&gt; GSM11216     3  0.1118      0.973 0.000 0.000 0.964 0.036
#&gt; GSM28715     2  0.1792      0.890 0.000 0.932 0.000 0.068
#&gt; GSM11234     2  0.2530      0.863 0.000 0.888 0.000 0.112
#&gt; GSM28699     2  0.0921      0.870 0.000 0.972 0.000 0.028
#&gt; GSM11233     2  0.0817      0.869 0.000 0.976 0.000 0.024
#&gt; GSM28718     2  0.1716      0.891 0.000 0.936 0.000 0.064
#&gt; GSM11231     4  0.4999      0.153 0.000 0.492 0.000 0.508
#&gt; GSM11237     2  0.1716      0.891 0.000 0.936 0.000 0.064
#&gt; GSM11228     4  0.2149      0.936 0.000 0.088 0.000 0.912
#&gt; GSM28697     4  0.2149      0.936 0.000 0.088 0.000 0.912
#&gt; GSM28698     3  0.0921      0.974 0.000 0.000 0.972 0.028
#&gt; GSM11238     3  0.0000      0.975 0.000 0.000 1.000 0.000
#&gt; GSM11242     3  0.0817      0.972 0.000 0.000 0.976 0.024
#&gt; GSM28719     4  0.2011      0.933 0.000 0.080 0.000 0.920
#&gt; GSM28708     4  0.2197      0.932 0.000 0.080 0.004 0.916
#&gt; GSM28722     2  0.3688      0.801 0.000 0.792 0.000 0.208
#&gt; GSM11232     4  0.2149      0.936 0.000 0.088 0.000 0.912
#&gt; GSM28709     3  0.1118      0.973 0.000 0.000 0.964 0.036
#&gt; GSM11226     4  0.2149      0.936 0.000 0.088 0.000 0.912
#&gt; GSM11239     3  0.0817      0.972 0.000 0.000 0.976 0.024
#&gt; GSM11225     3  0.0817      0.972 0.000 0.000 0.976 0.024
#&gt; GSM11220     3  0.1118      0.973 0.000 0.000 0.964 0.036
#&gt; GSM28701     4  0.3444      0.816 0.000 0.184 0.000 0.816
#&gt; GSM28721     4  0.2334      0.934 0.000 0.088 0.004 0.908
#&gt; GSM28713     2  0.2530      0.863 0.000 0.888 0.000 0.112
#&gt; GSM28716     2  0.3215      0.789 0.092 0.876 0.000 0.032
#&gt; GSM11221     2  0.3266      0.842 0.000 0.832 0.000 0.168
#&gt; GSM28717     2  0.0921      0.870 0.000 0.972 0.000 0.028
#&gt; GSM11223     1  0.0188      0.998 0.996 0.000 0.000 0.004
#&gt; GSM11218     4  0.2644      0.906 0.000 0.060 0.032 0.908
#&gt; GSM11219     2  0.1716      0.891 0.000 0.936 0.000 0.064
#&gt; GSM11236     4  0.2011      0.933 0.000 0.080 0.000 0.920
#&gt; GSM28702     3  0.2011      0.914 0.000 0.000 0.920 0.080
#&gt; GSM28705     4  0.2149      0.936 0.000 0.088 0.000 0.912
#&gt; GSM11230     2  0.3266      0.841 0.000 0.832 0.000 0.168
#&gt; GSM28704     2  0.3311      0.839 0.000 0.828 0.000 0.172
#&gt; GSM28700     2  0.0188      0.881 0.000 0.996 0.000 0.004
#&gt; GSM11224     2  0.0469      0.884 0.000 0.988 0.000 0.012
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-3-a').click(function(){
  $('#tab-SD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-4'>
<p><a id='tab-SD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28710     2  0.5458     0.0836 0.000 0.552 0.000 0.068 0.380
#&gt; GSM28711     2  0.6482     0.1216 0.000 0.468 0.000 0.332 0.200
#&gt; GSM28712     2  0.2516     0.4445 0.000 0.860 0.000 0.000 0.140
#&gt; GSM11222     3  0.2612     0.8432 0.000 0.000 0.868 0.008 0.124
#&gt; GSM28720     1  0.0000     0.9769 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000     0.9769 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000     0.9769 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.1341     0.9592 0.944 0.000 0.000 0.000 0.056
#&gt; GSM28703     1  0.0000     0.9769 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000     0.9769 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.1671     0.9466 0.924 0.000 0.000 0.000 0.076
#&gt; GSM11229     1  0.0000     0.9769 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000     0.9769 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.1341     0.9592 0.944 0.000 0.000 0.000 0.056
#&gt; GSM11240     2  0.1117     0.5199 0.000 0.964 0.000 0.016 0.020
#&gt; GSM28714     2  0.1117     0.5199 0.000 0.964 0.000 0.016 0.020
#&gt; GSM11216     3  0.3053     0.8791 0.000 0.000 0.828 0.008 0.164
#&gt; GSM28715     2  0.1830     0.5398 0.000 0.932 0.000 0.028 0.040
#&gt; GSM11234     2  0.5376     0.3007 0.000 0.612 0.000 0.080 0.308
#&gt; GSM28699     2  0.4415    -0.5701 0.000 0.552 0.000 0.004 0.444
#&gt; GSM11233     2  0.4276    -0.5013 0.000 0.616 0.000 0.004 0.380
#&gt; GSM28718     2  0.1117     0.5199 0.000 0.964 0.000 0.016 0.020
#&gt; GSM11231     2  0.4640     0.0963 0.000 0.584 0.000 0.400 0.016
#&gt; GSM11237     2  0.1117     0.5199 0.000 0.964 0.000 0.016 0.020
#&gt; GSM11228     4  0.0693     0.9046 0.000 0.012 0.000 0.980 0.008
#&gt; GSM28697     4  0.0898     0.9022 0.000 0.020 0.000 0.972 0.008
#&gt; GSM28698     3  0.2233     0.8936 0.000 0.000 0.892 0.004 0.104
#&gt; GSM11238     3  0.0000     0.9003 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11242     3  0.0880     0.9000 0.000 0.000 0.968 0.000 0.032
#&gt; GSM28719     4  0.1386     0.8954 0.000 0.016 0.000 0.952 0.032
#&gt; GSM28708     4  0.1251     0.8976 0.000 0.008 0.000 0.956 0.036
#&gt; GSM28722     2  0.4890     0.4727 0.000 0.720 0.000 0.140 0.140
#&gt; GSM11232     4  0.1648     0.9018 0.000 0.020 0.000 0.940 0.040
#&gt; GSM28709     3  0.3053     0.8791 0.000 0.000 0.828 0.008 0.164
#&gt; GSM11226     4  0.3163     0.8740 0.000 0.012 0.000 0.824 0.164
#&gt; GSM11239     3  0.0880     0.9000 0.000 0.000 0.968 0.000 0.032
#&gt; GSM11225     3  0.0880     0.9000 0.000 0.000 0.968 0.000 0.032
#&gt; GSM11220     3  0.3053     0.8791 0.000 0.000 0.828 0.008 0.164
#&gt; GSM28701     4  0.4349     0.6777 0.000 0.068 0.000 0.756 0.176
#&gt; GSM28721     4  0.3203     0.8730 0.000 0.012 0.000 0.820 0.168
#&gt; GSM28713     2  0.4618     0.4742 0.000 0.724 0.000 0.068 0.208
#&gt; GSM28716     5  0.4995     0.0000 0.024 0.420 0.000 0.004 0.552
#&gt; GSM11221     2  0.4871     0.4833 0.000 0.704 0.000 0.084 0.212
#&gt; GSM28717     2  0.4415    -0.5701 0.000 0.552 0.000 0.004 0.444
#&gt; GSM11223     1  0.2074     0.9316 0.896 0.000 0.000 0.000 0.104
#&gt; GSM11218     4  0.3163     0.8740 0.000 0.012 0.000 0.824 0.164
#&gt; GSM11219     2  0.1549     0.5392 0.000 0.944 0.000 0.016 0.040
#&gt; GSM11236     4  0.1211     0.8979 0.000 0.016 0.000 0.960 0.024
#&gt; GSM28702     3  0.4094     0.7723 0.000 0.000 0.788 0.084 0.128
#&gt; GSM28705     4  0.2818     0.8864 0.000 0.012 0.000 0.856 0.132
#&gt; GSM11230     2  0.3301     0.5231 0.000 0.848 0.000 0.080 0.072
#&gt; GSM28704     2  0.4610     0.4990 0.000 0.740 0.000 0.092 0.168
#&gt; GSM28700     2  0.3636     0.1804 0.000 0.728 0.000 0.000 0.272
#&gt; GSM11224     2  0.2843     0.4680 0.000 0.848 0.000 0.008 0.144
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-4-a').click(function(){
  $('#tab-SD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-5'>
<p><a id='tab-SD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28710     5  0.7060     0.0906 0.000 0.344 0.000 0.200 0.372 0.084
#&gt; GSM28711     2  0.7454     0.1215 0.000 0.364 0.000 0.228 0.144 0.264
#&gt; GSM28712     2  0.4684     0.1638 0.000 0.656 0.000 0.088 0.256 0.000
#&gt; GSM11222     3  0.3840     0.6578 0.000 0.000 0.696 0.000 0.020 0.284
#&gt; GSM28720     1  0.0000     0.9454 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000     0.9454 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000     0.9454 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.2376     0.9087 0.888 0.000 0.000 0.068 0.044 0.000
#&gt; GSM28703     1  0.0000     0.9454 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000     0.9454 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.3566     0.8474 0.800 0.000 0.000 0.096 0.104 0.000
#&gt; GSM11229     1  0.0000     0.9454 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000     0.9454 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.2318     0.9101 0.892 0.000 0.000 0.064 0.044 0.000
#&gt; GSM11240     2  0.1141     0.4329 0.000 0.948 0.000 0.000 0.052 0.000
#&gt; GSM28714     2  0.1141     0.4329 0.000 0.948 0.000 0.000 0.052 0.000
#&gt; GSM11216     3  0.3876     0.8143 0.000 0.000 0.772 0.108 0.120 0.000
#&gt; GSM28715     2  0.0790     0.4569 0.000 0.968 0.000 0.032 0.000 0.000
#&gt; GSM11234     2  0.7273    -0.1307 0.000 0.368 0.000 0.208 0.312 0.112
#&gt; GSM28699     5  0.3634     0.6754 0.000 0.296 0.000 0.008 0.696 0.000
#&gt; GSM11233     5  0.3830     0.5822 0.000 0.376 0.000 0.004 0.620 0.000
#&gt; GSM28718     2  0.1141     0.4329 0.000 0.948 0.000 0.000 0.052 0.000
#&gt; GSM11231     2  0.5458    -0.0254 0.000 0.532 0.000 0.368 0.016 0.084
#&gt; GSM11237     2  0.1141     0.4329 0.000 0.948 0.000 0.000 0.052 0.000
#&gt; GSM11228     6  0.4141    -0.3704 0.000 0.000 0.000 0.432 0.012 0.556
#&gt; GSM28697     6  0.4152    -0.3869 0.000 0.000 0.000 0.440 0.012 0.548
#&gt; GSM28698     3  0.2897     0.8302 0.000 0.000 0.852 0.060 0.088 0.000
#&gt; GSM11238     3  0.0547     0.8366 0.000 0.000 0.980 0.000 0.020 0.000
#&gt; GSM11242     3  0.1219     0.8357 0.000 0.000 0.948 0.048 0.004 0.000
#&gt; GSM28719     4  0.4091     0.3953 0.000 0.000 0.000 0.520 0.008 0.472
#&gt; GSM28708     4  0.4098     0.3633 0.000 0.000 0.000 0.496 0.008 0.496
#&gt; GSM28722     2  0.6758     0.2932 0.000 0.480 0.000 0.208 0.072 0.240
#&gt; GSM11232     6  0.3699    -0.0419 0.000 0.004 0.000 0.336 0.000 0.660
#&gt; GSM28709     3  0.3876     0.8143 0.000 0.000 0.772 0.108 0.120 0.000
#&gt; GSM11226     6  0.0146     0.4746 0.000 0.000 0.000 0.004 0.000 0.996
#&gt; GSM11239     3  0.1219     0.8357 0.000 0.000 0.948 0.048 0.004 0.000
#&gt; GSM11225     3  0.1219     0.8357 0.000 0.000 0.948 0.048 0.004 0.000
#&gt; GSM11220     3  0.3921     0.8137 0.000 0.000 0.768 0.116 0.116 0.000
#&gt; GSM28701     4  0.5853     0.2497 0.000 0.044 0.000 0.572 0.104 0.280
#&gt; GSM28721     6  0.0000     0.4738 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM28713     2  0.6881     0.2246 0.000 0.496 0.000 0.208 0.180 0.116
#&gt; GSM28716     5  0.5651     0.5061 0.028 0.168 0.000 0.188 0.616 0.000
#&gt; GSM11221     2  0.6939     0.2692 0.000 0.492 0.000 0.212 0.152 0.144
#&gt; GSM28717     5  0.3634     0.6754 0.000 0.296 0.000 0.008 0.696 0.000
#&gt; GSM11223     1  0.4318     0.8012 0.728 0.000 0.000 0.140 0.132 0.000
#&gt; GSM11218     6  0.0146     0.4746 0.000 0.000 0.000 0.004 0.000 0.996
#&gt; GSM11219     2  0.0458     0.4543 0.000 0.984 0.000 0.016 0.000 0.000
#&gt; GSM11236     6  0.4098    -0.5430 0.000 0.000 0.000 0.496 0.008 0.496
#&gt; GSM28702     3  0.4209     0.5329 0.000 0.000 0.596 0.000 0.020 0.384
#&gt; GSM28705     6  0.2163     0.4212 0.000 0.004 0.000 0.096 0.008 0.892
#&gt; GSM11230     2  0.3790     0.4121 0.000 0.772 0.000 0.072 0.000 0.156
#&gt; GSM28704     2  0.6493     0.3334 0.000 0.540 0.000 0.212 0.080 0.168
#&gt; GSM28700     2  0.5430    -0.2231 0.000 0.512 0.000 0.108 0.376 0.004
#&gt; GSM11224     2  0.4896     0.2218 0.000 0.664 0.000 0.120 0.212 0.004
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-5-a').click(function(){
  $('#tab-SD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-kmeans-signature_compare](figure_cola/SD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-kmeans-collect-classes](figure_cola/SD-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) k
#> SD:kmeans 41     0.383 2
#> SD:kmeans 53     0.373 3
#> SD:kmeans 52     0.417 4
#> SD:kmeans 39     0.405 5
#> SD:kmeans 25     0.394 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:skmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "skmeans"]
# you can also extract it by
# res = res_list["SD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21452 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-skmeans-collect-plots](figure_cola/SD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-skmeans-select-partition-number](figure_cola/SD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.987       0.993         0.4937 0.508   0.508
#> 3 3 0.970           0.939       0.977         0.3180 0.715   0.499
#> 4 4 0.943           0.928       0.965         0.1317 0.887   0.688
#> 5 5 0.797           0.668       0.810         0.0841 0.943   0.787
#> 6 6 0.802           0.665       0.813         0.0408 0.939   0.730
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-classes'>
<ul>
<li><a href='#tab-SD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-skmeans-get-classes-1'>
<p><a id='tab-SD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28710     2  0.0000      0.991 0.000 1.000
#&gt; GSM28711     2  0.0000      0.991 0.000 1.000
#&gt; GSM28712     2  0.0000      0.991 0.000 1.000
#&gt; GSM11222     1  0.0000      0.996 1.000 0.000
#&gt; GSM28720     1  0.0672      0.996 0.992 0.008
#&gt; GSM11217     1  0.0672      0.996 0.992 0.008
#&gt; GSM28723     1  0.0672      0.996 0.992 0.008
#&gt; GSM11241     1  0.0672      0.996 0.992 0.008
#&gt; GSM28703     1  0.0672      0.996 0.992 0.008
#&gt; GSM11227     1  0.0672      0.996 0.992 0.008
#&gt; GSM28706     1  0.0672      0.996 0.992 0.008
#&gt; GSM11229     1  0.0672      0.996 0.992 0.008
#&gt; GSM11235     1  0.0672      0.996 0.992 0.008
#&gt; GSM28707     1  0.0672      0.996 0.992 0.008
#&gt; GSM11240     2  0.0000      0.991 0.000 1.000
#&gt; GSM28714     2  0.0000      0.991 0.000 1.000
#&gt; GSM11216     1  0.0000      0.996 1.000 0.000
#&gt; GSM28715     2  0.0000      0.991 0.000 1.000
#&gt; GSM11234     2  0.0000      0.991 0.000 1.000
#&gt; GSM28699     2  0.0000      0.991 0.000 1.000
#&gt; GSM11233     2  0.0000      0.991 0.000 1.000
#&gt; GSM28718     2  0.0000      0.991 0.000 1.000
#&gt; GSM11231     2  0.0000      0.991 0.000 1.000
#&gt; GSM11237     2  0.0000      0.991 0.000 1.000
#&gt; GSM11228     2  0.0672      0.987 0.008 0.992
#&gt; GSM28697     2  0.0672      0.987 0.008 0.992
#&gt; GSM28698     1  0.0000      0.996 1.000 0.000
#&gt; GSM11238     1  0.0000      0.996 1.000 0.000
#&gt; GSM11242     1  0.0000      0.996 1.000 0.000
#&gt; GSM28719     2  0.0672      0.987 0.008 0.992
#&gt; GSM28708     2  0.7376      0.746 0.208 0.792
#&gt; GSM28722     2  0.0000      0.991 0.000 1.000
#&gt; GSM11232     2  0.0672      0.987 0.008 0.992
#&gt; GSM28709     1  0.0000      0.996 1.000 0.000
#&gt; GSM11226     2  0.0672      0.987 0.008 0.992
#&gt; GSM11239     1  0.0000      0.996 1.000 0.000
#&gt; GSM11225     1  0.0000      0.996 1.000 0.000
#&gt; GSM11220     1  0.0000      0.996 1.000 0.000
#&gt; GSM28701     2  0.0672      0.987 0.008 0.992
#&gt; GSM28721     2  0.0672      0.987 0.008 0.992
#&gt; GSM28713     2  0.0000      0.991 0.000 1.000
#&gt; GSM28716     2  0.0000      0.991 0.000 1.000
#&gt; GSM11221     2  0.0000      0.991 0.000 1.000
#&gt; GSM28717     2  0.0000      0.991 0.000 1.000
#&gt; GSM11223     1  0.0672      0.996 0.992 0.008
#&gt; GSM11218     2  0.1414      0.979 0.020 0.980
#&gt; GSM11219     2  0.0000      0.991 0.000 1.000
#&gt; GSM11236     1  0.0376      0.994 0.996 0.004
#&gt; GSM28702     1  0.0000      0.996 1.000 0.000
#&gt; GSM28705     2  0.0672      0.987 0.008 0.992
#&gt; GSM11230     2  0.0000      0.991 0.000 1.000
#&gt; GSM28704     2  0.0000      0.991 0.000 1.000
#&gt; GSM28700     2  0.0000      0.991 0.000 1.000
#&gt; GSM11224     2  0.0000      0.991 0.000 1.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-1-a').click(function(){
  $('#tab-SD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-2'>
<p><a id='tab-SD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1    p2    p3
#&gt; GSM28710     2   0.000      0.982  0 1.000 0.000
#&gt; GSM28711     2   0.000      0.982  0 1.000 0.000
#&gt; GSM28712     2   0.000      0.982  0 1.000 0.000
#&gt; GSM11222     3   0.000      0.939  0 0.000 1.000
#&gt; GSM28720     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11217     1   0.000      1.000  1 0.000 0.000
#&gt; GSM28723     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11241     1   0.000      1.000  1 0.000 0.000
#&gt; GSM28703     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11227     1   0.000      1.000  1 0.000 0.000
#&gt; GSM28706     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11229     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11235     1   0.000      1.000  1 0.000 0.000
#&gt; GSM28707     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11240     2   0.000      0.982  0 1.000 0.000
#&gt; GSM28714     2   0.000      0.982  0 1.000 0.000
#&gt; GSM11216     3   0.000      0.939  0 0.000 1.000
#&gt; GSM28715     2   0.000      0.982  0 1.000 0.000
#&gt; GSM11234     2   0.000      0.982  0 1.000 0.000
#&gt; GSM28699     2   0.000      0.982  0 1.000 0.000
#&gt; GSM11233     2   0.000      0.982  0 1.000 0.000
#&gt; GSM28718     2   0.000      0.982  0 1.000 0.000
#&gt; GSM11231     2   0.000      0.982  0 1.000 0.000
#&gt; GSM11237     2   0.000      0.982  0 1.000 0.000
#&gt; GSM11228     3   0.506      0.685  0 0.244 0.756
#&gt; GSM28697     2   0.604      0.317  0 0.620 0.380
#&gt; GSM28698     3   0.000      0.939  0 0.000 1.000
#&gt; GSM11238     3   0.000      0.939  0 0.000 1.000
#&gt; GSM11242     3   0.000      0.939  0 0.000 1.000
#&gt; GSM28719     3   0.623      0.261  0 0.436 0.564
#&gt; GSM28708     3   0.000      0.939  0 0.000 1.000
#&gt; GSM28722     2   0.000      0.982  0 1.000 0.000
#&gt; GSM11232     2   0.000      0.982  0 1.000 0.000
#&gt; GSM28709     3   0.000      0.939  0 0.000 1.000
#&gt; GSM11226     3   0.000      0.939  0 0.000 1.000
#&gt; GSM11239     3   0.000      0.939  0 0.000 1.000
#&gt; GSM11225     3   0.000      0.939  0 0.000 1.000
#&gt; GSM11220     3   0.000      0.939  0 0.000 1.000
#&gt; GSM28701     2   0.000      0.982  0 1.000 0.000
#&gt; GSM28721     3   0.000      0.939  0 0.000 1.000
#&gt; GSM28713     2   0.000      0.982  0 1.000 0.000
#&gt; GSM28716     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11221     2   0.000      0.982  0 1.000 0.000
#&gt; GSM28717     2   0.000      0.982  0 1.000 0.000
#&gt; GSM11223     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11218     3   0.000      0.939  0 0.000 1.000
#&gt; GSM11219     2   0.000      0.982  0 1.000 0.000
#&gt; GSM11236     3   0.000      0.939  0 0.000 1.000
#&gt; GSM28702     3   0.000      0.939  0 0.000 1.000
#&gt; GSM28705     3   0.440      0.764  0 0.188 0.812
#&gt; GSM11230     2   0.000      0.982  0 1.000 0.000
#&gt; GSM28704     2   0.000      0.982  0 1.000 0.000
#&gt; GSM28700     2   0.000      0.982  0 1.000 0.000
#&gt; GSM11224     2   0.000      0.982  0 1.000 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-2-a').click(function(){
  $('#tab-SD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-3'>
<p><a id='tab-SD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1    p2    p3    p4
#&gt; GSM28710     2  0.0188      0.980  0 0.996 0.000 0.004
#&gt; GSM28711     2  0.0336      0.975  0 0.992 0.000 0.008
#&gt; GSM28712     2  0.0188      0.980  0 0.996 0.000 0.004
#&gt; GSM11222     3  0.0000      0.979  0 0.000 1.000 0.000
#&gt; GSM28720     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11240     2  0.0000      0.980  0 1.000 0.000 0.000
#&gt; GSM28714     2  0.0000      0.980  0 1.000 0.000 0.000
#&gt; GSM11216     3  0.0000      0.979  0 0.000 1.000 0.000
#&gt; GSM28715     2  0.0000      0.980  0 1.000 0.000 0.000
#&gt; GSM11234     2  0.0592      0.972  0 0.984 0.000 0.016
#&gt; GSM28699     2  0.0188      0.980  0 0.996 0.000 0.004
#&gt; GSM11233     2  0.0188      0.980  0 0.996 0.000 0.004
#&gt; GSM28718     2  0.0000      0.980  0 1.000 0.000 0.000
#&gt; GSM11231     2  0.4331      0.600  0 0.712 0.000 0.288
#&gt; GSM11237     2  0.0000      0.980  0 1.000 0.000 0.000
#&gt; GSM11228     4  0.0188      0.837  0 0.000 0.004 0.996
#&gt; GSM28697     4  0.0000      0.836  0 0.000 0.000 1.000
#&gt; GSM28698     3  0.0000      0.979  0 0.000 1.000 0.000
#&gt; GSM11238     3  0.0000      0.979  0 0.000 1.000 0.000
#&gt; GSM11242     3  0.0000      0.979  0 0.000 1.000 0.000
#&gt; GSM28719     4  0.0188      0.837  0 0.000 0.004 0.996
#&gt; GSM28708     4  0.0188      0.837  0 0.000 0.004 0.996
#&gt; GSM28722     2  0.1302      0.942  0 0.956 0.000 0.044
#&gt; GSM11232     4  0.0188      0.835  0 0.004 0.000 0.996
#&gt; GSM28709     3  0.0000      0.979  0 0.000 1.000 0.000
#&gt; GSM11226     4  0.4761      0.526  0 0.000 0.372 0.628
#&gt; GSM11239     3  0.0000      0.979  0 0.000 1.000 0.000
#&gt; GSM11225     3  0.0000      0.979  0 0.000 1.000 0.000
#&gt; GSM11220     3  0.0000      0.979  0 0.000 1.000 0.000
#&gt; GSM28701     4  0.3975      0.613  0 0.240 0.000 0.760
#&gt; GSM28721     4  0.3266      0.772  0 0.000 0.168 0.832
#&gt; GSM28713     2  0.0188      0.980  0 0.996 0.000 0.004
#&gt; GSM28716     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11221     2  0.0000      0.980  0 1.000 0.000 0.000
#&gt; GSM28717     2  0.0188      0.980  0 0.996 0.000 0.004
#&gt; GSM11223     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11218     4  0.4761      0.526  0 0.000 0.372 0.628
#&gt; GSM11219     2  0.0000      0.980  0 1.000 0.000 0.000
#&gt; GSM11236     3  0.3400      0.754  0 0.000 0.820 0.180
#&gt; GSM28702     3  0.0000      0.979  0 0.000 1.000 0.000
#&gt; GSM28705     4  0.3024      0.786  0 0.000 0.148 0.852
#&gt; GSM11230     2  0.0000      0.980  0 1.000 0.000 0.000
#&gt; GSM28704     2  0.0000      0.980  0 1.000 0.000 0.000
#&gt; GSM28700     2  0.0188      0.980  0 0.996 0.000 0.004
#&gt; GSM11224     2  0.0188      0.980  0 0.996 0.000 0.004
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-3-a').click(function(){
  $('#tab-SD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-4'>
<p><a id='tab-SD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette   p1    p2    p3    p4    p5
#&gt; GSM28710     5   0.407     0.4450 0.00 0.324 0.000 0.004 0.672
#&gt; GSM28711     2   0.622     0.0453 0.00 0.448 0.000 0.140 0.412
#&gt; GSM28712     2   0.359     0.2283 0.00 0.736 0.000 0.000 0.264
#&gt; GSM11222     3   0.088     0.9498 0.00 0.000 0.968 0.032 0.000
#&gt; GSM28720     1   0.000     0.9797 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11217     1   0.000     0.9797 1.00 0.000 0.000 0.000 0.000
#&gt; GSM28723     1   0.000     0.9797 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11241     1   0.000     0.9797 1.00 0.000 0.000 0.000 0.000
#&gt; GSM28703     1   0.000     0.9797 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11227     1   0.000     0.9797 1.00 0.000 0.000 0.000 0.000
#&gt; GSM28706     1   0.000     0.9797 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11229     1   0.000     0.9797 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11235     1   0.000     0.9797 1.00 0.000 0.000 0.000 0.000
#&gt; GSM28707     1   0.000     0.9797 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11240     2   0.000     0.5839 0.00 1.000 0.000 0.000 0.000
#&gt; GSM28714     2   0.000     0.5839 0.00 1.000 0.000 0.000 0.000
#&gt; GSM11216     3   0.000     0.9694 0.00 0.000 1.000 0.000 0.000
#&gt; GSM28715     2   0.191     0.5907 0.00 0.908 0.000 0.000 0.092
#&gt; GSM11234     5   0.492     0.1061 0.00 0.420 0.000 0.028 0.552
#&gt; GSM28699     5   0.428     0.4399 0.00 0.456 0.000 0.000 0.544
#&gt; GSM11233     2   0.427    -0.3935 0.00 0.552 0.000 0.000 0.448
#&gt; GSM28718     2   0.000     0.5839 0.00 1.000 0.000 0.000 0.000
#&gt; GSM11231     2   0.441     0.2817 0.00 0.724 0.000 0.044 0.232
#&gt; GSM11237     2   0.000     0.5839 0.00 1.000 0.000 0.000 0.000
#&gt; GSM11228     4   0.353     0.8019 0.00 0.000 0.000 0.744 0.256
#&gt; GSM28697     4   0.361     0.7979 0.00 0.000 0.000 0.732 0.268
#&gt; GSM28698     3   0.000     0.9694 0.00 0.000 1.000 0.000 0.000
#&gt; GSM11238     3   0.000     0.9694 0.00 0.000 1.000 0.000 0.000
#&gt; GSM11242     3   0.000     0.9694 0.00 0.000 1.000 0.000 0.000
#&gt; GSM28719     4   0.361     0.7976 0.00 0.000 0.000 0.732 0.268
#&gt; GSM28708     4   0.353     0.8016 0.00 0.000 0.000 0.744 0.256
#&gt; GSM28722     2   0.554     0.4441 0.00 0.648 0.000 0.164 0.188
#&gt; GSM11232     4   0.314     0.7932 0.00 0.000 0.000 0.796 0.204
#&gt; GSM28709     3   0.000     0.9694 0.00 0.000 1.000 0.000 0.000
#&gt; GSM11226     4   0.297     0.6833 0.00 0.000 0.184 0.816 0.000
#&gt; GSM11239     3   0.000     0.9694 0.00 0.000 1.000 0.000 0.000
#&gt; GSM11225     3   0.000     0.9694 0.00 0.000 1.000 0.000 0.000
#&gt; GSM11220     3   0.000     0.9694 0.00 0.000 1.000 0.000 0.000
#&gt; GSM28701     5   0.477    -0.1232 0.00 0.068 0.000 0.228 0.704
#&gt; GSM28721     4   0.148     0.7599 0.00 0.000 0.048 0.944 0.008
#&gt; GSM28713     2   0.491     0.3340 0.00 0.588 0.000 0.032 0.380
#&gt; GSM28716     1   0.327     0.7294 0.78 0.000 0.000 0.000 0.220
#&gt; GSM11221     2   0.499     0.4306 0.00 0.628 0.000 0.048 0.324
#&gt; GSM28717     5   0.428     0.4399 0.00 0.456 0.000 0.000 0.544
#&gt; GSM11223     1   0.000     0.9797 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11218     4   0.342     0.6204 0.00 0.000 0.240 0.760 0.000
#&gt; GSM11219     2   0.185     0.5913 0.00 0.912 0.000 0.000 0.088
#&gt; GSM11236     3   0.343     0.8029 0.00 0.000 0.836 0.056 0.108
#&gt; GSM28702     3   0.207     0.8903 0.00 0.000 0.896 0.104 0.000
#&gt; GSM28705     4   0.140     0.7596 0.00 0.000 0.024 0.952 0.024
#&gt; GSM11230     2   0.380     0.5486 0.00 0.804 0.000 0.056 0.140
#&gt; GSM28704     2   0.478     0.4894 0.00 0.692 0.000 0.060 0.248
#&gt; GSM28700     2   0.420    -0.1890 0.00 0.592 0.000 0.000 0.408
#&gt; GSM11224     2   0.364     0.2935 0.00 0.728 0.000 0.000 0.272
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-4-a').click(function(){
  $('#tab-SD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-5'>
<p><a id='tab-SD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette   p1    p2    p3    p4    p5    p6
#&gt; GSM28710     5  0.4130     0.5909 0.00 0.092 0.000 0.032 0.784 0.092
#&gt; GSM28711     2  0.6817     0.0152 0.00 0.372 0.000 0.044 0.320 0.264
#&gt; GSM28712     2  0.4704    -0.1533 0.00 0.488 0.000 0.000 0.468 0.044
#&gt; GSM11222     3  0.2092     0.8296 0.00 0.000 0.876 0.000 0.000 0.124
#&gt; GSM28720     1  0.0000     0.9677 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000     0.9677 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000     0.9677 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000     0.9677 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000     0.9677 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000     0.9677 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000     0.9677 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000     0.9677 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000     0.9677 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000     0.9677 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.2092     0.5393 0.00 0.876 0.000 0.000 0.124 0.000
#&gt; GSM28714     2  0.2092     0.5393 0.00 0.876 0.000 0.000 0.124 0.000
#&gt; GSM11216     3  0.0000     0.9278 0.00 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28715     2  0.0508     0.5647 0.00 0.984 0.000 0.004 0.012 0.000
#&gt; GSM11234     5  0.6082     0.2339 0.00 0.264 0.000 0.024 0.528 0.184
#&gt; GSM28699     5  0.1957     0.6750 0.00 0.112 0.000 0.000 0.888 0.000
#&gt; GSM11233     5  0.3198     0.5662 0.00 0.260 0.000 0.000 0.740 0.000
#&gt; GSM28718     2  0.2092     0.5393 0.00 0.876 0.000 0.000 0.124 0.000
#&gt; GSM11231     4  0.4591     0.1178 0.00 0.464 0.000 0.500 0.036 0.000
#&gt; GSM11237     2  0.2092     0.5393 0.00 0.876 0.000 0.000 0.124 0.000
#&gt; GSM11228     4  0.2436     0.6239 0.00 0.000 0.000 0.880 0.032 0.088
#&gt; GSM28697     4  0.2164     0.6427 0.00 0.000 0.000 0.900 0.032 0.068
#&gt; GSM28698     3  0.0000     0.9278 0.00 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11238     3  0.0000     0.9278 0.00 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11242     3  0.0000     0.9278 0.00 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28719     4  0.0260     0.6664 0.00 0.000 0.000 0.992 0.000 0.008
#&gt; GSM28708     4  0.0713     0.6612 0.00 0.000 0.000 0.972 0.000 0.028
#&gt; GSM28722     2  0.5957     0.3496 0.00 0.508 0.000 0.024 0.132 0.336
#&gt; GSM11232     4  0.4361     0.4706 0.00 0.040 0.000 0.736 0.032 0.192
#&gt; GSM28709     3  0.0000     0.9278 0.00 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11226     6  0.4281     0.9103 0.00 0.000 0.072 0.220 0.000 0.708
#&gt; GSM11239     3  0.0000     0.9278 0.00 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11225     3  0.0000     0.9278 0.00 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11220     3  0.0000     0.9278 0.00 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28701     4  0.5876     0.4659 0.00 0.056 0.000 0.616 0.164 0.164
#&gt; GSM28721     6  0.3688     0.9055 0.00 0.000 0.020 0.256 0.000 0.724
#&gt; GSM28713     2  0.6061     0.1443 0.00 0.448 0.000 0.004 0.312 0.236
#&gt; GSM28716     1  0.3578     0.4965 0.66 0.000 0.000 0.000 0.340 0.000
#&gt; GSM11221     2  0.5648     0.3470 0.00 0.536 0.000 0.000 0.224 0.240
#&gt; GSM28717     5  0.2003     0.6757 0.00 0.116 0.000 0.000 0.884 0.000
#&gt; GSM11223     1  0.0000     0.9677 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     6  0.4428     0.8973 0.00 0.000 0.084 0.220 0.000 0.696
#&gt; GSM11219     2  0.1261     0.5663 0.00 0.952 0.000 0.000 0.024 0.024
#&gt; GSM11236     3  0.3519     0.6755 0.00 0.000 0.752 0.232 0.008 0.008
#&gt; GSM28702     3  0.3428     0.5896 0.00 0.000 0.696 0.000 0.000 0.304
#&gt; GSM28705     6  0.3650     0.8722 0.00 0.000 0.004 0.272 0.008 0.716
#&gt; GSM11230     2  0.3130     0.5306 0.00 0.828 0.000 0.000 0.048 0.124
#&gt; GSM28704     2  0.5183     0.4244 0.00 0.604 0.000 0.000 0.140 0.256
#&gt; GSM28700     5  0.5104     0.3647 0.00 0.304 0.000 0.000 0.588 0.108
#&gt; GSM11224     2  0.5442    -0.0385 0.00 0.468 0.000 0.000 0.412 0.120
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-5-a').click(function(){
  $('#tab-SD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-skmeans-signature_compare](figure_cola/SD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-skmeans-collect-classes](figure_cola/SD-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> SD:skmeans 54     0.398 2
#> SD:skmeans 52     0.372 3
#> SD:skmeans 54     0.355 4
#> SD:skmeans 39     0.405 5
#> SD:skmeans 41     0.389 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:pam*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "pam"]
# you can also extract it by
# res = res_list["SD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21452 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-pam-collect-plots](figure_cola/SD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-pam-select-partition-number](figure_cola/SD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.968       0.988         0.4675 0.535   0.535
#> 3 3 0.915           0.941       0.975         0.3338 0.770   0.596
#> 4 4 0.765           0.801       0.885         0.1409 0.901   0.745
#> 5 5 0.772           0.677       0.821         0.0937 0.877   0.607
#> 6 6 0.775           0.669       0.833         0.0297 0.857   0.504
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-classes'>
<ul>
<li><a href='#tab-SD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-pam-get-classes-1'>
<p><a id='tab-SD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28710     2  0.0000      0.987 0.000 1.000
#&gt; GSM28711     2  0.0000      0.987 0.000 1.000
#&gt; GSM28712     2  0.0000      0.987 0.000 1.000
#&gt; GSM11222     2  0.0000      0.987 0.000 1.000
#&gt; GSM28720     1  0.0000      0.988 1.000 0.000
#&gt; GSM11217     1  0.0000      0.988 1.000 0.000
#&gt; GSM28723     1  0.0000      0.988 1.000 0.000
#&gt; GSM11241     1  0.0000      0.988 1.000 0.000
#&gt; GSM28703     1  0.0000      0.988 1.000 0.000
#&gt; GSM11227     1  0.0000      0.988 1.000 0.000
#&gt; GSM28706     1  0.0000      0.988 1.000 0.000
#&gt; GSM11229     1  0.0000      0.988 1.000 0.000
#&gt; GSM11235     1  0.0000      0.988 1.000 0.000
#&gt; GSM28707     1  0.0000      0.988 1.000 0.000
#&gt; GSM11240     2  0.0000      0.987 0.000 1.000
#&gt; GSM28714     2  0.0000      0.987 0.000 1.000
#&gt; GSM11216     1  0.0000      0.988 1.000 0.000
#&gt; GSM28715     2  0.0000      0.987 0.000 1.000
#&gt; GSM11234     2  0.0000      0.987 0.000 1.000
#&gt; GSM28699     2  0.0000      0.987 0.000 1.000
#&gt; GSM11233     2  0.0000      0.987 0.000 1.000
#&gt; GSM28718     2  0.0000      0.987 0.000 1.000
#&gt; GSM11231     2  0.0000      0.987 0.000 1.000
#&gt; GSM11237     2  0.0000      0.987 0.000 1.000
#&gt; GSM11228     2  0.0000      0.987 0.000 1.000
#&gt; GSM28697     2  0.0000      0.987 0.000 1.000
#&gt; GSM28698     1  0.0000      0.988 1.000 0.000
#&gt; GSM11238     1  0.0376      0.985 0.996 0.004
#&gt; GSM11242     1  0.7056      0.757 0.808 0.192
#&gt; GSM28719     2  0.0000      0.987 0.000 1.000
#&gt; GSM28708     2  0.0000      0.987 0.000 1.000
#&gt; GSM28722     2  0.0000      0.987 0.000 1.000
#&gt; GSM11232     2  0.0000      0.987 0.000 1.000
#&gt; GSM28709     2  0.9896      0.193 0.440 0.560
#&gt; GSM11226     2  0.0000      0.987 0.000 1.000
#&gt; GSM11239     1  0.0000      0.988 1.000 0.000
#&gt; GSM11225     1  0.0000      0.988 1.000 0.000
#&gt; GSM11220     1  0.0000      0.988 1.000 0.000
#&gt; GSM28701     2  0.0000      0.987 0.000 1.000
#&gt; GSM28721     2  0.0000      0.987 0.000 1.000
#&gt; GSM28713     2  0.0000      0.987 0.000 1.000
#&gt; GSM28716     1  0.0672      0.982 0.992 0.008
#&gt; GSM11221     2  0.0000      0.987 0.000 1.000
#&gt; GSM28717     2  0.0000      0.987 0.000 1.000
#&gt; GSM11223     1  0.0000      0.988 1.000 0.000
#&gt; GSM11218     2  0.0000      0.987 0.000 1.000
#&gt; GSM11219     2  0.0000      0.987 0.000 1.000
#&gt; GSM11236     2  0.0000      0.987 0.000 1.000
#&gt; GSM28702     2  0.0000      0.987 0.000 1.000
#&gt; GSM28705     2  0.0000      0.987 0.000 1.000
#&gt; GSM11230     2  0.0000      0.987 0.000 1.000
#&gt; GSM28704     2  0.0000      0.987 0.000 1.000
#&gt; GSM28700     2  0.0000      0.987 0.000 1.000
#&gt; GSM11224     2  0.0000      0.987 0.000 1.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-1-a').click(function(){
  $('#tab-SD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-2'>
<p><a id='tab-SD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28710     2  0.0000      0.973 0.000 1.000 0.000
#&gt; GSM28711     2  0.0000      0.973 0.000 1.000 0.000
#&gt; GSM28712     2  0.0000      0.973 0.000 1.000 0.000
#&gt; GSM11222     3  0.0000      0.938 0.000 0.000 1.000
#&gt; GSM28720     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM11217     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM28723     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM11241     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM28703     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM11227     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM28706     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM11229     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM11235     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM28707     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM11240     2  0.0000      0.973 0.000 1.000 0.000
#&gt; GSM28714     2  0.0000      0.973 0.000 1.000 0.000
#&gt; GSM11216     3  0.0000      0.938 0.000 0.000 1.000
#&gt; GSM28715     2  0.0000      0.973 0.000 1.000 0.000
#&gt; GSM11234     2  0.0000      0.973 0.000 1.000 0.000
#&gt; GSM28699     2  0.0000      0.973 0.000 1.000 0.000
#&gt; GSM11233     2  0.0000      0.973 0.000 1.000 0.000
#&gt; GSM28718     2  0.0000      0.973 0.000 1.000 0.000
#&gt; GSM11231     2  0.0000      0.973 0.000 1.000 0.000
#&gt; GSM11237     2  0.0000      0.973 0.000 1.000 0.000
#&gt; GSM11228     2  0.5733      0.494 0.000 0.676 0.324
#&gt; GSM28697     2  0.0000      0.973 0.000 1.000 0.000
#&gt; GSM28698     3  0.0000      0.938 0.000 0.000 1.000
#&gt; GSM11238     3  0.0000      0.938 0.000 0.000 1.000
#&gt; GSM11242     3  0.0000      0.938 0.000 0.000 1.000
#&gt; GSM28719     2  0.0747      0.958 0.000 0.984 0.016
#&gt; GSM28708     3  0.4291      0.809 0.000 0.180 0.820
#&gt; GSM28722     2  0.0000      0.973 0.000 1.000 0.000
#&gt; GSM11232     2  0.0000      0.973 0.000 1.000 0.000
#&gt; GSM28709     3  0.0000      0.938 0.000 0.000 1.000
#&gt; GSM11226     3  0.3816      0.845 0.000 0.148 0.852
#&gt; GSM11239     3  0.0000      0.938 0.000 0.000 1.000
#&gt; GSM11225     3  0.0000      0.938 0.000 0.000 1.000
#&gt; GSM11220     3  0.0000      0.938 0.000 0.000 1.000
#&gt; GSM28701     2  0.0000      0.973 0.000 1.000 0.000
#&gt; GSM28721     3  0.3816      0.845 0.000 0.148 0.852
#&gt; GSM28713     2  0.0000      0.973 0.000 1.000 0.000
#&gt; GSM28716     1  0.1964      0.923 0.944 0.056 0.000
#&gt; GSM11221     2  0.0000      0.973 0.000 1.000 0.000
#&gt; GSM28717     2  0.0000      0.973 0.000 1.000 0.000
#&gt; GSM11223     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM11218     3  0.3816      0.845 0.000 0.148 0.852
#&gt; GSM11219     2  0.0000      0.973 0.000 1.000 0.000
#&gt; GSM11236     2  0.0000      0.973 0.000 1.000 0.000
#&gt; GSM28702     3  0.0000      0.938 0.000 0.000 1.000
#&gt; GSM28705     2  0.5760      0.485 0.000 0.672 0.328
#&gt; GSM11230     2  0.0000      0.973 0.000 1.000 0.000
#&gt; GSM28704     2  0.0000      0.973 0.000 1.000 0.000
#&gt; GSM28700     2  0.0000      0.973 0.000 1.000 0.000
#&gt; GSM11224     2  0.0000      0.973 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-2-a').click(function(){
  $('#tab-SD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-3'>
<p><a id='tab-SD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28710     2  0.0000      0.823 0.000 1.000 0.000 0.000
#&gt; GSM28711     2  0.4790      0.550 0.000 0.620 0.000 0.380
#&gt; GSM28712     2  0.0000      0.823 0.000 1.000 0.000 0.000
#&gt; GSM11222     4  0.4999      0.053 0.000 0.000 0.492 0.508
#&gt; GSM28720     1  0.0000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.4304      0.723 0.000 0.716 0.000 0.284
#&gt; GSM28714     2  0.4304      0.723 0.000 0.716 0.000 0.284
#&gt; GSM11216     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28715     2  0.4304      0.723 0.000 0.716 0.000 0.284
#&gt; GSM11234     2  0.0000      0.823 0.000 1.000 0.000 0.000
#&gt; GSM28699     2  0.0000      0.823 0.000 1.000 0.000 0.000
#&gt; GSM11233     2  0.4304      0.723 0.000 0.716 0.000 0.284
#&gt; GSM28718     2  0.4304      0.723 0.000 0.716 0.000 0.284
#&gt; GSM11231     2  0.4304      0.723 0.000 0.716 0.000 0.284
#&gt; GSM11237     2  0.4304      0.723 0.000 0.716 0.000 0.284
#&gt; GSM11228     4  0.4304      0.773 0.000 0.284 0.000 0.716
#&gt; GSM28697     2  0.4103      0.425 0.000 0.744 0.000 0.256
#&gt; GSM28698     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11238     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11242     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28719     4  0.4992      0.449 0.000 0.476 0.000 0.524
#&gt; GSM28708     4  0.1022      0.566 0.000 0.000 0.032 0.968
#&gt; GSM28722     2  0.0000      0.823 0.000 1.000 0.000 0.000
#&gt; GSM11232     2  0.0817      0.803 0.000 0.976 0.000 0.024
#&gt; GSM28709     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11226     4  0.5025      0.790 0.000 0.252 0.032 0.716
#&gt; GSM11239     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11225     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11220     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28701     2  0.0000      0.823 0.000 1.000 0.000 0.000
#&gt; GSM28721     4  0.5025      0.790 0.000 0.252 0.032 0.716
#&gt; GSM28713     2  0.0000      0.823 0.000 1.000 0.000 0.000
#&gt; GSM28716     1  0.4304      0.503 0.716 0.284 0.000 0.000
#&gt; GSM11221     2  0.0000      0.823 0.000 1.000 0.000 0.000
#&gt; GSM28717     2  0.0000      0.823 0.000 1.000 0.000 0.000
#&gt; GSM11223     1  0.0000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM11218     4  0.5025      0.790 0.000 0.252 0.032 0.716
#&gt; GSM11219     2  0.0000      0.823 0.000 1.000 0.000 0.000
#&gt; GSM11236     2  0.4927      0.376 0.000 0.712 0.024 0.264
#&gt; GSM28702     4  0.4304      0.487 0.000 0.000 0.284 0.716
#&gt; GSM28705     4  0.4304      0.773 0.000 0.284 0.000 0.716
#&gt; GSM11230     2  0.4304      0.723 0.000 0.716 0.000 0.284
#&gt; GSM28704     2  0.0000      0.823 0.000 1.000 0.000 0.000
#&gt; GSM28700     2  0.0000      0.823 0.000 1.000 0.000 0.000
#&gt; GSM11224     2  0.0000      0.823 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-3-a').click(function(){
  $('#tab-SD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-4'>
<p><a id='tab-SD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28710     2  0.0162     0.5681 0.000 0.996 0.000 0.000 0.004
#&gt; GSM28711     4  0.6569    -0.0932 0.000 0.304 0.000 0.464 0.232
#&gt; GSM28712     2  0.0000     0.5683 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11222     4  0.3177     0.6096 0.000 0.000 0.208 0.792 0.000
#&gt; GSM28720     1  0.0000     0.9637 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000     0.9637 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000     0.9637 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000     0.9637 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000     0.9637 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000     0.9637 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000     0.9637 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000     0.9637 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000     0.9637 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000     0.9637 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     5  0.4294     0.7820 0.000 0.468 0.000 0.000 0.532
#&gt; GSM28714     5  0.4291     0.7827 0.000 0.464 0.000 0.000 0.536
#&gt; GSM11216     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28715     5  0.4294     0.7820 0.000 0.468 0.000 0.000 0.532
#&gt; GSM11234     2  0.0000     0.5683 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28699     2  0.3932     0.2977 0.000 0.672 0.000 0.000 0.328
#&gt; GSM11233     5  0.4101     0.0305 0.000 0.372 0.000 0.000 0.628
#&gt; GSM28718     5  0.4291     0.7827 0.000 0.464 0.000 0.000 0.536
#&gt; GSM11231     2  0.4300    -0.3167 0.000 0.524 0.000 0.000 0.476
#&gt; GSM11237     5  0.4262     0.7389 0.000 0.440 0.000 0.000 0.560
#&gt; GSM11228     4  0.5355     0.5739 0.000 0.220 0.000 0.660 0.120
#&gt; GSM28697     2  0.4805     0.3494 0.000 0.728 0.000 0.144 0.128
#&gt; GSM28698     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11238     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11242     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28719     4  0.6109     0.4720 0.000 0.320 0.000 0.532 0.148
#&gt; GSM28708     4  0.2471     0.7391 0.000 0.000 0.000 0.864 0.136
#&gt; GSM28722     2  0.3242     0.4509 0.000 0.784 0.000 0.000 0.216
#&gt; GSM11232     2  0.4192     0.4140 0.000 0.736 0.000 0.032 0.232
#&gt; GSM28709     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11226     4  0.0000     0.7711 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11239     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11225     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11220     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28701     2  0.3586     0.4356 0.000 0.736 0.000 0.000 0.264
#&gt; GSM28721     4  0.0000     0.7711 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28713     2  0.0000     0.5683 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28716     1  0.4084     0.4898 0.668 0.328 0.000 0.000 0.004
#&gt; GSM11221     2  0.3274     0.4465 0.000 0.780 0.000 0.000 0.220
#&gt; GSM28717     2  0.3949     0.2945 0.000 0.668 0.000 0.000 0.332
#&gt; GSM11223     1  0.0000     0.9637 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     4  0.0000     0.7711 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11219     2  0.3366     0.4217 0.000 0.768 0.000 0.000 0.232
#&gt; GSM11236     4  0.5845     0.1271 0.000 0.352 0.000 0.540 0.108
#&gt; GSM28702     4  0.0000     0.7711 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28705     4  0.0000     0.7711 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11230     5  0.4294     0.7820 0.000 0.468 0.000 0.000 0.532
#&gt; GSM28704     2  0.3274     0.4465 0.000 0.780 0.000 0.000 0.220
#&gt; GSM28700     2  0.0162     0.5681 0.000 0.996 0.000 0.000 0.004
#&gt; GSM11224     2  0.3274     0.4465 0.000 0.780 0.000 0.000 0.220
</code></pre>

<script>
$('#tab-SD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-4-a').click(function(){
  $('#tab-SD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-5'>
<p><a id='tab-SD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28710     2  0.5951     0.4292 0.000 0.456 0.000 0.000 0.268 0.276
#&gt; GSM28711     2  0.5586     0.1286 0.000 0.544 0.000 0.260 0.000 0.196
#&gt; GSM28712     2  0.5951     0.4292 0.000 0.456 0.000 0.000 0.268 0.276
#&gt; GSM11222     6  0.5711     0.6693 0.000 0.000 0.208 0.276 0.000 0.516
#&gt; GSM28720     1  0.0000     0.9608 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000     0.9608 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000     0.9608 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000     0.9608 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000     0.9608 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000     0.9608 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000     0.9608 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000     0.9608 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000     0.9608 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000     0.9608 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.0000     0.5499 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28714     2  0.0000     0.5499 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11216     3  0.4332     0.7881 0.000 0.000 0.672 0.052 0.276 0.000
#&gt; GSM28715     2  0.0000     0.5499 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11234     2  0.5951     0.4292 0.000 0.456 0.000 0.000 0.268 0.276
#&gt; GSM28699     5  0.3288     0.6598 0.000 0.000 0.000 0.000 0.724 0.276
#&gt; GSM11233     5  0.3409     0.3584 0.000 0.300 0.000 0.000 0.700 0.000
#&gt; GSM28718     2  0.0000     0.5499 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11231     2  0.4086    -0.1789 0.000 0.528 0.000 0.464 0.008 0.000
#&gt; GSM11237     2  0.2100     0.4867 0.000 0.884 0.000 0.112 0.004 0.000
#&gt; GSM11228     4  0.3607     0.6157 0.000 0.000 0.000 0.652 0.000 0.348
#&gt; GSM28697     4  0.6543     0.2346 0.000 0.080 0.000 0.472 0.116 0.332
#&gt; GSM28698     3  0.0622     0.8737 0.000 0.000 0.980 0.008 0.012 0.000
#&gt; GSM11238     3  0.0000     0.8757 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11242     3  0.0000     0.8757 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28719     4  0.3098     0.6433 0.000 0.024 0.000 0.812 0.000 0.164
#&gt; GSM28708     4  0.1141     0.3823 0.000 0.000 0.000 0.948 0.000 0.052
#&gt; GSM28722     2  0.4358     0.6129 0.000 0.680 0.000 0.012 0.032 0.276
#&gt; GSM11232     2  0.5777     0.5608 0.000 0.556 0.000 0.124 0.024 0.296
#&gt; GSM28709     3  0.4332     0.7881 0.000 0.000 0.672 0.052 0.276 0.000
#&gt; GSM11226     6  0.3288     0.9377 0.000 0.000 0.000 0.276 0.000 0.724
#&gt; GSM11239     3  0.0000     0.8757 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11225     3  0.0000     0.8757 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11220     3  0.4332     0.7881 0.000 0.000 0.672 0.052 0.276 0.000
#&gt; GSM28701     2  0.5341     0.5616 0.000 0.588 0.000 0.132 0.004 0.276
#&gt; GSM28721     6  0.3288     0.9377 0.000 0.000 0.000 0.276 0.000 0.724
#&gt; GSM28713     2  0.5951     0.4292 0.000 0.456 0.000 0.000 0.268 0.276
#&gt; GSM28716     1  0.4388     0.3807 0.668 0.000 0.000 0.000 0.056 0.276
#&gt; GSM11221     2  0.3950     0.6164 0.000 0.696 0.000 0.000 0.028 0.276
#&gt; GSM28717     5  0.3288     0.6598 0.000 0.000 0.000 0.000 0.724 0.276
#&gt; GSM11223     1  0.0000     0.9608 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     6  0.3288     0.9377 0.000 0.000 0.000 0.276 0.000 0.724
#&gt; GSM11219     2  0.3876     0.6169 0.000 0.700 0.000 0.000 0.024 0.276
#&gt; GSM11236     2  0.5839    -0.0169 0.000 0.408 0.000 0.404 0.000 0.188
#&gt; GSM28702     6  0.3288     0.9377 0.000 0.000 0.000 0.276 0.000 0.724
#&gt; GSM28705     6  0.3288     0.9377 0.000 0.000 0.000 0.276 0.000 0.724
#&gt; GSM11230     2  0.0000     0.5499 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28704     2  0.3950     0.6164 0.000 0.696 0.000 0.000 0.028 0.276
#&gt; GSM28700     2  0.5951     0.4292 0.000 0.456 0.000 0.000 0.268 0.276
#&gt; GSM11224     2  0.3950     0.6164 0.000 0.696 0.000 0.000 0.028 0.276
</code></pre>

<script>
$('#tab-SD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-5-a').click(function(){
  $('#tab-SD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-SD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-pam-signature_compare](figure_cola/SD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-SD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-pam-collect-classes](figure_cola/SD-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n tissue(p) k
#> SD:pam 53     0.397 2
#> SD:pam 52     0.372 3
#> SD:pam 49     0.348 4
#> SD:pam 38     0.394 5
#> SD:pam 41     0.389 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "mclust"]
# you can also extract it by
# res = res_list["SD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21452 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-mclust-collect-plots](figure_cola/SD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-mclust-select-partition-number](figure_cola/SD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.476           0.690       0.847         0.4180 0.648   0.648
#> 3 3 1.000           0.981       0.990         0.2795 0.778   0.670
#> 4 4 0.767           0.877       0.915         0.3247 0.807   0.593
#> 5 5 0.728           0.741       0.859         0.1006 0.903   0.661
#> 6 6 0.783           0.790       0.864         0.0289 0.955   0.796
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-classes'>
<ul>
<li><a href='#tab-SD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-mclust-get-classes-1'>
<p><a id='tab-SD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28710     2  0.9988     0.4146 0.480 0.520
#&gt; GSM28711     2  0.0938     0.7633 0.012 0.988
#&gt; GSM28712     2  0.9988     0.4146 0.480 0.520
#&gt; GSM11222     2  0.0000     0.7664 0.000 1.000
#&gt; GSM28720     1  0.0000     0.9517 1.000 0.000
#&gt; GSM11217     1  0.0000     0.9517 1.000 0.000
#&gt; GSM28723     1  0.0000     0.9517 1.000 0.000
#&gt; GSM11241     1  0.0000     0.9517 1.000 0.000
#&gt; GSM28703     1  0.0000     0.9517 1.000 0.000
#&gt; GSM11227     1  0.0000     0.9517 1.000 0.000
#&gt; GSM28706     1  0.0000     0.9517 1.000 0.000
#&gt; GSM11229     1  0.0000     0.9517 1.000 0.000
#&gt; GSM11235     1  0.0000     0.9517 1.000 0.000
#&gt; GSM28707     1  0.0000     0.9517 1.000 0.000
#&gt; GSM11240     2  0.9988     0.4146 0.480 0.520
#&gt; GSM28714     2  0.9988     0.4146 0.480 0.520
#&gt; GSM11216     2  0.0000     0.7664 0.000 1.000
#&gt; GSM28715     2  0.9983     0.4207 0.476 0.524
#&gt; GSM11234     2  0.5178     0.7257 0.116 0.884
#&gt; GSM28699     2  0.9988     0.4146 0.480 0.520
#&gt; GSM11233     2  0.9988     0.4146 0.480 0.520
#&gt; GSM28718     2  0.9988     0.4146 0.480 0.520
#&gt; GSM11231     2  0.9983     0.4207 0.476 0.524
#&gt; GSM11237     2  0.9988     0.4146 0.480 0.520
#&gt; GSM11228     2  0.0000     0.7664 0.000 1.000
#&gt; GSM28697     2  0.0000     0.7664 0.000 1.000
#&gt; GSM28698     2  0.0000     0.7664 0.000 1.000
#&gt; GSM11238     2  0.0000     0.7664 0.000 1.000
#&gt; GSM11242     2  0.0000     0.7664 0.000 1.000
#&gt; GSM28719     2  0.0000     0.7664 0.000 1.000
#&gt; GSM28708     2  0.0000     0.7664 0.000 1.000
#&gt; GSM28722     2  0.0000     0.7664 0.000 1.000
#&gt; GSM11232     2  0.0000     0.7664 0.000 1.000
#&gt; GSM28709     2  0.0000     0.7664 0.000 1.000
#&gt; GSM11226     2  0.0000     0.7664 0.000 1.000
#&gt; GSM11239     2  0.0000     0.7664 0.000 1.000
#&gt; GSM11225     2  0.0000     0.7664 0.000 1.000
#&gt; GSM11220     2  0.0000     0.7664 0.000 1.000
#&gt; GSM28701     2  0.4022     0.7391 0.080 0.920
#&gt; GSM28721     2  0.0000     0.7664 0.000 1.000
#&gt; GSM28713     2  0.7815     0.6596 0.232 0.768
#&gt; GSM28716     1  0.9491     0.0412 0.632 0.368
#&gt; GSM11221     2  0.9922     0.4563 0.448 0.552
#&gt; GSM28717     2  0.9988     0.4146 0.480 0.520
#&gt; GSM11223     1  0.0000     0.9517 1.000 0.000
#&gt; GSM11218     2  0.0000     0.7664 0.000 1.000
#&gt; GSM11219     2  0.9988     0.4146 0.480 0.520
#&gt; GSM11236     2  0.0000     0.7664 0.000 1.000
#&gt; GSM28702     2  0.0000     0.7664 0.000 1.000
#&gt; GSM28705     2  0.0000     0.7664 0.000 1.000
#&gt; GSM11230     2  0.9977     0.4262 0.472 0.528
#&gt; GSM28704     2  0.6247     0.7060 0.156 0.844
#&gt; GSM28700     2  0.9393     0.5554 0.356 0.644
#&gt; GSM11224     2  0.8608     0.6207 0.284 0.716
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-1-a').click(function(){
  $('#tab-SD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-2'>
<p><a id='tab-SD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28710     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM28711     2  0.0237      0.984 0.000 0.996 0.004
#&gt; GSM28712     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM11222     2  0.4002      0.819 0.000 0.840 0.160
#&gt; GSM28720     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11217     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28723     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11241     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28703     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11227     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28706     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11229     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11235     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28707     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11240     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM28714     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM11216     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM28715     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM11234     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM28699     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM11233     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM28718     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM11231     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM11237     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM11228     2  0.0237      0.984 0.000 0.996 0.004
#&gt; GSM28697     2  0.0237      0.984 0.000 0.996 0.004
#&gt; GSM28698     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11238     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11242     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM28719     2  0.0237      0.984 0.000 0.996 0.004
#&gt; GSM28708     2  0.0237      0.984 0.000 0.996 0.004
#&gt; GSM28722     2  0.0237      0.984 0.000 0.996 0.004
#&gt; GSM11232     2  0.0237      0.984 0.000 0.996 0.004
#&gt; GSM28709     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11226     2  0.0237      0.984 0.000 0.996 0.004
#&gt; GSM11239     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11225     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11220     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM28701     2  0.0237      0.984 0.000 0.996 0.004
#&gt; GSM28721     2  0.0237      0.984 0.000 0.996 0.004
#&gt; GSM28713     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM28716     2  0.3816      0.826 0.148 0.852 0.000
#&gt; GSM11221     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM28717     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM11223     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11218     2  0.0237      0.984 0.000 0.996 0.004
#&gt; GSM11219     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM11236     2  0.0237      0.984 0.000 0.996 0.004
#&gt; GSM28702     2  0.4002      0.819 0.000 0.840 0.160
#&gt; GSM28705     2  0.0237      0.984 0.000 0.996 0.004
#&gt; GSM11230     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM28704     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM28700     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM11224     2  0.0000      0.984 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-2-a').click(function(){
  $('#tab-SD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-3'>
<p><a id='tab-SD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28710     2  0.4040      0.815 0.000 0.752 0.000 0.248
#&gt; GSM28711     2  0.4250      0.799 0.000 0.724 0.000 0.276
#&gt; GSM28712     2  0.0469      0.805 0.000 0.988 0.000 0.012
#&gt; GSM11222     4  0.3528      0.758 0.000 0.000 0.192 0.808
#&gt; GSM28720     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.0000      0.799 0.000 1.000 0.000 0.000
#&gt; GSM28714     2  0.0000      0.799 0.000 1.000 0.000 0.000
#&gt; GSM11216     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28715     2  0.3486      0.821 0.000 0.812 0.000 0.188
#&gt; GSM11234     2  0.4250      0.799 0.000 0.724 0.000 0.276
#&gt; GSM28699     2  0.1637      0.818 0.000 0.940 0.000 0.060
#&gt; GSM11233     2  0.0336      0.803 0.000 0.992 0.000 0.008
#&gt; GSM28718     2  0.0000      0.799 0.000 1.000 0.000 0.000
#&gt; GSM11231     2  0.3975      0.817 0.000 0.760 0.000 0.240
#&gt; GSM11237     2  0.0000      0.799 0.000 1.000 0.000 0.000
#&gt; GSM11228     4  0.0000      0.890 0.000 0.000 0.000 1.000
#&gt; GSM28697     4  0.3528      0.756 0.000 0.192 0.000 0.808
#&gt; GSM28698     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11238     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11242     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28719     4  0.3486      0.763 0.000 0.188 0.000 0.812
#&gt; GSM28708     4  0.0469      0.888 0.000 0.012 0.000 0.988
#&gt; GSM28722     2  0.4304      0.790 0.000 0.716 0.000 0.284
#&gt; GSM11232     4  0.3356      0.780 0.000 0.176 0.000 0.824
#&gt; GSM28709     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11226     4  0.0000      0.890 0.000 0.000 0.000 1.000
#&gt; GSM11239     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11225     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11220     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28701     2  0.4933      0.501 0.000 0.568 0.000 0.432
#&gt; GSM28721     4  0.0000      0.890 0.000 0.000 0.000 1.000
#&gt; GSM28713     2  0.4193      0.805 0.000 0.732 0.000 0.268
#&gt; GSM28716     2  0.1970      0.815 0.008 0.932 0.000 0.060
#&gt; GSM11221     2  0.4164      0.807 0.000 0.736 0.000 0.264
#&gt; GSM28717     2  0.1637      0.818 0.000 0.940 0.000 0.060
#&gt; GSM11223     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11218     4  0.0000      0.890 0.000 0.000 0.000 1.000
#&gt; GSM11219     2  0.3172      0.825 0.000 0.840 0.000 0.160
#&gt; GSM11236     4  0.3266      0.788 0.000 0.168 0.000 0.832
#&gt; GSM28702     4  0.0921      0.874 0.000 0.000 0.028 0.972
#&gt; GSM28705     4  0.0000      0.890 0.000 0.000 0.000 1.000
#&gt; GSM11230     2  0.3528      0.821 0.000 0.808 0.000 0.192
#&gt; GSM28704     2  0.4250      0.799 0.000 0.724 0.000 0.276
#&gt; GSM28700     2  0.1637      0.818 0.000 0.940 0.000 0.060
#&gt; GSM11224     2  0.4040      0.815 0.000 0.752 0.000 0.248
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-3-a').click(function(){
  $('#tab-SD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-4'>
<p><a id='tab-SD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28710     2  0.1668      0.776 0.000 0.940 0.000 0.032 0.028
#&gt; GSM28711     2  0.1568      0.773 0.000 0.944 0.000 0.036 0.020
#&gt; GSM28712     5  0.3837      0.738 0.000 0.308 0.000 0.000 0.692
#&gt; GSM11222     3  0.4415      0.387 0.000 0.000 0.552 0.444 0.004
#&gt; GSM28720     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0162      0.998 0.996 0.000 0.000 0.000 0.004
#&gt; GSM28703     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0162      0.998 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11240     5  0.1908      0.812 0.000 0.092 0.000 0.000 0.908
#&gt; GSM28714     5  0.1908      0.812 0.000 0.092 0.000 0.000 0.908
#&gt; GSM11216     3  0.0000      0.947 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28715     2  0.3999      0.374 0.000 0.656 0.000 0.000 0.344
#&gt; GSM11234     2  0.1831      0.754 0.000 0.920 0.000 0.004 0.076
#&gt; GSM28699     5  0.4642      0.717 0.000 0.308 0.000 0.032 0.660
#&gt; GSM11233     5  0.3707      0.746 0.000 0.284 0.000 0.000 0.716
#&gt; GSM28718     5  0.1908      0.812 0.000 0.092 0.000 0.000 0.908
#&gt; GSM11231     2  0.1478      0.772 0.000 0.936 0.000 0.000 0.064
#&gt; GSM11237     5  0.1908      0.812 0.000 0.092 0.000 0.000 0.908
#&gt; GSM11228     4  0.2280      0.763 0.000 0.120 0.000 0.880 0.000
#&gt; GSM28697     4  0.4953      0.373 0.000 0.440 0.000 0.532 0.028
#&gt; GSM28698     3  0.0000      0.947 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11238     3  0.0000      0.947 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11242     3  0.0000      0.947 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28719     4  0.4527      0.639 0.000 0.272 0.000 0.692 0.036
#&gt; GSM28708     4  0.2989      0.756 0.000 0.132 0.008 0.852 0.008
#&gt; GSM28722     2  0.0451      0.783 0.000 0.988 0.000 0.004 0.008
#&gt; GSM11232     4  0.5157      0.357 0.000 0.440 0.000 0.520 0.040
#&gt; GSM28709     3  0.0000      0.947 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11226     4  0.2280      0.763 0.000 0.120 0.000 0.880 0.000
#&gt; GSM11239     3  0.0000      0.947 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11225     3  0.0000      0.947 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11220     3  0.0000      0.947 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28701     2  0.4996     -0.153 0.000 0.548 0.000 0.420 0.032
#&gt; GSM28721     4  0.2280      0.763 0.000 0.120 0.000 0.880 0.000
#&gt; GSM28713     2  0.1952      0.755 0.000 0.912 0.000 0.004 0.084
#&gt; GSM28716     2  0.3681      0.735 0.044 0.848 0.000 0.056 0.052
#&gt; GSM11221     2  0.0771      0.783 0.000 0.976 0.000 0.004 0.020
#&gt; GSM28717     5  0.4679      0.714 0.000 0.316 0.000 0.032 0.652
#&gt; GSM11223     1  0.0162      0.998 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11218     4  0.2280      0.763 0.000 0.120 0.000 0.880 0.000
#&gt; GSM11219     2  0.4306     -0.181 0.000 0.508 0.000 0.000 0.492
#&gt; GSM11236     4  0.5044      0.436 0.000 0.408 0.000 0.556 0.036
#&gt; GSM28702     4  0.4403     -0.271 0.000 0.000 0.436 0.560 0.004
#&gt; GSM28705     4  0.2280      0.763 0.000 0.120 0.000 0.880 0.000
#&gt; GSM11230     2  0.3895      0.415 0.000 0.680 0.000 0.000 0.320
#&gt; GSM28704     2  0.0671      0.782 0.000 0.980 0.000 0.004 0.016
#&gt; GSM28700     2  0.2561      0.670 0.000 0.856 0.000 0.000 0.144
#&gt; GSM11224     2  0.1792      0.761 0.000 0.916 0.000 0.000 0.084
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-4-a').click(function(){
  $('#tab-SD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-5'>
<p><a id='tab-SD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; GSM28710     2  0.2034      0.823 0.000 0.912 0.000 0.060 0.024 NA
#&gt; GSM28711     2  0.3089      0.816 0.000 0.856 0.000 0.080 0.024 NA
#&gt; GSM28712     5  0.3966      0.146 0.000 0.444 0.000 0.004 0.552 NA
#&gt; GSM11222     4  0.6001      0.336 0.000 0.000 0.268 0.436 0.000 NA
#&gt; GSM28720     1  0.0000      0.968 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM11217     1  0.0291      0.968 0.992 0.000 0.000 0.000 0.004 NA
#&gt; GSM28723     1  0.0363      0.966 0.988 0.000 0.000 0.000 0.000 NA
#&gt; GSM11241     1  0.2092      0.915 0.876 0.000 0.000 0.000 0.000 NA
#&gt; GSM28703     1  0.0291      0.968 0.992 0.000 0.000 0.000 0.004 NA
#&gt; GSM11227     1  0.0291      0.968 0.992 0.000 0.000 0.000 0.004 NA
#&gt; GSM28706     1  0.0363      0.966 0.988 0.000 0.000 0.000 0.000 NA
#&gt; GSM11229     1  0.0291      0.968 0.992 0.000 0.000 0.000 0.004 NA
#&gt; GSM11235     1  0.0291      0.968 0.992 0.000 0.000 0.000 0.004 NA
#&gt; GSM28707     1  0.1663      0.935 0.912 0.000 0.000 0.000 0.000 NA
#&gt; GSM11240     5  0.0458      0.736 0.000 0.016 0.000 0.000 0.984 NA
#&gt; GSM28714     5  0.0458      0.736 0.000 0.016 0.000 0.000 0.984 NA
#&gt; GSM11216     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM28715     2  0.2738      0.763 0.000 0.820 0.000 0.000 0.176 NA
#&gt; GSM11234     2  0.1563      0.809 0.000 0.932 0.000 0.000 0.012 NA
#&gt; GSM28699     5  0.6852      0.560 0.000 0.204 0.000 0.060 0.388 NA
#&gt; GSM11233     5  0.5419      0.666 0.000 0.132 0.000 0.004 0.568 NA
#&gt; GSM28718     5  0.0458      0.736 0.000 0.016 0.000 0.000 0.984 NA
#&gt; GSM11231     2  0.2431      0.797 0.000 0.860 0.000 0.000 0.132 NA
#&gt; GSM11237     5  0.0458      0.736 0.000 0.016 0.000 0.000 0.984 NA
#&gt; GSM11228     4  0.0260      0.789 0.000 0.000 0.000 0.992 0.000 NA
#&gt; GSM28697     2  0.4936      0.401 0.000 0.552 0.000 0.396 0.024 NA
#&gt; GSM28698     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM11238     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM11242     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM28719     4  0.4961      0.288 0.000 0.332 0.000 0.604 0.024 NA
#&gt; GSM28708     4  0.1116      0.779 0.000 0.008 0.000 0.960 0.004 NA
#&gt; GSM28722     2  0.1225      0.822 0.000 0.952 0.000 0.012 0.000 NA
#&gt; GSM11232     2  0.4252      0.729 0.000 0.728 0.000 0.216 0.024 NA
#&gt; GSM28709     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM11226     4  0.0000      0.790 0.000 0.000 0.000 1.000 0.000 NA
#&gt; GSM11239     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM11225     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM11220     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM28701     2  0.4153      0.743 0.000 0.752 0.000 0.184 0.024 NA
#&gt; GSM28721     4  0.0000      0.790 0.000 0.000 0.000 1.000 0.000 NA
#&gt; GSM28713     2  0.0993      0.810 0.000 0.964 0.000 0.000 0.012 NA
#&gt; GSM28716     2  0.4897      0.696 0.008 0.692 0.000 0.056 0.024 NA
#&gt; GSM11221     2  0.1668      0.825 0.000 0.928 0.000 0.060 0.008 NA
#&gt; GSM28717     5  0.6488      0.626 0.000 0.140 0.000 0.060 0.476 NA
#&gt; GSM11223     1  0.2178      0.910 0.868 0.000 0.000 0.000 0.000 NA
#&gt; GSM11218     4  0.0000      0.790 0.000 0.000 0.000 1.000 0.000 NA
#&gt; GSM11219     2  0.3468      0.620 0.000 0.712 0.000 0.000 0.284 NA
#&gt; GSM11236     4  0.4984      0.445 0.000 0.280 0.000 0.640 0.024 NA
#&gt; GSM28702     4  0.5074      0.560 0.000 0.000 0.108 0.596 0.000 NA
#&gt; GSM28705     4  0.0260      0.789 0.000 0.000 0.000 0.992 0.000 NA
#&gt; GSM11230     2  0.2971      0.785 0.000 0.832 0.000 0.020 0.144 NA
#&gt; GSM28704     2  0.1124      0.819 0.000 0.956 0.000 0.000 0.008 NA
#&gt; GSM28700     2  0.2006      0.771 0.000 0.892 0.000 0.000 0.104 NA
#&gt; GSM11224     2  0.1074      0.809 0.000 0.960 0.000 0.000 0.012 NA
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-5-a').click(function(){
  $('#tab-SD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-mclust-signature_compare](figure_cola/SD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-mclust-collect-classes](figure_cola/SD-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) k
#> SD:mclust 39     0.425 2
#> SD:mclust 54     0.374 3
#> SD:mclust 54     0.355 4
#> SD:mclust 45     0.419 5
#> SD:mclust 49     0.426 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "NMF"]
# you can also extract it by
# res = res_list["SD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21452 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-NMF-collect-plots](figure_cola/SD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-NMF-select-partition-number](figure_cola/SD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.562           0.872       0.899         0.4062 0.628   0.628
#> 3 3 0.971           0.928       0.975         0.5108 0.718   0.564
#> 4 4 0.896           0.905       0.946         0.2133 0.825   0.569
#> 5 5 0.817           0.790       0.876         0.0575 0.929   0.732
#> 6 6 0.819           0.754       0.859         0.0352 0.959   0.811
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-classes'>
<ul>
<li><a href='#tab-SD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-NMF-get-classes-1'>
<p><a id='tab-SD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28710     2  0.5059      0.881 0.112 0.888
#&gt; GSM28711     2  0.4298      0.894 0.088 0.912
#&gt; GSM28712     2  0.4562      0.890 0.096 0.904
#&gt; GSM11222     2  0.6801      0.789 0.180 0.820
#&gt; GSM28720     1  0.0672      0.958 0.992 0.008
#&gt; GSM11217     1  0.0672      0.958 0.992 0.008
#&gt; GSM28723     1  0.0672      0.958 0.992 0.008
#&gt; GSM11241     1  0.0672      0.958 0.992 0.008
#&gt; GSM28703     1  0.0672      0.958 0.992 0.008
#&gt; GSM11227     1  0.0672      0.958 0.992 0.008
#&gt; GSM28706     1  0.0672      0.958 0.992 0.008
#&gt; GSM11229     1  0.0672      0.958 0.992 0.008
#&gt; GSM11235     1  0.0672      0.958 0.992 0.008
#&gt; GSM28707     1  0.0672      0.958 0.992 0.008
#&gt; GSM11240     2  0.3879      0.897 0.076 0.924
#&gt; GSM28714     2  0.3879      0.897 0.076 0.924
#&gt; GSM11216     2  0.7674      0.744 0.224 0.776
#&gt; GSM28715     2  0.3431      0.898 0.064 0.936
#&gt; GSM11234     2  0.4298      0.894 0.088 0.912
#&gt; GSM28699     1  0.8499      0.634 0.724 0.276
#&gt; GSM11233     2  0.4690      0.888 0.100 0.900
#&gt; GSM28718     2  0.3879      0.897 0.076 0.924
#&gt; GSM11231     2  0.3274      0.898 0.060 0.940
#&gt; GSM11237     2  0.3879      0.897 0.076 0.924
#&gt; GSM11228     2  0.1414      0.890 0.020 0.980
#&gt; GSM28697     2  0.3274      0.898 0.060 0.940
#&gt; GSM28698     2  0.6887      0.786 0.184 0.816
#&gt; GSM11238     2  0.6801      0.789 0.180 0.820
#&gt; GSM11242     2  0.6801      0.789 0.180 0.820
#&gt; GSM28719     2  0.1184      0.889 0.016 0.984
#&gt; GSM28708     2  0.3879      0.857 0.076 0.924
#&gt; GSM28722     2  0.3733      0.898 0.072 0.928
#&gt; GSM11232     2  0.2043      0.894 0.032 0.968
#&gt; GSM28709     2  0.6801      0.789 0.180 0.820
#&gt; GSM11226     2  0.0376      0.880 0.004 0.996
#&gt; GSM11239     2  0.6801      0.789 0.180 0.820
#&gt; GSM11225     2  0.6801      0.789 0.180 0.820
#&gt; GSM11220     2  0.7883      0.729 0.236 0.764
#&gt; GSM28701     2  0.5408      0.872 0.124 0.876
#&gt; GSM28721     2  0.0672      0.878 0.008 0.992
#&gt; GSM28713     2  0.4298      0.894 0.088 0.912
#&gt; GSM28716     1  0.6712      0.775 0.824 0.176
#&gt; GSM11221     2  0.4690      0.888 0.100 0.900
#&gt; GSM28717     2  0.8267      0.701 0.260 0.740
#&gt; GSM11223     1  0.0938      0.954 0.988 0.012
#&gt; GSM11218     2  0.0938      0.878 0.012 0.988
#&gt; GSM11219     2  0.3879      0.897 0.076 0.924
#&gt; GSM11236     2  0.5629      0.840 0.132 0.868
#&gt; GSM28702     2  0.6801      0.789 0.180 0.820
#&gt; GSM28705     2  0.2423      0.895 0.040 0.960
#&gt; GSM11230     2  0.2043      0.894 0.032 0.968
#&gt; GSM28704     2  0.3733      0.898 0.072 0.928
#&gt; GSM28700     2  0.4690      0.888 0.100 0.900
#&gt; GSM11224     2  0.4298      0.894 0.088 0.912
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-1-a').click(function(){
  $('#tab-SD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-2'>
<p><a id='tab-SD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28710     2  0.0000     0.9774 0.000 1.000 0.000
#&gt; GSM28711     2  0.0000     0.9774 0.000 1.000 0.000
#&gt; GSM28712     2  0.0000     0.9774 0.000 1.000 0.000
#&gt; GSM11222     3  0.0000     0.9217 0.000 0.000 1.000
#&gt; GSM28720     1  0.0000     0.9986 1.000 0.000 0.000
#&gt; GSM11217     1  0.0000     0.9986 1.000 0.000 0.000
#&gt; GSM28723     1  0.0000     0.9986 1.000 0.000 0.000
#&gt; GSM11241     1  0.0000     0.9986 1.000 0.000 0.000
#&gt; GSM28703     1  0.0000     0.9986 1.000 0.000 0.000
#&gt; GSM11227     1  0.0000     0.9986 1.000 0.000 0.000
#&gt; GSM28706     1  0.0000     0.9986 1.000 0.000 0.000
#&gt; GSM11229     1  0.0000     0.9986 1.000 0.000 0.000
#&gt; GSM11235     1  0.0000     0.9986 1.000 0.000 0.000
#&gt; GSM28707     1  0.0000     0.9986 1.000 0.000 0.000
#&gt; GSM11240     2  0.0000     0.9774 0.000 1.000 0.000
#&gt; GSM28714     2  0.0000     0.9774 0.000 1.000 0.000
#&gt; GSM11216     3  0.0000     0.9217 0.000 0.000 1.000
#&gt; GSM28715     2  0.0000     0.9774 0.000 1.000 0.000
#&gt; GSM11234     2  0.0000     0.9774 0.000 1.000 0.000
#&gt; GSM28699     2  0.1860     0.9252 0.052 0.948 0.000
#&gt; GSM11233     2  0.0000     0.9774 0.000 1.000 0.000
#&gt; GSM28718     2  0.0000     0.9774 0.000 1.000 0.000
#&gt; GSM11231     2  0.0000     0.9774 0.000 1.000 0.000
#&gt; GSM11237     2  0.0000     0.9774 0.000 1.000 0.000
#&gt; GSM11228     2  0.0000     0.9774 0.000 1.000 0.000
#&gt; GSM28697     2  0.0000     0.9774 0.000 1.000 0.000
#&gt; GSM28698     3  0.0000     0.9217 0.000 0.000 1.000
#&gt; GSM11238     3  0.0000     0.9217 0.000 0.000 1.000
#&gt; GSM11242     3  0.0000     0.9217 0.000 0.000 1.000
#&gt; GSM28719     2  0.0000     0.9774 0.000 1.000 0.000
#&gt; GSM28708     3  0.3686     0.7954 0.000 0.140 0.860
#&gt; GSM28722     2  0.0000     0.9774 0.000 1.000 0.000
#&gt; GSM11232     2  0.0000     0.9774 0.000 1.000 0.000
#&gt; GSM28709     3  0.0000     0.9217 0.000 0.000 1.000
#&gt; GSM11226     2  0.1411     0.9415 0.000 0.964 0.036
#&gt; GSM11239     3  0.0000     0.9217 0.000 0.000 1.000
#&gt; GSM11225     3  0.0000     0.9217 0.000 0.000 1.000
#&gt; GSM11220     3  0.0000     0.9217 0.000 0.000 1.000
#&gt; GSM28701     2  0.0000     0.9774 0.000 1.000 0.000
#&gt; GSM28721     2  0.6309    -0.0965 0.000 0.500 0.500
#&gt; GSM28713     2  0.0000     0.9774 0.000 1.000 0.000
#&gt; GSM28716     1  0.0592     0.9840 0.988 0.012 0.000
#&gt; GSM11221     2  0.0000     0.9774 0.000 1.000 0.000
#&gt; GSM28717     2  0.0000     0.9774 0.000 1.000 0.000
#&gt; GSM11223     1  0.0000     0.9986 1.000 0.000 0.000
#&gt; GSM11218     3  0.6299     0.1045 0.000 0.476 0.524
#&gt; GSM11219     2  0.0000     0.9774 0.000 1.000 0.000
#&gt; GSM11236     3  0.3192     0.8265 0.000 0.112 0.888
#&gt; GSM28702     3  0.0000     0.9217 0.000 0.000 1.000
#&gt; GSM28705     2  0.0000     0.9774 0.000 1.000 0.000
#&gt; GSM11230     2  0.0000     0.9774 0.000 1.000 0.000
#&gt; GSM28704     2  0.0000     0.9774 0.000 1.000 0.000
#&gt; GSM28700     2  0.0000     0.9774 0.000 1.000 0.000
#&gt; GSM11224     2  0.0000     0.9774 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-2-a').click(function(){
  $('#tab-SD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-3'>
<p><a id='tab-SD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28710     2  0.0336     0.9321 0.000 0.992 0.000 0.008
#&gt; GSM28711     2  0.2281     0.8886 0.000 0.904 0.000 0.096
#&gt; GSM28712     2  0.0336     0.9321 0.000 0.992 0.000 0.008
#&gt; GSM11222     3  0.0469     0.9921 0.000 0.000 0.988 0.012
#&gt; GSM28720     1  0.0000     0.9994 1.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000     0.9994 1.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000     0.9994 1.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000     0.9994 1.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000     0.9994 1.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000     0.9994 1.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000     0.9994 1.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000     0.9994 1.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000     0.9994 1.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000     0.9994 1.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.0188     0.9317 0.000 0.996 0.000 0.004
#&gt; GSM28714     2  0.0000     0.9307 0.000 1.000 0.000 0.000
#&gt; GSM11216     3  0.0000     0.9944 0.000 0.000 1.000 0.000
#&gt; GSM28715     2  0.1022     0.9286 0.000 0.968 0.000 0.032
#&gt; GSM11234     4  0.1557     0.8903 0.000 0.056 0.000 0.944
#&gt; GSM28699     2  0.1022     0.9125 0.000 0.968 0.000 0.032
#&gt; GSM11233     2  0.1022     0.9131 0.000 0.968 0.000 0.032
#&gt; GSM28718     2  0.0000     0.9307 0.000 1.000 0.000 0.000
#&gt; GSM11231     2  0.2408     0.8781 0.000 0.896 0.000 0.104
#&gt; GSM11237     2  0.0336     0.9309 0.000 0.992 0.000 0.008
#&gt; GSM11228     4  0.1211     0.8915 0.000 0.040 0.000 0.960
#&gt; GSM28697     4  0.3088     0.8466 0.000 0.128 0.008 0.864
#&gt; GSM28698     3  0.0000     0.9944 0.000 0.000 1.000 0.000
#&gt; GSM11238     3  0.0188     0.9944 0.000 0.000 0.996 0.004
#&gt; GSM11242     3  0.0336     0.9943 0.000 0.000 0.992 0.008
#&gt; GSM28719     4  0.3975     0.7250 0.000 0.240 0.000 0.760
#&gt; GSM28708     4  0.3606     0.7978 0.000 0.024 0.132 0.844
#&gt; GSM28722     4  0.1716     0.8877 0.000 0.064 0.000 0.936
#&gt; GSM11232     4  0.1389     0.8915 0.000 0.048 0.000 0.952
#&gt; GSM28709     3  0.0000     0.9944 0.000 0.000 1.000 0.000
#&gt; GSM11226     4  0.1211     0.8915 0.000 0.040 0.000 0.960
#&gt; GSM11239     3  0.0336     0.9943 0.000 0.000 0.992 0.008
#&gt; GSM11225     3  0.0336     0.9943 0.000 0.000 0.992 0.008
#&gt; GSM11220     3  0.0188     0.9929 0.000 0.000 0.996 0.004
#&gt; GSM28701     2  0.4679     0.4357 0.000 0.648 0.000 0.352
#&gt; GSM28721     4  0.1356     0.8876 0.000 0.032 0.008 0.960
#&gt; GSM28713     2  0.4193     0.6599 0.000 0.732 0.000 0.268
#&gt; GSM28716     1  0.0336     0.9936 0.992 0.000 0.000 0.008
#&gt; GSM11221     2  0.1211     0.9253 0.000 0.960 0.000 0.040
#&gt; GSM28717     2  0.1022     0.9125 0.000 0.968 0.000 0.032
#&gt; GSM11223     1  0.0000     0.9994 1.000 0.000 0.000 0.000
#&gt; GSM11218     4  0.1209     0.8877 0.000 0.032 0.004 0.964
#&gt; GSM11219     2  0.0707     0.9317 0.000 0.980 0.000 0.020
#&gt; GSM11236     3  0.0469     0.9839 0.000 0.012 0.988 0.000
#&gt; GSM28702     4  0.4992     0.0955 0.000 0.000 0.476 0.524
#&gt; GSM28705     4  0.1211     0.8915 0.000 0.040 0.000 0.960
#&gt; GSM11230     2  0.0817     0.9309 0.000 0.976 0.000 0.024
#&gt; GSM28704     4  0.4250     0.6373 0.000 0.276 0.000 0.724
#&gt; GSM28700     2  0.0707     0.9319 0.000 0.980 0.000 0.020
#&gt; GSM11224     2  0.2281     0.8891 0.000 0.904 0.000 0.096
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-3-a').click(function(){
  $('#tab-SD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-4'>
<p><a id='tab-SD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28710     2  0.1041   0.899644 0.000 0.964 0.000 0.004 0.032
#&gt; GSM28711     2  0.3163   0.810363 0.000 0.824 0.000 0.164 0.012
#&gt; GSM28712     2  0.0451   0.908641 0.000 0.988 0.000 0.008 0.004
#&gt; GSM11222     3  0.0794   0.810247 0.000 0.000 0.972 0.000 0.028
#&gt; GSM28720     1  0.0000   0.999138 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000   0.999138 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000   0.999138 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000   0.999138 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000   0.999138 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000   0.999138 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000   0.999138 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000   0.999138 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000   0.999138 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000   0.999138 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.0798   0.910389 0.000 0.976 0.000 0.016 0.008
#&gt; GSM28714     2  0.0807   0.910248 0.000 0.976 0.000 0.012 0.012
#&gt; GSM11216     3  0.3913   0.646480 0.000 0.000 0.676 0.000 0.324
#&gt; GSM28715     2  0.2046   0.892820 0.000 0.916 0.000 0.068 0.016
#&gt; GSM11234     4  0.2793   0.761249 0.000 0.036 0.000 0.876 0.088
#&gt; GSM28699     2  0.1571   0.880123 0.000 0.936 0.000 0.004 0.060
#&gt; GSM11233     2  0.1502   0.882503 0.000 0.940 0.000 0.004 0.056
#&gt; GSM28718     2  0.0807   0.910346 0.000 0.976 0.000 0.012 0.012
#&gt; GSM11231     5  0.5382   0.540515 0.000 0.260 0.000 0.100 0.640
#&gt; GSM11237     2  0.0794   0.905705 0.000 0.972 0.000 0.000 0.028
#&gt; GSM11228     4  0.4142   0.353964 0.000 0.004 0.004 0.684 0.308
#&gt; GSM28697     5  0.5723   0.345581 0.000 0.088 0.000 0.392 0.520
#&gt; GSM28698     3  0.1197   0.799780 0.000 0.000 0.952 0.000 0.048
#&gt; GSM11238     3  0.0000   0.809565 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11242     3  0.1124   0.808653 0.000 0.000 0.960 0.004 0.036
#&gt; GSM28719     5  0.4323   0.611985 0.000 0.024 0.020 0.196 0.760
#&gt; GSM28708     5  0.5040   0.540731 0.000 0.000 0.068 0.272 0.660
#&gt; GSM28722     4  0.0963   0.817539 0.000 0.036 0.000 0.964 0.000
#&gt; GSM11232     4  0.2843   0.697460 0.000 0.008 0.000 0.848 0.144
#&gt; GSM28709     3  0.3966   0.619820 0.000 0.000 0.664 0.000 0.336
#&gt; GSM11226     4  0.0798   0.826137 0.000 0.016 0.008 0.976 0.000
#&gt; GSM11239     3  0.1041   0.809728 0.000 0.000 0.964 0.004 0.032
#&gt; GSM11225     3  0.1205   0.807686 0.000 0.000 0.956 0.004 0.040
#&gt; GSM11220     3  0.3816   0.660640 0.000 0.000 0.696 0.000 0.304
#&gt; GSM28701     5  0.4666   0.593436 0.000 0.180 0.000 0.088 0.732
#&gt; GSM28721     4  0.0290   0.827387 0.000 0.008 0.000 0.992 0.000
#&gt; GSM28713     2  0.4574   0.358395 0.000 0.576 0.000 0.412 0.012
#&gt; GSM28716     1  0.0290   0.990470 0.992 0.008 0.000 0.000 0.000
#&gt; GSM11221     2  0.2020   0.878224 0.000 0.900 0.000 0.100 0.000
#&gt; GSM28717     2  0.1638   0.877745 0.000 0.932 0.000 0.004 0.064
#&gt; GSM11223     1  0.0000   0.999138 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     4  0.0833   0.822105 0.000 0.004 0.016 0.976 0.004
#&gt; GSM11219     2  0.1041   0.909237 0.000 0.964 0.000 0.032 0.004
#&gt; GSM11236     5  0.4686  -0.000263 0.000 0.012 0.396 0.004 0.588
#&gt; GSM28702     3  0.4331   0.313753 0.000 0.000 0.596 0.400 0.004
#&gt; GSM28705     4  0.0451   0.827000 0.000 0.008 0.000 0.988 0.004
#&gt; GSM11230     2  0.1704   0.896875 0.000 0.928 0.000 0.068 0.004
#&gt; GSM28704     4  0.4292   0.446601 0.000 0.272 0.000 0.704 0.024
#&gt; GSM28700     2  0.0992   0.910426 0.000 0.968 0.000 0.024 0.008
#&gt; GSM11224     2  0.2890   0.824999 0.000 0.836 0.000 0.160 0.004
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-4-a').click(function(){
  $('#tab-SD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-5'>
<p><a id='tab-SD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28710     2  0.3316      0.793 0.000 0.812 0.000 0.052 0.136 0.000
#&gt; GSM28711     2  0.2773      0.789 0.000 0.836 0.000 0.008 0.004 0.152
#&gt; GSM28712     2  0.0713      0.856 0.000 0.972 0.000 0.000 0.028 0.000
#&gt; GSM11222     3  0.0993      0.791 0.000 0.000 0.964 0.012 0.024 0.000
#&gt; GSM28720     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.0858      0.860 0.000 0.968 0.000 0.004 0.000 0.028
#&gt; GSM28714     2  0.1149      0.859 0.000 0.960 0.000 0.008 0.008 0.024
#&gt; GSM11216     5  0.4234      0.825 0.000 0.000 0.280 0.044 0.676 0.000
#&gt; GSM28715     2  0.1882      0.848 0.000 0.920 0.000 0.012 0.008 0.060
#&gt; GSM11234     6  0.4361      0.580 0.000 0.040 0.000 0.020 0.224 0.716
#&gt; GSM28699     2  0.4411      0.739 0.000 0.736 0.000 0.076 0.172 0.016
#&gt; GSM11233     2  0.3979      0.765 0.000 0.772 0.000 0.052 0.160 0.016
#&gt; GSM28718     2  0.0820      0.859 0.000 0.972 0.000 0.012 0.000 0.016
#&gt; GSM11231     4  0.2383      0.664 0.000 0.096 0.000 0.880 0.000 0.024
#&gt; GSM11237     2  0.0603      0.857 0.000 0.980 0.000 0.016 0.004 0.000
#&gt; GSM11228     4  0.4129      0.340 0.000 0.000 0.000 0.564 0.012 0.424
#&gt; GSM28697     4  0.5990      0.473 0.000 0.020 0.004 0.532 0.304 0.140
#&gt; GSM28698     3  0.2854      0.534 0.000 0.000 0.792 0.000 0.208 0.000
#&gt; GSM11238     3  0.1643      0.761 0.000 0.000 0.924 0.008 0.068 0.000
#&gt; GSM11242     3  0.0146      0.796 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; GSM28719     4  0.1586      0.689 0.000 0.012 0.004 0.940 0.004 0.040
#&gt; GSM28708     4  0.2322      0.684 0.000 0.000 0.036 0.896 0.004 0.064
#&gt; GSM28722     6  0.1471      0.751 0.000 0.064 0.000 0.004 0.000 0.932
#&gt; GSM11232     6  0.3690      0.371 0.000 0.008 0.000 0.308 0.000 0.684
#&gt; GSM28709     5  0.4866      0.736 0.000 0.000 0.364 0.068 0.568 0.000
#&gt; GSM11226     6  0.1511      0.755 0.000 0.044 0.012 0.004 0.000 0.940
#&gt; GSM11239     3  0.0458      0.795 0.000 0.000 0.984 0.000 0.016 0.000
#&gt; GSM11225     3  0.1082      0.776 0.000 0.000 0.956 0.004 0.040 0.000
#&gt; GSM11220     5  0.3640      0.765 0.000 0.000 0.204 0.028 0.764 0.004
#&gt; GSM28701     4  0.3785      0.645 0.000 0.040 0.000 0.804 0.120 0.036
#&gt; GSM28721     6  0.1251      0.747 0.000 0.024 0.000 0.012 0.008 0.956
#&gt; GSM28713     6  0.4135      0.304 0.000 0.404 0.000 0.004 0.008 0.584
#&gt; GSM28716     1  0.0260      0.992 0.992 0.000 0.000 0.000 0.008 0.000
#&gt; GSM11221     2  0.3272      0.799 0.000 0.820 0.000 0.016 0.020 0.144
#&gt; GSM28717     2  0.5052      0.628 0.000 0.632 0.000 0.084 0.272 0.012
#&gt; GSM11223     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     6  0.2039      0.731 0.000 0.020 0.052 0.012 0.000 0.916
#&gt; GSM11219     2  0.1049      0.859 0.000 0.960 0.000 0.008 0.000 0.032
#&gt; GSM11236     4  0.5859     -0.194 0.000 0.012 0.136 0.452 0.400 0.000
#&gt; GSM28702     3  0.4621      0.290 0.000 0.000 0.604 0.016 0.024 0.356
#&gt; GSM28705     6  0.0891      0.751 0.000 0.024 0.000 0.008 0.000 0.968
#&gt; GSM11230     2  0.2431      0.814 0.000 0.872 0.004 0.004 0.004 0.116
#&gt; GSM28704     6  0.3607      0.485 0.000 0.348 0.000 0.000 0.000 0.652
#&gt; GSM28700     2  0.3256      0.826 0.000 0.836 0.000 0.020 0.112 0.032
#&gt; GSM11224     2  0.2838      0.749 0.000 0.808 0.000 0.000 0.004 0.188
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-5-a').click(function(){
  $('#tab-SD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-NMF-signature_compare](figure_cola/SD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-SD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-NMF-collect-classes](figure_cola/SD-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n tissue(p) k
#> SD:NMF 54     0.398 2
#> SD:NMF 52     0.372 3
#> SD:NMF 52     0.352 4
#> SD:NMF 48     0.476 5
#> SD:NMF 47     0.438 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:hclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "hclust"]
# you can also extract it by
# res = res_list["CV:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21452 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-hclust-collect-plots](figure_cola/CV-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-hclust-select-partition-number](figure_cola/CV-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.627           0.790       0.905         0.3568 0.693   0.693
#> 3 3 0.427           0.457       0.733         0.5841 0.622   0.480
#> 4 4 0.695           0.792       0.883         0.2724 0.729   0.421
#> 5 5 0.712           0.723       0.826         0.0683 0.958   0.851
#> 6 6 0.727           0.653       0.819         0.0398 0.946   0.779
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-classes'>
<ul>
<li><a href='#tab-CV-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-hclust-get-classes-1'>
<p><a id='tab-CV-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28710     2   0.295      0.877 0.052 0.948
#&gt; GSM28711     2   0.295      0.877 0.052 0.948
#&gt; GSM28712     2   0.184      0.886 0.028 0.972
#&gt; GSM11222     1   0.936      0.356 0.648 0.352
#&gt; GSM28720     2   0.000      0.885 0.000 1.000
#&gt; GSM11217     2   0.000      0.885 0.000 1.000
#&gt; GSM28723     2   0.000      0.885 0.000 1.000
#&gt; GSM11241     2   0.000      0.885 0.000 1.000
#&gt; GSM28703     2   0.000      0.885 0.000 1.000
#&gt; GSM11227     2   0.000      0.885 0.000 1.000
#&gt; GSM28706     2   0.000      0.885 0.000 1.000
#&gt; GSM11229     2   0.000      0.885 0.000 1.000
#&gt; GSM11235     2   0.000      0.885 0.000 1.000
#&gt; GSM28707     2   0.000      0.885 0.000 1.000
#&gt; GSM11240     2   0.184      0.886 0.028 0.972
#&gt; GSM28714     2   0.184      0.886 0.028 0.972
#&gt; GSM11216     1   0.000      0.902 1.000 0.000
#&gt; GSM28715     2   0.184      0.886 0.028 0.972
#&gt; GSM11234     2   0.000      0.885 0.000 1.000
#&gt; GSM28699     2   0.000      0.885 0.000 1.000
#&gt; GSM11233     2   0.000      0.885 0.000 1.000
#&gt; GSM28718     2   0.184      0.886 0.028 0.972
#&gt; GSM11231     2   0.671      0.776 0.176 0.824
#&gt; GSM11237     2   0.184      0.886 0.028 0.972
#&gt; GSM11228     2   0.871      0.642 0.292 0.708
#&gt; GSM28697     2   0.871      0.642 0.292 0.708
#&gt; GSM28698     1   0.000      0.902 1.000 0.000
#&gt; GSM11238     1   0.000      0.902 1.000 0.000
#&gt; GSM11242     1   0.000      0.902 1.000 0.000
#&gt; GSM28719     2   0.955      0.490 0.376 0.624
#&gt; GSM28708     2   0.955      0.490 0.376 0.624
#&gt; GSM28722     2   0.327      0.873 0.060 0.940
#&gt; GSM11232     2   0.745      0.742 0.212 0.788
#&gt; GSM28709     1   0.000      0.902 1.000 0.000
#&gt; GSM11226     2   1.000      0.163 0.492 0.508
#&gt; GSM11239     1   0.000      0.902 1.000 0.000
#&gt; GSM11225     1   0.000      0.902 1.000 0.000
#&gt; GSM11220     1   0.000      0.902 1.000 0.000
#&gt; GSM28701     2   0.552      0.824 0.128 0.872
#&gt; GSM28721     2   0.993      0.296 0.452 0.548
#&gt; GSM28713     2   0.224      0.884 0.036 0.964
#&gt; GSM28716     2   0.000      0.885 0.000 1.000
#&gt; GSM11221     2   0.224      0.884 0.036 0.964
#&gt; GSM28717     2   0.000      0.885 0.000 1.000
#&gt; GSM11223     2   0.000      0.885 0.000 1.000
#&gt; GSM11218     2   1.000      0.163 0.492 0.508
#&gt; GSM11219     2   0.204      0.885 0.032 0.968
#&gt; GSM11236     2   0.909      0.588 0.324 0.676
#&gt; GSM28702     1   0.936      0.356 0.648 0.352
#&gt; GSM28705     2   0.886      0.624 0.304 0.696
#&gt; GSM11230     2   0.184      0.886 0.028 0.972
#&gt; GSM28704     2   0.224      0.884 0.036 0.964
#&gt; GSM28700     2   0.000      0.885 0.000 1.000
#&gt; GSM11224     2   0.184      0.886 0.028 0.972
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-1-a').click(function(){
  $('#tab-CV-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-2'>
<p><a id='tab-CV-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28710     2   0.614     0.3536 0.404 0.596 0.000
#&gt; GSM28711     2   0.614     0.3536 0.404 0.596 0.000
#&gt; GSM28712     1   0.630    -0.1638 0.528 0.472 0.000
#&gt; GSM11222     2   0.626    -0.2167 0.000 0.552 0.448
#&gt; GSM28720     1   0.000     0.7147 1.000 0.000 0.000
#&gt; GSM11217     1   0.000     0.7147 1.000 0.000 0.000
#&gt; GSM28723     1   0.000     0.7147 1.000 0.000 0.000
#&gt; GSM11241     1   0.000     0.7147 1.000 0.000 0.000
#&gt; GSM28703     1   0.000     0.7147 1.000 0.000 0.000
#&gt; GSM11227     1   0.000     0.7147 1.000 0.000 0.000
#&gt; GSM28706     1   0.000     0.7147 1.000 0.000 0.000
#&gt; GSM11229     1   0.000     0.7147 1.000 0.000 0.000
#&gt; GSM11235     1   0.000     0.7147 1.000 0.000 0.000
#&gt; GSM28707     1   0.000     0.7147 1.000 0.000 0.000
#&gt; GSM11240     2   0.631     0.2136 0.488 0.512 0.000
#&gt; GSM28714     2   0.631     0.2136 0.488 0.512 0.000
#&gt; GSM11216     3   0.000     1.0000 0.000 0.000 1.000
#&gt; GSM28715     2   0.631     0.2136 0.488 0.512 0.000
#&gt; GSM11234     1   0.625    -0.0613 0.556 0.444 0.000
#&gt; GSM28699     1   0.536     0.4489 0.724 0.276 0.000
#&gt; GSM11233     1   0.536     0.4489 0.724 0.276 0.000
#&gt; GSM28718     2   0.631     0.2136 0.488 0.512 0.000
#&gt; GSM11231     2   0.757     0.3564 0.376 0.576 0.048
#&gt; GSM11237     2   0.631     0.2136 0.488 0.512 0.000
#&gt; GSM11228     2   0.474     0.4814 0.104 0.848 0.048
#&gt; GSM28697     2   0.474     0.4814 0.104 0.848 0.048
#&gt; GSM28698     3   0.000     1.0000 0.000 0.000 1.000
#&gt; GSM11238     3   0.000     1.0000 0.000 0.000 1.000
#&gt; GSM11242     3   0.000     1.0000 0.000 0.000 1.000
#&gt; GSM28719     2   0.175     0.4592 0.000 0.952 0.048
#&gt; GSM28708     2   0.175     0.4592 0.000 0.952 0.048
#&gt; GSM28722     2   0.597     0.3917 0.364 0.636 0.000
#&gt; GSM11232     2   0.683     0.4528 0.260 0.692 0.048
#&gt; GSM28709     3   0.000     1.0000 0.000 0.000 1.000
#&gt; GSM11226     2   0.550     0.2343 0.000 0.708 0.292
#&gt; GSM11239     3   0.000     1.0000 0.000 0.000 1.000
#&gt; GSM11225     3   0.000     1.0000 0.000 0.000 1.000
#&gt; GSM11220     3   0.000     1.0000 0.000 0.000 1.000
#&gt; GSM28701     2   0.628     0.4325 0.304 0.680 0.016
#&gt; GSM28721     2   0.558     0.3093 0.008 0.736 0.256
#&gt; GSM28713     2   0.630     0.2472 0.476 0.524 0.000
#&gt; GSM28716     1   0.579     0.2828 0.668 0.332 0.000
#&gt; GSM11221     2   0.631     0.2121 0.492 0.508 0.000
#&gt; GSM28717     1   0.536     0.4489 0.724 0.276 0.000
#&gt; GSM11223     1   0.000     0.7147 1.000 0.000 0.000
#&gt; GSM11218     2   0.550     0.2343 0.000 0.708 0.292
#&gt; GSM11219     2   0.631     0.2117 0.492 0.508 0.000
#&gt; GSM11236     2   0.653     0.4672 0.152 0.756 0.092
#&gt; GSM28702     2   0.626    -0.2167 0.000 0.552 0.448
#&gt; GSM28705     2   0.839     0.4551 0.176 0.624 0.200
#&gt; GSM11230     2   0.631     0.2136 0.488 0.512 0.000
#&gt; GSM28704     2   0.630     0.2549 0.472 0.528 0.000
#&gt; GSM28700     1   0.625    -0.0671 0.556 0.444 0.000
#&gt; GSM11224     1   0.630    -0.2075 0.516 0.484 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-2-a').click(function(){
  $('#tab-CV-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-3'>
<p><a id='tab-CV-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28710     2  0.3024     0.7758 0.000 0.852 0.000 0.148
#&gt; GSM28711     2  0.3024     0.7758 0.000 0.852 0.000 0.148
#&gt; GSM28712     2  0.0779     0.8568 0.016 0.980 0.000 0.004
#&gt; GSM11222     4  0.4955     0.3038 0.000 0.000 0.444 0.556
#&gt; GSM28720     1  0.0000     1.0000 1.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000     1.0000 1.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000     1.0000 1.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000     1.0000 1.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000     1.0000 1.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000     1.0000 1.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000     1.0000 1.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000     1.0000 1.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000     1.0000 1.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000     1.0000 1.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.1557     0.8583 0.000 0.944 0.000 0.056
#&gt; GSM28714     2  0.1557     0.8583 0.000 0.944 0.000 0.056
#&gt; GSM11216     3  0.0000     1.0000 0.000 0.000 1.000 0.000
#&gt; GSM28715     2  0.1557     0.8583 0.000 0.944 0.000 0.056
#&gt; GSM11234     2  0.2227     0.8438 0.036 0.928 0.000 0.036
#&gt; GSM28699     2  0.4464     0.7041 0.208 0.768 0.000 0.024
#&gt; GSM11233     2  0.4464     0.7041 0.208 0.768 0.000 0.024
#&gt; GSM28718     2  0.1557     0.8583 0.000 0.944 0.000 0.056
#&gt; GSM11231     2  0.4543     0.5635 0.000 0.676 0.000 0.324
#&gt; GSM11237     2  0.1557     0.8583 0.000 0.944 0.000 0.056
#&gt; GSM11228     4  0.3801     0.6101 0.000 0.220 0.000 0.780
#&gt; GSM28697     4  0.3801     0.6101 0.000 0.220 0.000 0.780
#&gt; GSM28698     3  0.0000     1.0000 0.000 0.000 1.000 0.000
#&gt; GSM11238     3  0.0000     1.0000 0.000 0.000 1.000 0.000
#&gt; GSM11242     3  0.0000     1.0000 0.000 0.000 1.000 0.000
#&gt; GSM28719     4  0.0817     0.6350 0.000 0.024 0.000 0.976
#&gt; GSM28708     4  0.0817     0.6350 0.000 0.024 0.000 0.976
#&gt; GSM28722     2  0.3569     0.7077 0.000 0.804 0.000 0.196
#&gt; GSM11232     4  0.4981     0.0753 0.000 0.464 0.000 0.536
#&gt; GSM28709     3  0.0000     1.0000 0.000 0.000 1.000 0.000
#&gt; GSM11226     4  0.4304     0.5431 0.000 0.000 0.284 0.716
#&gt; GSM11239     3  0.0000     1.0000 0.000 0.000 1.000 0.000
#&gt; GSM11225     3  0.0000     1.0000 0.000 0.000 1.000 0.000
#&gt; GSM11220     3  0.0000     1.0000 0.000 0.000 1.000 0.000
#&gt; GSM28701     2  0.4222     0.5692 0.000 0.728 0.000 0.272
#&gt; GSM28721     4  0.5249     0.5867 0.000 0.044 0.248 0.708
#&gt; GSM28713     2  0.1022     0.8514 0.000 0.968 0.000 0.032
#&gt; GSM28716     2  0.4426     0.6974 0.204 0.772 0.000 0.024
#&gt; GSM11221     2  0.0592     0.8558 0.000 0.984 0.000 0.016
#&gt; GSM28717     2  0.4464     0.7041 0.208 0.768 0.000 0.024
#&gt; GSM11223     1  0.0000     1.0000 1.000 0.000 0.000 0.000
#&gt; GSM11218     4  0.4304     0.5431 0.000 0.000 0.284 0.716
#&gt; GSM11219     2  0.1302     0.8601 0.000 0.956 0.000 0.044
#&gt; GSM11236     4  0.6094     0.2240 0.000 0.416 0.048 0.536
#&gt; GSM28702     4  0.4955     0.3038 0.000 0.000 0.444 0.556
#&gt; GSM28705     4  0.7373     0.5507 0.000 0.300 0.192 0.508
#&gt; GSM11230     2  0.1557     0.8583 0.000 0.944 0.000 0.056
#&gt; GSM28704     2  0.1389     0.8463 0.000 0.952 0.000 0.048
#&gt; GSM28700     2  0.1489     0.8484 0.044 0.952 0.000 0.004
#&gt; GSM11224     2  0.0376     0.8566 0.004 0.992 0.000 0.004
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-3-a').click(function(){
  $('#tab-CV-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-4'>
<p><a id='tab-CV-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28710     2  0.3962    0.68696 0.000 0.800 0.000 0.112 0.088
#&gt; GSM28711     2  0.3849    0.69069 0.000 0.808 0.000 0.112 0.080
#&gt; GSM28712     2  0.3143    0.53801 0.000 0.796 0.000 0.000 0.204
#&gt; GSM11222     4  0.5538    0.31656 0.000 0.000 0.428 0.504 0.068
#&gt; GSM28720     1  0.0000    1.00000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000    1.00000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000    1.00000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000    1.00000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000    1.00000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000    1.00000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000    1.00000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000    1.00000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000    1.00000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000    1.00000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.1845    0.75130 0.000 0.928 0.000 0.056 0.016
#&gt; GSM28714     2  0.1845    0.75130 0.000 0.928 0.000 0.056 0.016
#&gt; GSM11216     3  0.2516    0.88087 0.000 0.000 0.860 0.000 0.140
#&gt; GSM28715     2  0.1845    0.75130 0.000 0.928 0.000 0.056 0.016
#&gt; GSM11234     2  0.3966    0.33274 0.000 0.664 0.000 0.000 0.336
#&gt; GSM28699     5  0.3949    1.00000 0.000 0.332 0.000 0.000 0.668
#&gt; GSM11233     5  0.3949    1.00000 0.000 0.332 0.000 0.000 0.668
#&gt; GSM28718     2  0.1845    0.75130 0.000 0.928 0.000 0.056 0.016
#&gt; GSM11231     2  0.4165    0.48414 0.000 0.672 0.000 0.320 0.008
#&gt; GSM11237     2  0.1845    0.75130 0.000 0.928 0.000 0.056 0.016
#&gt; GSM11228     4  0.4168    0.53460 0.000 0.184 0.000 0.764 0.052
#&gt; GSM28697     4  0.4168    0.53460 0.000 0.184 0.000 0.764 0.052
#&gt; GSM28698     3  0.0000    0.92724 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11238     3  0.0000    0.92724 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11242     3  0.0000    0.92724 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28719     4  0.0451    0.59093 0.000 0.004 0.000 0.988 0.008
#&gt; GSM28708     4  0.0451    0.59093 0.000 0.004 0.000 0.988 0.008
#&gt; GSM28722     2  0.4294    0.63179 0.000 0.768 0.000 0.152 0.080
#&gt; GSM11232     4  0.5850   -0.00915 0.000 0.428 0.000 0.476 0.096
#&gt; GSM28709     3  0.2516    0.88087 0.000 0.000 0.860 0.000 0.140
#&gt; GSM11226     4  0.5353    0.52729 0.000 0.000 0.272 0.636 0.092
#&gt; GSM11239     3  0.0000    0.92724 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11225     3  0.0000    0.92724 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11220     3  0.2516    0.88087 0.000 0.000 0.860 0.000 0.140
#&gt; GSM28701     2  0.5508    0.49125 0.000 0.636 0.000 0.244 0.120
#&gt; GSM28721     4  0.5972    0.56459 0.000 0.016 0.244 0.620 0.120
#&gt; GSM28713     2  0.1478    0.74532 0.000 0.936 0.000 0.000 0.064
#&gt; GSM28716     2  0.6062    0.03461 0.168 0.564 0.000 0.000 0.268
#&gt; GSM11221     2  0.0703    0.75112 0.000 0.976 0.000 0.000 0.024
#&gt; GSM28717     5  0.3949    1.00000 0.000 0.332 0.000 0.000 0.668
#&gt; GSM11223     1  0.0000    1.00000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     4  0.5353    0.52729 0.000 0.000 0.272 0.636 0.092
#&gt; GSM11219     2  0.1668    0.75528 0.000 0.940 0.000 0.032 0.028
#&gt; GSM11236     4  0.6768    0.22370 0.000 0.352 0.040 0.496 0.112
#&gt; GSM28702     4  0.5538    0.31656 0.000 0.000 0.428 0.504 0.068
#&gt; GSM28705     4  0.7773    0.49802 0.000 0.260 0.192 0.452 0.096
#&gt; GSM11230     2  0.1943    0.75189 0.000 0.924 0.000 0.056 0.020
#&gt; GSM28704     2  0.1892    0.73922 0.000 0.916 0.000 0.004 0.080
#&gt; GSM28700     2  0.3508    0.43331 0.000 0.748 0.000 0.000 0.252
#&gt; GSM11224     2  0.1792    0.69888 0.000 0.916 0.000 0.000 0.084
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-4-a').click(function(){
  $('#tab-CV-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-5'>
<p><a id='tab-CV-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28710     2   0.573     0.5236 0.000 0.624 0.000 0.120 0.204 0.052
#&gt; GSM28711     2   0.556     0.5422 0.000 0.644 0.000 0.108 0.196 0.052
#&gt; GSM28712     2   0.413     0.2237 0.000 0.600 0.000 0.016 0.384 0.000
#&gt; GSM11222     6   0.242     0.5199 0.000 0.000 0.156 0.000 0.000 0.844
#&gt; GSM28720     1   0.000     1.0000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1   0.000     1.0000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1   0.000     1.0000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1   0.000     1.0000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1   0.000     1.0000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1   0.000     1.0000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1   0.000     1.0000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1   0.000     1.0000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1   0.000     1.0000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1   0.000     1.0000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2   0.159     0.6963 0.000 0.932 0.000 0.052 0.016 0.000
#&gt; GSM28714     2   0.159     0.6963 0.000 0.932 0.000 0.052 0.016 0.000
#&gt; GSM11216     3   0.000     0.8219 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28715     2   0.159     0.6963 0.000 0.932 0.000 0.052 0.016 0.000
#&gt; GSM11234     5   0.479     0.0906 0.000 0.416 0.000 0.044 0.536 0.004
#&gt; GSM28699     5   0.196     0.6986 0.000 0.112 0.000 0.000 0.888 0.000
#&gt; GSM11233     5   0.196     0.6986 0.000 0.112 0.000 0.000 0.888 0.000
#&gt; GSM28718     2   0.159     0.6963 0.000 0.932 0.000 0.052 0.016 0.000
#&gt; GSM11231     2   0.384     0.4622 0.000 0.668 0.000 0.320 0.012 0.000
#&gt; GSM11237     2   0.159     0.6963 0.000 0.932 0.000 0.052 0.016 0.000
#&gt; GSM11228     4   0.427     0.6546 0.000 0.164 0.000 0.752 0.064 0.020
#&gt; GSM28697     4   0.427     0.6546 0.000 0.164 0.000 0.752 0.064 0.020
#&gt; GSM28698     3   0.273     0.8869 0.000 0.000 0.808 0.000 0.000 0.192
#&gt; GSM11238     3   0.273     0.8869 0.000 0.000 0.808 0.000 0.000 0.192
#&gt; GSM11242     3   0.273     0.8869 0.000 0.000 0.808 0.000 0.000 0.192
#&gt; GSM28719     4   0.144     0.5785 0.000 0.004 0.000 0.944 0.012 0.040
#&gt; GSM28708     4   0.144     0.5785 0.000 0.004 0.000 0.944 0.012 0.040
#&gt; GSM28722     2   0.524     0.5647 0.000 0.664 0.000 0.176 0.136 0.024
#&gt; GSM11232     4   0.585     0.1270 0.000 0.412 0.000 0.472 0.048 0.068
#&gt; GSM28709     3   0.000     0.8219 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11226     6   0.107     0.6121 0.000 0.000 0.000 0.048 0.000 0.952
#&gt; GSM11239     3   0.273     0.8869 0.000 0.000 0.808 0.000 0.000 0.192
#&gt; GSM11225     3   0.273     0.8869 0.000 0.000 0.808 0.000 0.000 0.192
#&gt; GSM11220     3   0.000     0.8219 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28701     2   0.674     0.2487 0.000 0.464 0.000 0.240 0.236 0.060
#&gt; GSM28721     6   0.413     0.4533 0.000 0.020 0.000 0.244 0.020 0.716
#&gt; GSM28713     2   0.266     0.6822 0.000 0.848 0.000 0.016 0.136 0.000
#&gt; GSM28716     5   0.646     0.2658 0.168 0.352 0.000 0.040 0.440 0.000
#&gt; GSM11221     2   0.217     0.6882 0.000 0.888 0.000 0.012 0.100 0.000
#&gt; GSM28717     5   0.196     0.6986 0.000 0.112 0.000 0.000 0.888 0.000
#&gt; GSM11223     1   0.000     1.0000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     6   0.107     0.6121 0.000 0.000 0.000 0.048 0.000 0.952
#&gt; GSM11219     2   0.263     0.6957 0.000 0.864 0.000 0.032 0.104 0.000
#&gt; GSM11236     6   0.725    -0.2441 0.000 0.304 0.000 0.300 0.088 0.308
#&gt; GSM28702     6   0.242     0.5199 0.000 0.000 0.156 0.000 0.000 0.844
#&gt; GSM28705     6   0.663    -0.1694 0.000 0.240 0.000 0.344 0.032 0.384
#&gt; GSM11230     2   0.168     0.6962 0.000 0.928 0.000 0.052 0.020 0.000
#&gt; GSM28704     2   0.311     0.6775 0.000 0.832 0.000 0.016 0.136 0.016
#&gt; GSM28700     2   0.425    -0.0399 0.000 0.528 0.000 0.016 0.456 0.000
#&gt; GSM11224     2   0.351     0.5458 0.000 0.744 0.000 0.016 0.240 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-5-a').click(function(){
  $('#tab-CV-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-hclust-signature_compare](figure_cola/CV-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-hclust-collect-classes](figure_cola/CV-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) k
#> CV:hclust 47     0.391 2
#> CV:hclust 19     0.392 3
#> CV:hclust 50     0.349 4
#> CV:hclust 44     0.401 5
#> CV:hclust 44     0.393 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:kmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "kmeans"]
# you can also extract it by
# res = res_list["CV:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21452 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-kmeans-collect-plots](figure_cola/CV-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-kmeans-select-partition-number](figure_cola/CV-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.293           0.472       0.686         0.4060 0.628   0.628
#> 3 3 0.861           0.910       0.959         0.4635 0.753   0.620
#> 4 4 0.698           0.812       0.869         0.2198 0.806   0.558
#> 5 5 0.729           0.593       0.780         0.0831 0.923   0.712
#> 6 6 0.733           0.639       0.725         0.0443 0.912   0.615
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-classes'>
<ul>
<li><a href='#tab-CV-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-kmeans-get-classes-1'>
<p><a id='tab-CV-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28710     2   1.000     0.5108 0.488 0.512
#&gt; GSM28711     2   1.000     0.5108 0.488 0.512
#&gt; GSM28712     2   1.000     0.5108 0.488 0.512
#&gt; GSM11222     2   0.866     0.0749 0.288 0.712
#&gt; GSM28720     1   0.000     0.8970 1.000 0.000
#&gt; GSM11217     1   0.000     0.8970 1.000 0.000
#&gt; GSM28723     1   0.000     0.8970 1.000 0.000
#&gt; GSM11241     1   0.000     0.8970 1.000 0.000
#&gt; GSM28703     1   0.000     0.8970 1.000 0.000
#&gt; GSM11227     1   0.000     0.8970 1.000 0.000
#&gt; GSM28706     1   0.000     0.8970 1.000 0.000
#&gt; GSM11229     1   0.000     0.8970 1.000 0.000
#&gt; GSM11235     1   0.000     0.8970 1.000 0.000
#&gt; GSM28707     1   0.000     0.8970 1.000 0.000
#&gt; GSM11240     2   1.000     0.5108 0.488 0.512
#&gt; GSM28714     2   1.000     0.5108 0.488 0.512
#&gt; GSM11216     2   0.866     0.0749 0.288 0.712
#&gt; GSM28715     2   0.999     0.5107 0.484 0.516
#&gt; GSM11234     2   1.000     0.5108 0.488 0.512
#&gt; GSM28699     2   1.000     0.5108 0.488 0.512
#&gt; GSM11233     2   1.000     0.5108 0.488 0.512
#&gt; GSM28718     2   1.000     0.5108 0.488 0.512
#&gt; GSM11231     2   0.999     0.5107 0.484 0.516
#&gt; GSM11237     2   1.000     0.5108 0.488 0.512
#&gt; GSM11228     2   0.969     0.4254 0.396 0.604
#&gt; GSM28697     2   0.999     0.5107 0.484 0.516
#&gt; GSM28698     2   0.866     0.0749 0.288 0.712
#&gt; GSM11238     2   0.866     0.0749 0.288 0.712
#&gt; GSM11242     2   0.866     0.0749 0.288 0.712
#&gt; GSM28719     2   0.999     0.5085 0.480 0.520
#&gt; GSM28708     2   0.900     0.3413 0.316 0.684
#&gt; GSM28722     2   0.999     0.5107 0.484 0.516
#&gt; GSM11232     2   0.958     0.4414 0.380 0.620
#&gt; GSM28709     2   0.866     0.0749 0.288 0.712
#&gt; GSM11226     2   0.900     0.3413 0.316 0.684
#&gt; GSM11239     2   0.866     0.0749 0.288 0.712
#&gt; GSM11225     2   0.866     0.0749 0.288 0.712
#&gt; GSM11220     2   0.866     0.0749 0.288 0.712
#&gt; GSM28701     2   1.000     0.5108 0.488 0.512
#&gt; GSM28721     2   0.518     0.2428 0.116 0.884
#&gt; GSM28713     2   1.000     0.5108 0.488 0.512
#&gt; GSM28716     1   0.866     0.1751 0.712 0.288
#&gt; GSM11221     2   1.000     0.5108 0.488 0.512
#&gt; GSM28717     2   1.000     0.5108 0.488 0.512
#&gt; GSM11223     1   0.000     0.8970 1.000 0.000
#&gt; GSM11218     2   0.260     0.2220 0.044 0.956
#&gt; GSM11219     2   1.000     0.5108 0.488 0.512
#&gt; GSM11236     1   0.966     0.0416 0.608 0.392
#&gt; GSM28702     2   0.866     0.0749 0.288 0.712
#&gt; GSM28705     2   0.975     0.4027 0.408 0.592
#&gt; GSM11230     2   0.999     0.5107 0.484 0.516
#&gt; GSM28704     2   1.000     0.5108 0.488 0.512
#&gt; GSM28700     2   1.000     0.5108 0.488 0.512
#&gt; GSM11224     2   1.000     0.5108 0.488 0.512
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-1-a').click(function(){
  $('#tab-CV-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-2'>
<p><a id='tab-CV-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28710     2  0.0237      0.936 0.000 0.996 0.004
#&gt; GSM28711     2  0.0000      0.937 0.000 1.000 0.000
#&gt; GSM28712     2  0.0237      0.936 0.000 0.996 0.004
#&gt; GSM11222     3  0.0237      1.000 0.004 0.000 0.996
#&gt; GSM28720     1  0.0000      0.964 1.000 0.000 0.000
#&gt; GSM11217     1  0.0000      0.964 1.000 0.000 0.000
#&gt; GSM28723     1  0.0000      0.964 1.000 0.000 0.000
#&gt; GSM11241     1  0.0000      0.964 1.000 0.000 0.000
#&gt; GSM28703     1  0.0000      0.964 1.000 0.000 0.000
#&gt; GSM11227     1  0.0000      0.964 1.000 0.000 0.000
#&gt; GSM28706     1  0.0000      0.964 1.000 0.000 0.000
#&gt; GSM11229     1  0.0000      0.964 1.000 0.000 0.000
#&gt; GSM11235     1  0.0000      0.964 1.000 0.000 0.000
#&gt; GSM28707     1  0.0000      0.964 1.000 0.000 0.000
#&gt; GSM11240     2  0.0000      0.937 0.000 1.000 0.000
#&gt; GSM28714     2  0.0000      0.937 0.000 1.000 0.000
#&gt; GSM11216     3  0.0237      1.000 0.004 0.000 0.996
#&gt; GSM28715     2  0.0000      0.937 0.000 1.000 0.000
#&gt; GSM11234     2  0.0237      0.936 0.000 0.996 0.004
#&gt; GSM28699     2  0.0237      0.936 0.000 0.996 0.004
#&gt; GSM11233     2  0.0237      0.936 0.000 0.996 0.004
#&gt; GSM28718     2  0.0000      0.937 0.000 1.000 0.000
#&gt; GSM11231     2  0.0000      0.937 0.000 1.000 0.000
#&gt; GSM11237     2  0.0000      0.937 0.000 1.000 0.000
#&gt; GSM11228     2  0.3482      0.840 0.000 0.872 0.128
#&gt; GSM28697     2  0.0000      0.937 0.000 1.000 0.000
#&gt; GSM28698     3  0.0237      1.000 0.004 0.000 0.996
#&gt; GSM11238     3  0.0237      1.000 0.004 0.000 0.996
#&gt; GSM11242     3  0.0237      1.000 0.004 0.000 0.996
#&gt; GSM28719     2  0.0000      0.937 0.000 1.000 0.000
#&gt; GSM28708     2  0.4931      0.733 0.000 0.768 0.232
#&gt; GSM28722     2  0.0000      0.937 0.000 1.000 0.000
#&gt; GSM11232     2  0.0000      0.937 0.000 1.000 0.000
#&gt; GSM28709     3  0.0237      1.000 0.004 0.000 0.996
#&gt; GSM11226     2  0.4887      0.738 0.000 0.772 0.228
#&gt; GSM11239     3  0.0237      1.000 0.004 0.000 0.996
#&gt; GSM11225     3  0.0237      1.000 0.004 0.000 0.996
#&gt; GSM11220     3  0.0237      1.000 0.004 0.000 0.996
#&gt; GSM28701     2  0.0000      0.937 0.000 1.000 0.000
#&gt; GSM28721     2  0.6225      0.355 0.000 0.568 0.432
#&gt; GSM28713     2  0.0237      0.936 0.000 0.996 0.004
#&gt; GSM28716     1  0.5690      0.571 0.708 0.288 0.004
#&gt; GSM11221     2  0.0000      0.937 0.000 1.000 0.000
#&gt; GSM28717     2  0.0237      0.936 0.000 0.996 0.004
#&gt; GSM11223     1  0.0000      0.964 1.000 0.000 0.000
#&gt; GSM11218     2  0.6225      0.355 0.000 0.568 0.432
#&gt; GSM11219     2  0.0000      0.937 0.000 1.000 0.000
#&gt; GSM11236     2  0.4931      0.754 0.004 0.784 0.212
#&gt; GSM28702     3  0.0237      1.000 0.004 0.000 0.996
#&gt; GSM28705     2  0.4605      0.766 0.000 0.796 0.204
#&gt; GSM11230     2  0.0000      0.937 0.000 1.000 0.000
#&gt; GSM28704     2  0.0000      0.937 0.000 1.000 0.000
#&gt; GSM28700     2  0.0237      0.936 0.000 0.996 0.004
#&gt; GSM11224     2  0.0237      0.936 0.000 0.996 0.004
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-2-a').click(function(){
  $('#tab-CV-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-3'>
<p><a id='tab-CV-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28710     2  0.2973     0.7429 0.000 0.856 0.000 0.144
#&gt; GSM28711     2  0.4356     0.6926 0.000 0.708 0.000 0.292
#&gt; GSM28712     2  0.0000     0.7764 0.000 1.000 0.000 0.000
#&gt; GSM11222     3  0.0592     0.9809 0.000 0.000 0.984 0.016
#&gt; GSM28720     1  0.0000     0.9982 1.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000     0.9982 1.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000     0.9982 1.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000     0.9982 1.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000     0.9982 1.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000     0.9982 1.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0469     0.9917 0.988 0.000 0.000 0.012
#&gt; GSM11229     1  0.0000     0.9982 1.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000     0.9982 1.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000     0.9982 1.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.3266     0.7718 0.000 0.832 0.000 0.168
#&gt; GSM28714     2  0.3266     0.7718 0.000 0.832 0.000 0.168
#&gt; GSM11216     3  0.0921     0.9767 0.000 0.000 0.972 0.028
#&gt; GSM28715     2  0.3266     0.7718 0.000 0.832 0.000 0.168
#&gt; GSM11234     2  0.3975     0.6039 0.000 0.760 0.000 0.240
#&gt; GSM28699     2  0.2469     0.7470 0.000 0.892 0.000 0.108
#&gt; GSM11233     2  0.1867     0.7417 0.000 0.928 0.000 0.072
#&gt; GSM28718     2  0.3266     0.7718 0.000 0.832 0.000 0.168
#&gt; GSM11231     4  0.4972     0.2505 0.000 0.456 0.000 0.544
#&gt; GSM11237     2  0.3356     0.7698 0.000 0.824 0.000 0.176
#&gt; GSM11228     4  0.2773     0.7918 0.000 0.116 0.004 0.880
#&gt; GSM28697     4  0.2868     0.7836 0.000 0.136 0.000 0.864
#&gt; GSM28698     3  0.0817     0.9775 0.000 0.000 0.976 0.024
#&gt; GSM11238     3  0.0188     0.9809 0.000 0.000 0.996 0.004
#&gt; GSM11242     3  0.0921     0.9792 0.000 0.000 0.972 0.028
#&gt; GSM28719     4  0.2760     0.7828 0.000 0.128 0.000 0.872
#&gt; GSM28708     4  0.4093     0.7974 0.000 0.096 0.072 0.832
#&gt; GSM28722     4  0.4998    -0.0525 0.000 0.488 0.000 0.512
#&gt; GSM11232     4  0.2973     0.7839 0.000 0.144 0.000 0.856
#&gt; GSM28709     3  0.0921     0.9767 0.000 0.000 0.972 0.028
#&gt; GSM11226     4  0.5452     0.7738 0.000 0.108 0.156 0.736
#&gt; GSM11239     3  0.0921     0.9792 0.000 0.000 0.972 0.028
#&gt; GSM11225     3  0.0921     0.9792 0.000 0.000 0.972 0.028
#&gt; GSM11220     3  0.0921     0.9767 0.000 0.000 0.972 0.028
#&gt; GSM28701     4  0.4624     0.4667 0.000 0.340 0.000 0.660
#&gt; GSM28721     4  0.5633     0.7574 0.000 0.100 0.184 0.716
#&gt; GSM28713     2  0.3266     0.7482 0.000 0.832 0.000 0.168
#&gt; GSM28716     2  0.5720     0.4621 0.296 0.652 0.000 0.052
#&gt; GSM11221     2  0.3764     0.7380 0.000 0.784 0.000 0.216
#&gt; GSM28717     2  0.2469     0.7470 0.000 0.892 0.000 0.108
#&gt; GSM11223     1  0.0469     0.9917 0.988 0.000 0.000 0.012
#&gt; GSM11218     4  0.5279     0.7468 0.000 0.072 0.192 0.736
#&gt; GSM11219     2  0.3219     0.7732 0.000 0.836 0.000 0.164
#&gt; GSM11236     4  0.4163     0.7981 0.000 0.096 0.076 0.828
#&gt; GSM28702     3  0.0592     0.9809 0.000 0.000 0.984 0.016
#&gt; GSM28705     4  0.4956     0.7904 0.000 0.108 0.116 0.776
#&gt; GSM11230     2  0.3219     0.7732 0.000 0.836 0.000 0.164
#&gt; GSM28704     2  0.4406     0.6613 0.000 0.700 0.000 0.300
#&gt; GSM28700     2  0.1389     0.7773 0.000 0.952 0.000 0.048
#&gt; GSM11224     2  0.2011     0.7882 0.000 0.920 0.000 0.080
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-3-a').click(function(){
  $('#tab-CV-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-4'>
<p><a id='tab-CV-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28710     5  0.5495     0.4983 0.000 0.436 0.000 0.064 0.500
#&gt; GSM28711     5  0.6497     0.3393 0.000 0.392 0.000 0.188 0.420
#&gt; GSM28712     2  0.3177     0.2399 0.000 0.792 0.000 0.000 0.208
#&gt; GSM11222     3  0.2573     0.8667 0.000 0.000 0.880 0.016 0.104
#&gt; GSM28720     1  0.0000     0.9775 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000     0.9775 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000     0.9775 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.1124     0.9651 0.960 0.000 0.000 0.004 0.036
#&gt; GSM28703     1  0.0000     0.9775 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000     0.9775 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.2470     0.9234 0.884 0.000 0.000 0.012 0.104
#&gt; GSM11229     1  0.0000     0.9775 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000     0.9775 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0794     0.9695 0.972 0.000 0.000 0.000 0.028
#&gt; GSM11240     2  0.1121     0.5616 0.000 0.956 0.000 0.044 0.000
#&gt; GSM28714     2  0.1121     0.5616 0.000 0.956 0.000 0.044 0.000
#&gt; GSM11216     3  0.2723     0.9018 0.000 0.000 0.864 0.012 0.124
#&gt; GSM28715     2  0.1907     0.5579 0.000 0.928 0.000 0.044 0.028
#&gt; GSM11234     5  0.6547     0.4546 0.000 0.296 0.000 0.232 0.472
#&gt; GSM28699     5  0.4291     0.4817 0.000 0.464 0.000 0.000 0.536
#&gt; GSM11233     2  0.3932     0.0263 0.000 0.672 0.000 0.000 0.328
#&gt; GSM28718     2  0.1121     0.5616 0.000 0.956 0.000 0.044 0.000
#&gt; GSM11231     2  0.4384     0.2812 0.000 0.660 0.000 0.324 0.016
#&gt; GSM11237     2  0.1121     0.5616 0.000 0.956 0.000 0.044 0.000
#&gt; GSM11228     4  0.1386     0.7773 0.000 0.016 0.000 0.952 0.032
#&gt; GSM28697     4  0.1469     0.7760 0.000 0.016 0.000 0.948 0.036
#&gt; GSM28698     3  0.2077     0.9127 0.000 0.000 0.908 0.008 0.084
#&gt; GSM11238     3  0.0000     0.9193 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11242     3  0.1124     0.9177 0.000 0.000 0.960 0.004 0.036
#&gt; GSM28719     4  0.2104     0.7680 0.000 0.024 0.000 0.916 0.060
#&gt; GSM28708     4  0.2130     0.7728 0.000 0.016 0.016 0.924 0.044
#&gt; GSM28722     4  0.6349    -0.1252 0.000 0.360 0.000 0.472 0.168
#&gt; GSM11232     4  0.1836     0.7738 0.000 0.036 0.000 0.932 0.032
#&gt; GSM28709     3  0.2723     0.9018 0.000 0.000 0.864 0.012 0.124
#&gt; GSM11226     4  0.5386     0.7017 0.000 0.012 0.140 0.696 0.152
#&gt; GSM11239     3  0.1124     0.9177 0.000 0.000 0.960 0.004 0.036
#&gt; GSM11225     3  0.1124     0.9177 0.000 0.000 0.960 0.004 0.036
#&gt; GSM11220     3  0.2723     0.9018 0.000 0.000 0.864 0.012 0.124
#&gt; GSM28701     4  0.5981    -0.1053 0.000 0.112 0.000 0.484 0.404
#&gt; GSM28721     4  0.5388     0.6968 0.000 0.012 0.148 0.696 0.144
#&gt; GSM28713     2  0.6064    -0.4734 0.000 0.460 0.000 0.120 0.420
#&gt; GSM28716     5  0.6628     0.3555 0.212 0.232 0.000 0.016 0.540
#&gt; GSM11221     2  0.6003    -0.4812 0.000 0.448 0.000 0.112 0.440
#&gt; GSM28717     5  0.4291     0.4817 0.000 0.464 0.000 0.000 0.536
#&gt; GSM11223     1  0.2470     0.9234 0.884 0.000 0.000 0.012 0.104
#&gt; GSM11218     4  0.5278     0.6855 0.000 0.004 0.156 0.692 0.148
#&gt; GSM11219     2  0.1981     0.5569 0.000 0.924 0.000 0.048 0.028
#&gt; GSM11236     4  0.2086     0.7759 0.000 0.020 0.008 0.924 0.048
#&gt; GSM28702     3  0.2573     0.8667 0.000 0.000 0.880 0.016 0.104
#&gt; GSM28705     4  0.3980     0.7533 0.000 0.012 0.080 0.816 0.092
#&gt; GSM11230     2  0.2304     0.5469 0.000 0.908 0.000 0.048 0.044
#&gt; GSM28704     2  0.6348    -0.2049 0.000 0.496 0.000 0.180 0.324
#&gt; GSM28700     2  0.4882    -0.4756 0.000 0.532 0.000 0.024 0.444
#&gt; GSM11224     2  0.5002    -0.2810 0.000 0.596 0.000 0.040 0.364
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-4-a').click(function(){
  $('#tab-CV-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-5'>
<p><a id='tab-CV-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28710     5   0.455    0.61673 0.000 0.220 0.000 0.060 0.704 0.016
#&gt; GSM28711     5   0.657    0.54075 0.000 0.272 0.000 0.112 0.512 0.104
#&gt; GSM28712     2   0.374   -0.04028 0.000 0.648 0.000 0.000 0.348 0.004
#&gt; GSM11222     3   0.355    0.69255 0.000 0.000 0.668 0.000 0.000 0.332
#&gt; GSM28720     1   0.000    0.94139 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1   0.000    0.94139 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1   0.000    0.94139 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1   0.221    0.90574 0.892 0.000 0.000 0.004 0.012 0.092
#&gt; GSM28703     1   0.000    0.94139 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1   0.000    0.94139 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1   0.409    0.80875 0.736 0.000 0.000 0.004 0.056 0.204
#&gt; GSM11229     1   0.000    0.94139 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1   0.000    0.94139 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1   0.181    0.91200 0.908 0.000 0.000 0.004 0.000 0.088
#&gt; GSM11240     2   0.000    0.72977 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28714     2   0.000    0.72977 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11216     3   0.158    0.83188 0.000 0.000 0.940 0.012 0.012 0.036
#&gt; GSM28715     2   0.137    0.71550 0.000 0.944 0.000 0.000 0.044 0.012
#&gt; GSM11234     5   0.528    0.56218 0.000 0.092 0.000 0.120 0.696 0.092
#&gt; GSM28699     5   0.492    0.51984 0.000 0.264 0.000 0.008 0.644 0.084
#&gt; GSM11233     5   0.525    0.30617 0.000 0.404 0.000 0.004 0.508 0.084
#&gt; GSM28718     2   0.000    0.72977 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11231     2   0.418    0.31050 0.000 0.600 0.000 0.384 0.004 0.012
#&gt; GSM11237     2   0.000    0.72977 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11228     4   0.240    0.54909 0.000 0.016 0.000 0.896 0.024 0.064
#&gt; GSM28697     4   0.252    0.55695 0.000 0.020 0.000 0.892 0.032 0.056
#&gt; GSM28698     3   0.026    0.84558 0.000 0.000 0.992 0.008 0.000 0.000
#&gt; GSM11238     3   0.275    0.85636 0.000 0.000 0.856 0.000 0.036 0.108
#&gt; GSM11242     3   0.304    0.85535 0.000 0.000 0.832 0.000 0.040 0.128
#&gt; GSM28719     4   0.143    0.55465 0.000 0.016 0.000 0.948 0.008 0.028
#&gt; GSM28708     4   0.214    0.50696 0.000 0.012 0.008 0.912 0.008 0.060
#&gt; GSM28722     5   0.757    0.27823 0.000 0.276 0.000 0.200 0.340 0.184
#&gt; GSM11232     4   0.399    0.41707 0.000 0.016 0.000 0.784 0.084 0.116
#&gt; GSM28709     3   0.158    0.83188 0.000 0.000 0.940 0.012 0.012 0.036
#&gt; GSM11226     6   0.632    0.98579 0.000 0.008 0.088 0.416 0.052 0.436
#&gt; GSM11239     3   0.304    0.85535 0.000 0.000 0.832 0.000 0.040 0.128
#&gt; GSM11225     3   0.304    0.85535 0.000 0.000 0.832 0.000 0.040 0.128
#&gt; GSM11220     3   0.158    0.83188 0.000 0.000 0.940 0.012 0.012 0.036
#&gt; GSM28701     4   0.631    0.00389 0.000 0.044 0.000 0.432 0.396 0.128
#&gt; GSM28721     6   0.631    0.98840 0.000 0.008 0.092 0.416 0.048 0.436
#&gt; GSM28713     5   0.592    0.55242 0.000 0.292 0.000 0.064 0.564 0.080
#&gt; GSM28716     5   0.601    0.45479 0.128 0.104 0.000 0.008 0.640 0.120
#&gt; GSM11221     5   0.587    0.54595 0.000 0.304 0.000 0.060 0.560 0.076
#&gt; GSM28717     5   0.492    0.51984 0.000 0.264 0.000 0.008 0.644 0.084
#&gt; GSM11223     1   0.409    0.80875 0.736 0.000 0.000 0.004 0.056 0.204
#&gt; GSM11218     6   0.636    0.99048 0.000 0.008 0.092 0.416 0.052 0.432
#&gt; GSM11219     2   0.155    0.71165 0.000 0.936 0.000 0.000 0.044 0.020
#&gt; GSM11236     4   0.381    0.50285 0.000 0.016 0.000 0.800 0.096 0.088
#&gt; GSM28702     3   0.355    0.69255 0.000 0.000 0.668 0.000 0.000 0.332
#&gt; GSM28705     4   0.613   -0.48533 0.000 0.012 0.044 0.544 0.088 0.312
#&gt; GSM11230     2   0.272    0.64204 0.000 0.864 0.000 0.000 0.084 0.052
#&gt; GSM28704     2   0.650   -0.39331 0.000 0.408 0.000 0.068 0.408 0.116
#&gt; GSM28700     5   0.400    0.56548 0.000 0.328 0.000 0.012 0.656 0.004
#&gt; GSM11224     5   0.527    0.43046 0.000 0.444 0.000 0.020 0.484 0.052
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-5-a').click(function(){
  $('#tab-CV-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-kmeans-signature_compare](figure_cola/CV-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-kmeans-collect-classes](figure_cola/CV-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) k
#> CV:kmeans 35     0.373 2
#> CV:kmeans 52     0.372 3
#> CV:kmeans 50     0.416 4
#> CV:kmeans 38     0.404 5
#> CV:kmeans 44     0.393 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:skmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "skmeans"]
# you can also extract it by
# res = res_list["CV:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21452 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-skmeans-collect-plots](figure_cola/CV-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-skmeans-select-partition-number](figure_cola/CV-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.499           0.870       0.907         0.4953 0.508   0.508
#> 3 3 1.000           0.971       0.988         0.3042 0.757   0.561
#> 4 4 0.900           0.865       0.944         0.1481 0.888   0.694
#> 5 5 0.798           0.655       0.846         0.0840 0.908   0.666
#> 6 6 0.788           0.651       0.816         0.0346 0.909   0.595
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
#> attr(,"optional")
#> [1] 3
```

There is also optional best $k$ = 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-classes'>
<ul>
<li><a href='#tab-CV-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-skmeans-get-classes-1'>
<p><a id='tab-CV-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28710     2  0.7139      0.894 0.196 0.804
#&gt; GSM28711     2  0.6623      0.892 0.172 0.828
#&gt; GSM28712     2  0.7139      0.894 0.196 0.804
#&gt; GSM11222     1  0.7139      0.871 0.804 0.196
#&gt; GSM28720     1  0.0672      0.867 0.992 0.008
#&gt; GSM11217     1  0.0672      0.867 0.992 0.008
#&gt; GSM28723     1  0.0672      0.867 0.992 0.008
#&gt; GSM11241     1  0.0672      0.867 0.992 0.008
#&gt; GSM28703     1  0.0672      0.867 0.992 0.008
#&gt; GSM11227     1  0.0672      0.867 0.992 0.008
#&gt; GSM28706     1  0.0672      0.867 0.992 0.008
#&gt; GSM11229     1  0.0672      0.867 0.992 0.008
#&gt; GSM11235     1  0.0672      0.867 0.992 0.008
#&gt; GSM28707     1  0.0672      0.867 0.992 0.008
#&gt; GSM11240     2  0.7139      0.894 0.196 0.804
#&gt; GSM28714     2  0.7139      0.894 0.196 0.804
#&gt; GSM11216     1  0.7139      0.871 0.804 0.196
#&gt; GSM28715     2  0.0000      0.846 0.000 1.000
#&gt; GSM11234     2  0.7139      0.894 0.196 0.804
#&gt; GSM28699     2  0.7139      0.894 0.196 0.804
#&gt; GSM11233     2  0.7139      0.894 0.196 0.804
#&gt; GSM28718     2  0.7139      0.894 0.196 0.804
#&gt; GSM11231     2  0.0000      0.846 0.000 1.000
#&gt; GSM11237     2  0.7139      0.894 0.196 0.804
#&gt; GSM11228     2  0.0672      0.841 0.008 0.992
#&gt; GSM28697     2  0.0672      0.841 0.008 0.992
#&gt; GSM28698     1  0.7139      0.871 0.804 0.196
#&gt; GSM11238     1  0.7139      0.871 0.804 0.196
#&gt; GSM11242     1  0.7139      0.871 0.804 0.196
#&gt; GSM28719     2  0.0672      0.841 0.008 0.992
#&gt; GSM28708     2  0.1184      0.838 0.016 0.984
#&gt; GSM28722     2  0.0000      0.846 0.000 1.000
#&gt; GSM11232     2  0.0672      0.841 0.008 0.992
#&gt; GSM28709     1  0.7139      0.871 0.804 0.196
#&gt; GSM11226     2  0.1184      0.838 0.016 0.984
#&gt; GSM11239     1  0.7139      0.871 0.804 0.196
#&gt; GSM11225     1  0.7139      0.871 0.804 0.196
#&gt; GSM11220     1  0.7139      0.871 0.804 0.196
#&gt; GSM28701     2  0.7299      0.893 0.204 0.796
#&gt; GSM28721     2  0.1184      0.838 0.016 0.984
#&gt; GSM28713     2  0.7139      0.894 0.196 0.804
#&gt; GSM28716     2  0.7139      0.894 0.196 0.804
#&gt; GSM11221     2  0.7139      0.894 0.196 0.804
#&gt; GSM28717     2  0.7139      0.894 0.196 0.804
#&gt; GSM11223     1  0.0672      0.867 0.992 0.008
#&gt; GSM11218     2  0.3879      0.781 0.076 0.924
#&gt; GSM11219     2  0.7139      0.894 0.196 0.804
#&gt; GSM11236     1  0.7139      0.871 0.804 0.196
#&gt; GSM28702     1  0.7139      0.871 0.804 0.196
#&gt; GSM28705     2  0.1184      0.838 0.016 0.984
#&gt; GSM11230     2  0.0000      0.846 0.000 1.000
#&gt; GSM28704     2  0.6712      0.892 0.176 0.824
#&gt; GSM28700     2  0.7139      0.894 0.196 0.804
#&gt; GSM11224     2  0.7139      0.894 0.196 0.804
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-1-a').click(function(){
  $('#tab-CV-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-2'>
<p><a id='tab-CV-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1    p2    p3
#&gt; GSM28710     2   0.000      0.977  0 1.000 0.000
#&gt; GSM28711     2   0.000      0.977  0 1.000 0.000
#&gt; GSM28712     2   0.000      0.977  0 1.000 0.000
#&gt; GSM11222     3   0.000      0.994  0 0.000 1.000
#&gt; GSM28720     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11217     1   0.000      1.000  1 0.000 0.000
#&gt; GSM28723     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11241     1   0.000      1.000  1 0.000 0.000
#&gt; GSM28703     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11227     1   0.000      1.000  1 0.000 0.000
#&gt; GSM28706     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11229     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11235     1   0.000      1.000  1 0.000 0.000
#&gt; GSM28707     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11240     2   0.000      0.977  0 1.000 0.000
#&gt; GSM28714     2   0.000      0.977  0 1.000 0.000
#&gt; GSM11216     3   0.000      0.994  0 0.000 1.000
#&gt; GSM28715     2   0.000      0.977  0 1.000 0.000
#&gt; GSM11234     2   0.000      0.977  0 1.000 0.000
#&gt; GSM28699     2   0.000      0.977  0 1.000 0.000
#&gt; GSM11233     2   0.000      0.977  0 1.000 0.000
#&gt; GSM28718     2   0.000      0.977  0 1.000 0.000
#&gt; GSM11231     2   0.000      0.977  0 1.000 0.000
#&gt; GSM11237     2   0.000      0.977  0 1.000 0.000
#&gt; GSM11228     2   0.595      0.450  0 0.640 0.360
#&gt; GSM28697     2   0.000      0.977  0 1.000 0.000
#&gt; GSM28698     3   0.000      0.994  0 0.000 1.000
#&gt; GSM11238     3   0.000      0.994  0 0.000 1.000
#&gt; GSM11242     3   0.000      0.994  0 0.000 1.000
#&gt; GSM28719     2   0.000      0.977  0 1.000 0.000
#&gt; GSM28708     3   0.000      0.994  0 0.000 1.000
#&gt; GSM28722     2   0.000      0.977  0 1.000 0.000
#&gt; GSM11232     2   0.450      0.753  0 0.804 0.196
#&gt; GSM28709     3   0.000      0.994  0 0.000 1.000
#&gt; GSM11226     3   0.000      0.994  0 0.000 1.000
#&gt; GSM11239     3   0.000      0.994  0 0.000 1.000
#&gt; GSM11225     3   0.000      0.994  0 0.000 1.000
#&gt; GSM11220     3   0.000      0.994  0 0.000 1.000
#&gt; GSM28701     2   0.000      0.977  0 1.000 0.000
#&gt; GSM28721     3   0.000      0.994  0 0.000 1.000
#&gt; GSM28713     2   0.000      0.977  0 1.000 0.000
#&gt; GSM28716     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11221     2   0.000      0.977  0 1.000 0.000
#&gt; GSM28717     2   0.000      0.977  0 1.000 0.000
#&gt; GSM11223     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11218     3   0.000      0.994  0 0.000 1.000
#&gt; GSM11219     2   0.000      0.977  0 1.000 0.000
#&gt; GSM11236     3   0.000      0.994  0 0.000 1.000
#&gt; GSM28702     3   0.000      0.994  0 0.000 1.000
#&gt; GSM28705     3   0.245      0.904  0 0.076 0.924
#&gt; GSM11230     2   0.000      0.977  0 1.000 0.000
#&gt; GSM28704     2   0.000      0.977  0 1.000 0.000
#&gt; GSM28700     2   0.000      0.977  0 1.000 0.000
#&gt; GSM11224     2   0.000      0.977  0 1.000 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-2-a').click(function(){
  $('#tab-CV-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-3'>
<p><a id='tab-CV-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1    p2    p3    p4
#&gt; GSM28710     2  0.0336      0.951  0 0.992 0.000 0.008
#&gt; GSM28711     2  0.0188      0.952  0 0.996 0.000 0.004
#&gt; GSM28712     2  0.0469      0.953  0 0.988 0.000 0.012
#&gt; GSM11222     3  0.0000      0.891  0 0.000 1.000 0.000
#&gt; GSM28720     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11240     2  0.0469      0.953  0 0.988 0.000 0.012
#&gt; GSM28714     2  0.0469      0.953  0 0.988 0.000 0.012
#&gt; GSM11216     3  0.0000      0.891  0 0.000 1.000 0.000
#&gt; GSM28715     2  0.0469      0.953  0 0.988 0.000 0.012
#&gt; GSM11234     2  0.3907      0.690  0 0.768 0.000 0.232
#&gt; GSM28699     2  0.0336      0.951  0 0.992 0.000 0.008
#&gt; GSM11233     2  0.0707      0.951  0 0.980 0.000 0.020
#&gt; GSM28718     2  0.0469      0.953  0 0.988 0.000 0.012
#&gt; GSM11231     4  0.4843      0.315  0 0.396 0.000 0.604
#&gt; GSM11237     2  0.0469      0.953  0 0.988 0.000 0.012
#&gt; GSM11228     4  0.0592      0.826  0 0.000 0.016 0.984
#&gt; GSM28697     4  0.0469      0.827  0 0.012 0.000 0.988
#&gt; GSM28698     3  0.0000      0.891  0 0.000 1.000 0.000
#&gt; GSM11238     3  0.0000      0.891  0 0.000 1.000 0.000
#&gt; GSM11242     3  0.0000      0.891  0 0.000 1.000 0.000
#&gt; GSM28719     4  0.0000      0.829  0 0.000 0.000 1.000
#&gt; GSM28708     4  0.0707      0.823  0 0.000 0.020 0.980
#&gt; GSM28722     2  0.4697      0.428  0 0.644 0.000 0.356
#&gt; GSM11232     4  0.0336      0.828  0 0.008 0.000 0.992
#&gt; GSM28709     3  0.0000      0.891  0 0.000 1.000 0.000
#&gt; GSM11226     3  0.4072      0.663  0 0.000 0.748 0.252
#&gt; GSM11239     3  0.0000      0.891  0 0.000 1.000 0.000
#&gt; GSM11225     3  0.0000      0.891  0 0.000 1.000 0.000
#&gt; GSM11220     3  0.0000      0.891  0 0.000 1.000 0.000
#&gt; GSM28701     4  0.2814      0.754  0 0.132 0.000 0.868
#&gt; GSM28721     3  0.4817      0.406  0 0.000 0.612 0.388
#&gt; GSM28713     2  0.0469      0.947  0 0.988 0.000 0.012
#&gt; GSM28716     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11221     2  0.0188      0.951  0 0.996 0.000 0.004
#&gt; GSM28717     2  0.0336      0.951  0 0.992 0.000 0.008
#&gt; GSM11223     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11218     3  0.3975      0.679  0 0.000 0.760 0.240
#&gt; GSM11219     2  0.0469      0.953  0 0.988 0.000 0.012
#&gt; GSM11236     3  0.4679      0.399  0 0.000 0.648 0.352
#&gt; GSM28702     3  0.0000      0.891  0 0.000 1.000 0.000
#&gt; GSM28705     4  0.4679      0.291  0 0.000 0.352 0.648
#&gt; GSM11230     2  0.0469      0.953  0 0.988 0.000 0.012
#&gt; GSM28704     2  0.2081      0.879  0 0.916 0.000 0.084
#&gt; GSM28700     2  0.0336      0.951  0 0.992 0.000 0.008
#&gt; GSM11224     2  0.0000      0.952  0 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-3-a').click(function(){
  $('#tab-CV-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-4'>
<p><a id='tab-CV-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28710     5  0.3081     0.5856 0.000 0.156 0.000 0.012 0.832
#&gt; GSM28711     5  0.5039     0.1562 0.000 0.456 0.000 0.032 0.512
#&gt; GSM28712     2  0.3707     0.2495 0.000 0.716 0.000 0.000 0.284
#&gt; GSM11222     3  0.0324     0.8494 0.000 0.000 0.992 0.004 0.004
#&gt; GSM28720     1  0.0000     0.9857 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000     0.9857 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000     0.9857 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000     0.9857 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000     0.9857 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000     0.9857 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000     0.9857 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000     0.9857 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000     0.9857 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000     0.9857 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.0000     0.6764 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28714     2  0.0000     0.6764 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11216     3  0.0162     0.8510 0.000 0.000 0.996 0.000 0.004
#&gt; GSM28715     2  0.1197     0.6666 0.000 0.952 0.000 0.000 0.048
#&gt; GSM11234     5  0.3037     0.5687 0.000 0.100 0.000 0.040 0.860
#&gt; GSM28699     5  0.3612     0.5430 0.000 0.268 0.000 0.000 0.732
#&gt; GSM11233     2  0.4273    -0.1340 0.000 0.552 0.000 0.000 0.448
#&gt; GSM28718     2  0.0000     0.6764 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11231     2  0.4114     0.2761 0.000 0.624 0.000 0.376 0.000
#&gt; GSM11237     2  0.0000     0.6764 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11228     4  0.0290     0.8869 0.000 0.000 0.000 0.992 0.008
#&gt; GSM28697     4  0.0290     0.8869 0.000 0.000 0.000 0.992 0.008
#&gt; GSM28698     3  0.0000     0.8520 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11238     3  0.0000     0.8520 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11242     3  0.0000     0.8520 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28719     4  0.0566     0.8827 0.000 0.004 0.000 0.984 0.012
#&gt; GSM28708     4  0.0324     0.8840 0.000 0.004 0.000 0.992 0.004
#&gt; GSM28722     2  0.6386     0.0237 0.000 0.460 0.000 0.172 0.368
#&gt; GSM11232     4  0.2020     0.8355 0.000 0.000 0.000 0.900 0.100
#&gt; GSM28709     3  0.0162     0.8510 0.000 0.000 0.996 0.000 0.004
#&gt; GSM11226     3  0.5264     0.2774 0.000 0.000 0.556 0.392 0.052
#&gt; GSM11239     3  0.0000     0.8520 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11225     3  0.0000     0.8520 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11220     3  0.0162     0.8510 0.000 0.000 0.996 0.000 0.004
#&gt; GSM28701     5  0.5351     0.0346 0.000 0.052 0.000 0.464 0.484
#&gt; GSM28721     3  0.5344     0.1137 0.000 0.000 0.500 0.448 0.052
#&gt; GSM28713     5  0.4040     0.4702 0.000 0.276 0.000 0.012 0.712
#&gt; GSM28716     1  0.2648     0.8206 0.848 0.000 0.000 0.000 0.152
#&gt; GSM11221     5  0.4537     0.2802 0.000 0.396 0.000 0.012 0.592
#&gt; GSM28717     5  0.3636     0.5394 0.000 0.272 0.000 0.000 0.728
#&gt; GSM11223     1  0.0000     0.9857 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     3  0.5037     0.3365 0.000 0.000 0.584 0.376 0.040
#&gt; GSM11219     2  0.1792     0.6470 0.000 0.916 0.000 0.000 0.084
#&gt; GSM11236     3  0.3838     0.5150 0.000 0.000 0.716 0.280 0.004
#&gt; GSM28702     3  0.0324     0.8494 0.000 0.000 0.992 0.004 0.004
#&gt; GSM28705     4  0.5639     0.4477 0.000 0.000 0.260 0.616 0.124
#&gt; GSM11230     2  0.1908     0.6379 0.000 0.908 0.000 0.000 0.092
#&gt; GSM28704     2  0.4590     0.1207 0.000 0.568 0.000 0.012 0.420
#&gt; GSM28700     5  0.3949     0.5433 0.000 0.300 0.000 0.004 0.696
#&gt; GSM11224     5  0.4450     0.2139 0.000 0.488 0.000 0.004 0.508
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-4-a').click(function(){
  $('#tab-CV-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-5'>
<p><a id='tab-CV-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette   p1    p2    p3    p4    p5    p6
#&gt; GSM28710     5  0.2842      0.588 0.00 0.076 0.000 0.012 0.868 0.044
#&gt; GSM28711     5  0.6560      0.243 0.00 0.300 0.000 0.040 0.452 0.208
#&gt; GSM28712     2  0.4535      0.243 0.00 0.644 0.000 0.000 0.296 0.060
#&gt; GSM11222     3  0.2320      0.816 0.00 0.000 0.864 0.004 0.000 0.132
#&gt; GSM28720     1  0.0000      0.977 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      0.977 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      0.977 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      0.977 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      0.977 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      0.977 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      0.977 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      0.977 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      0.977 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      0.977 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.0000      0.781 0.00 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28714     2  0.0000      0.781 0.00 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11216     3  0.0146      0.924 0.00 0.000 0.996 0.000 0.000 0.004
#&gt; GSM28715     2  0.1382      0.763 0.00 0.948 0.000 0.008 0.036 0.008
#&gt; GSM11234     5  0.4790      0.491 0.00 0.032 0.000 0.036 0.660 0.272
#&gt; GSM28699     5  0.2597      0.567 0.00 0.176 0.000 0.000 0.824 0.000
#&gt; GSM11233     5  0.3765      0.300 0.00 0.404 0.000 0.000 0.596 0.000
#&gt; GSM28718     2  0.0000      0.781 0.00 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11231     4  0.4096      0.086 0.00 0.484 0.000 0.508 0.008 0.000
#&gt; GSM11237     2  0.0000      0.781 0.00 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11228     4  0.1245      0.700 0.00 0.000 0.000 0.952 0.016 0.032
#&gt; GSM28697     4  0.1745      0.697 0.00 0.000 0.000 0.924 0.020 0.056
#&gt; GSM28698     3  0.0000      0.925 0.00 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11238     3  0.0260      0.925 0.00 0.000 0.992 0.000 0.000 0.008
#&gt; GSM11242     3  0.0260      0.925 0.00 0.000 0.992 0.000 0.000 0.008
#&gt; GSM28719     4  0.0520      0.702 0.00 0.000 0.000 0.984 0.008 0.008
#&gt; GSM28708     4  0.0632      0.697 0.00 0.000 0.000 0.976 0.000 0.024
#&gt; GSM28722     6  0.6184     -0.105 0.00 0.212 0.000 0.048 0.180 0.560
#&gt; GSM11232     4  0.5201      0.380 0.00 0.012 0.000 0.604 0.088 0.296
#&gt; GSM28709     3  0.0146      0.924 0.00 0.000 0.996 0.000 0.000 0.004
#&gt; GSM11226     6  0.5340      0.495 0.00 0.000 0.284 0.128 0.004 0.584
#&gt; GSM11239     3  0.0260      0.925 0.00 0.000 0.992 0.000 0.000 0.008
#&gt; GSM11225     3  0.0260      0.925 0.00 0.000 0.992 0.000 0.000 0.008
#&gt; GSM11220     3  0.0146      0.924 0.00 0.000 0.996 0.000 0.000 0.004
#&gt; GSM28701     4  0.5931      0.262 0.00 0.012 0.000 0.508 0.308 0.172
#&gt; GSM28721     6  0.5377      0.493 0.00 0.000 0.272 0.156 0.000 0.572
#&gt; GSM28713     5  0.6141      0.352 0.00 0.184 0.000 0.016 0.464 0.336
#&gt; GSM28716     1  0.3189      0.683 0.76 0.000 0.000 0.000 0.236 0.004
#&gt; GSM11221     5  0.6376      0.278 0.00 0.252 0.000 0.016 0.416 0.316
#&gt; GSM28717     5  0.2562      0.569 0.00 0.172 0.000 0.000 0.828 0.000
#&gt; GSM11223     1  0.0000      0.977 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     6  0.5192      0.462 0.00 0.000 0.308 0.116 0.000 0.576
#&gt; GSM11219     2  0.2258      0.733 0.00 0.896 0.000 0.000 0.060 0.044
#&gt; GSM11236     3  0.4200      0.541 0.00 0.000 0.696 0.264 0.008 0.032
#&gt; GSM28702     3  0.2320      0.816 0.00 0.000 0.864 0.004 0.000 0.132
#&gt; GSM28705     6  0.4817      0.372 0.00 0.000 0.112 0.196 0.008 0.684
#&gt; GSM11230     2  0.3644      0.625 0.00 0.792 0.000 0.000 0.088 0.120
#&gt; GSM28704     6  0.6327     -0.287 0.00 0.364 0.000 0.016 0.220 0.400
#&gt; GSM28700     5  0.5126      0.501 0.00 0.216 0.000 0.000 0.624 0.160
#&gt; GSM11224     2  0.6000     -0.199 0.00 0.440 0.000 0.004 0.352 0.204
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-5-a').click(function(){
  $('#tab-CV-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-skmeans-signature_compare](figure_cola/CV-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-skmeans-collect-classes](figure_cola/CV-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> CV:skmeans 54     0.398 2
#> CV:skmeans 53     0.373 3
#> CV:skmeans 49     0.415 4
#> CV:skmeans 40     0.397 5
#> CV:skmeans 38     0.394 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:pam*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "pam"]
# you can also extract it by
# res = res_list["CV:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21452 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-pam-collect-plots](figure_cola/CV-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-pam-select-partition-number](figure_cola/CV-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.744           0.802       0.929         0.4147 0.591   0.591
#> 3 3 0.835           0.891       0.957         0.4740 0.709   0.541
#> 4 4 0.780           0.825       0.915         0.1942 0.868   0.662
#> 5 5 0.857           0.880       0.915         0.0895 0.899   0.645
#> 6 6 0.905           0.887       0.931         0.0313 0.976   0.884
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 6
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-classes'>
<ul>
<li><a href='#tab-CV-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-pam-get-classes-1'>
<p><a id='tab-CV-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28710     2   0.000     0.9291 0.000 1.000
#&gt; GSM28711     2   0.000     0.9291 0.000 1.000
#&gt; GSM28712     2   0.000     0.9291 0.000 1.000
#&gt; GSM11222     2   0.000     0.9291 0.000 1.000
#&gt; GSM28720     1   0.000     0.8701 1.000 0.000
#&gt; GSM11217     1   0.000     0.8701 1.000 0.000
#&gt; GSM28723     1   0.000     0.8701 1.000 0.000
#&gt; GSM11241     1   0.000     0.8701 1.000 0.000
#&gt; GSM28703     1   0.000     0.8701 1.000 0.000
#&gt; GSM11227     1   0.000     0.8701 1.000 0.000
#&gt; GSM28706     1   0.000     0.8701 1.000 0.000
#&gt; GSM11229     1   0.000     0.8701 1.000 0.000
#&gt; GSM11235     1   0.000     0.8701 1.000 0.000
#&gt; GSM28707     1   0.000     0.8701 1.000 0.000
#&gt; GSM11240     2   0.000     0.9291 0.000 1.000
#&gt; GSM28714     2   0.000     0.9291 0.000 1.000
#&gt; GSM11216     1   0.978     0.3103 0.588 0.412
#&gt; GSM28715     2   0.000     0.9291 0.000 1.000
#&gt; GSM11234     2   0.000     0.9291 0.000 1.000
#&gt; GSM28699     2   0.760     0.6382 0.220 0.780
#&gt; GSM11233     2   0.000     0.9291 0.000 1.000
#&gt; GSM28718     2   0.000     0.9291 0.000 1.000
#&gt; GSM11231     2   0.000     0.9291 0.000 1.000
#&gt; GSM11237     2   0.000     0.9291 0.000 1.000
#&gt; GSM11228     2   0.000     0.9291 0.000 1.000
#&gt; GSM28697     2   0.000     0.9291 0.000 1.000
#&gt; GSM28698     1   0.987     0.2530 0.568 0.432
#&gt; GSM11238     2   0.990     0.1391 0.440 0.560
#&gt; GSM11242     2   0.814     0.5966 0.252 0.748
#&gt; GSM28719     2   0.000     0.9291 0.000 1.000
#&gt; GSM28708     2   0.000     0.9291 0.000 1.000
#&gt; GSM28722     2   0.000     0.9291 0.000 1.000
#&gt; GSM11232     2   0.000     0.9291 0.000 1.000
#&gt; GSM28709     2   0.993     0.0972 0.452 0.548
#&gt; GSM11226     2   0.000     0.9291 0.000 1.000
#&gt; GSM11239     2   0.983     0.1894 0.424 0.576
#&gt; GSM11225     2   0.991     0.1256 0.444 0.556
#&gt; GSM11220     1   0.891     0.5317 0.692 0.308
#&gt; GSM28701     2   0.000     0.9291 0.000 1.000
#&gt; GSM28721     2   0.000     0.9291 0.000 1.000
#&gt; GSM28713     2   0.000     0.9291 0.000 1.000
#&gt; GSM28716     1   0.990     0.1980 0.560 0.440
#&gt; GSM11221     2   0.000     0.9291 0.000 1.000
#&gt; GSM28717     2   0.000     0.9291 0.000 1.000
#&gt; GSM11223     1   0.000     0.8701 1.000 0.000
#&gt; GSM11218     2   0.000     0.9291 0.000 1.000
#&gt; GSM11219     2   0.000     0.9291 0.000 1.000
#&gt; GSM11236     2   0.000     0.9291 0.000 1.000
#&gt; GSM28702     2   0.000     0.9291 0.000 1.000
#&gt; GSM28705     2   0.000     0.9291 0.000 1.000
#&gt; GSM11230     2   0.000     0.9291 0.000 1.000
#&gt; GSM28704     2   0.000     0.9291 0.000 1.000
#&gt; GSM28700     2   0.000     0.9291 0.000 1.000
#&gt; GSM11224     2   0.000     0.9291 0.000 1.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-1-a').click(function(){
  $('#tab-CV-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-2'>
<p><a id='tab-CV-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette  p1    p2    p3
#&gt; GSM28710     2   0.000     0.9528 0.0 1.000 0.000
#&gt; GSM28711     2   0.000     0.9528 0.0 1.000 0.000
#&gt; GSM28712     2   0.000     0.9528 0.0 1.000 0.000
#&gt; GSM11222     3   0.000     0.8801 0.0 0.000 1.000
#&gt; GSM28720     1   0.000     1.0000 1.0 0.000 0.000
#&gt; GSM11217     1   0.000     1.0000 1.0 0.000 0.000
#&gt; GSM28723     1   0.000     1.0000 1.0 0.000 0.000
#&gt; GSM11241     1   0.000     1.0000 1.0 0.000 0.000
#&gt; GSM28703     1   0.000     1.0000 1.0 0.000 0.000
#&gt; GSM11227     1   0.000     1.0000 1.0 0.000 0.000
#&gt; GSM28706     1   0.000     1.0000 1.0 0.000 0.000
#&gt; GSM11229     1   0.000     1.0000 1.0 0.000 0.000
#&gt; GSM11235     1   0.000     1.0000 1.0 0.000 0.000
#&gt; GSM28707     1   0.000     1.0000 1.0 0.000 0.000
#&gt; GSM11240     2   0.000     0.9528 0.0 1.000 0.000
#&gt; GSM28714     2   0.000     0.9528 0.0 1.000 0.000
#&gt; GSM11216     3   0.000     0.8801 0.0 0.000 1.000
#&gt; GSM28715     2   0.000     0.9528 0.0 1.000 0.000
#&gt; GSM11234     2   0.000     0.9528 0.0 1.000 0.000
#&gt; GSM28699     2   0.000     0.9528 0.0 1.000 0.000
#&gt; GSM11233     2   0.000     0.9528 0.0 1.000 0.000
#&gt; GSM28718     2   0.000     0.9528 0.0 1.000 0.000
#&gt; GSM11231     2   0.000     0.9528 0.0 1.000 0.000
#&gt; GSM11237     2   0.000     0.9528 0.0 1.000 0.000
#&gt; GSM11228     2   0.382     0.7858 0.0 0.852 0.148
#&gt; GSM28697     2   0.000     0.9528 0.0 1.000 0.000
#&gt; GSM28698     3   0.000     0.8801 0.0 0.000 1.000
#&gt; GSM11238     3   0.000     0.8801 0.0 0.000 1.000
#&gt; GSM11242     3   0.000     0.8801 0.0 0.000 1.000
#&gt; GSM28719     2   0.000     0.9528 0.0 1.000 0.000
#&gt; GSM28708     3   0.546     0.6632 0.0 0.288 0.712
#&gt; GSM28722     2   0.000     0.9528 0.0 1.000 0.000
#&gt; GSM11232     2   0.000     0.9528 0.0 1.000 0.000
#&gt; GSM28709     3   0.000     0.8801 0.0 0.000 1.000
#&gt; GSM11226     3   0.546     0.6632 0.0 0.288 0.712
#&gt; GSM11239     3   0.000     0.8801 0.0 0.000 1.000
#&gt; GSM11225     3   0.000     0.8801 0.0 0.000 1.000
#&gt; GSM11220     3   0.000     0.8801 0.0 0.000 1.000
#&gt; GSM28701     2   0.000     0.9528 0.0 1.000 0.000
#&gt; GSM28721     3   0.546     0.6632 0.0 0.288 0.712
#&gt; GSM28713     2   0.000     0.9528 0.0 1.000 0.000
#&gt; GSM28716     2   0.631     0.0495 0.5 0.500 0.000
#&gt; GSM11221     2   0.000     0.9528 0.0 1.000 0.000
#&gt; GSM28717     2   0.000     0.9528 0.0 1.000 0.000
#&gt; GSM11223     1   0.000     1.0000 1.0 0.000 0.000
#&gt; GSM11218     3   0.546     0.6632 0.0 0.288 0.712
#&gt; GSM11219     2   0.000     0.9528 0.0 1.000 0.000
#&gt; GSM11236     2   0.341     0.8182 0.0 0.876 0.124
#&gt; GSM28702     3   0.000     0.8801 0.0 0.000 1.000
#&gt; GSM28705     2   0.615     0.1994 0.0 0.592 0.408
#&gt; GSM11230     2   0.000     0.9528 0.0 1.000 0.000
#&gt; GSM28704     2   0.000     0.9528 0.0 1.000 0.000
#&gt; GSM28700     2   0.000     0.9528 0.0 1.000 0.000
#&gt; GSM11224     2   0.000     0.9528 0.0 1.000 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-2-a').click(function(){
  $('#tab-CV-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-3'>
<p><a id='tab-CV-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28710     4  0.0000      0.891 0.000 0.000 0.000 1.000
#&gt; GSM28711     4  0.4730      0.253 0.000 0.364 0.000 0.636
#&gt; GSM28712     4  0.0336      0.887 0.000 0.008 0.000 0.992
#&gt; GSM11222     3  0.0000      0.870 0.000 0.000 1.000 0.000
#&gt; GSM28720     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.3266      0.864 0.000 0.832 0.000 0.168
#&gt; GSM28714     2  0.3266      0.864 0.000 0.832 0.000 0.168
#&gt; GSM11216     3  0.0000      0.870 0.000 0.000 1.000 0.000
#&gt; GSM28715     2  0.2149      0.849 0.000 0.912 0.000 0.088
#&gt; GSM11234     4  0.0000      0.891 0.000 0.000 0.000 1.000
#&gt; GSM28699     4  0.0000      0.891 0.000 0.000 0.000 1.000
#&gt; GSM11233     2  0.3688      0.830 0.000 0.792 0.000 0.208
#&gt; GSM28718     2  0.3266      0.864 0.000 0.832 0.000 0.168
#&gt; GSM11231     2  0.0817      0.805 0.000 0.976 0.000 0.024
#&gt; GSM11237     2  0.1637      0.834 0.000 0.940 0.000 0.060
#&gt; GSM11228     4  0.4149      0.782 0.000 0.168 0.028 0.804
#&gt; GSM28697     4  0.1022      0.879 0.000 0.032 0.000 0.968
#&gt; GSM28698     3  0.0000      0.870 0.000 0.000 1.000 0.000
#&gt; GSM11238     3  0.0000      0.870 0.000 0.000 1.000 0.000
#&gt; GSM11242     3  0.0000      0.870 0.000 0.000 1.000 0.000
#&gt; GSM28719     4  0.3569      0.783 0.000 0.196 0.000 0.804
#&gt; GSM28708     2  0.4994     -0.131 0.000 0.520 0.480 0.000
#&gt; GSM28722     4  0.0000      0.891 0.000 0.000 0.000 1.000
#&gt; GSM11232     4  0.3024      0.810 0.000 0.148 0.000 0.852
#&gt; GSM28709     3  0.0000      0.870 0.000 0.000 1.000 0.000
#&gt; GSM11226     3  0.5159      0.451 0.000 0.012 0.624 0.364
#&gt; GSM11239     3  0.0000      0.870 0.000 0.000 1.000 0.000
#&gt; GSM11225     3  0.0000      0.870 0.000 0.000 1.000 0.000
#&gt; GSM11220     3  0.0000      0.870 0.000 0.000 1.000 0.000
#&gt; GSM28701     4  0.0000      0.891 0.000 0.000 0.000 1.000
#&gt; GSM28721     3  0.5159      0.451 0.000 0.012 0.624 0.364
#&gt; GSM28713     4  0.0000      0.891 0.000 0.000 0.000 1.000
#&gt; GSM28716     4  0.3837      0.702 0.224 0.000 0.000 0.776
#&gt; GSM11221     4  0.0000      0.891 0.000 0.000 0.000 1.000
#&gt; GSM28717     4  0.0817      0.878 0.000 0.024 0.000 0.976
#&gt; GSM11223     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11218     3  0.5159      0.451 0.000 0.012 0.624 0.364
#&gt; GSM11219     4  0.2216      0.828 0.000 0.092 0.000 0.908
#&gt; GSM11236     4  0.6922      0.422 0.000 0.168 0.248 0.584
#&gt; GSM28702     3  0.0000      0.870 0.000 0.000 1.000 0.000
#&gt; GSM28705     4  0.4267      0.708 0.000 0.024 0.188 0.788
#&gt; GSM11230     2  0.3266      0.864 0.000 0.832 0.000 0.168
#&gt; GSM28704     4  0.0000      0.891 0.000 0.000 0.000 1.000
#&gt; GSM28700     4  0.0000      0.891 0.000 0.000 0.000 1.000
#&gt; GSM11224     4  0.0000      0.891 0.000 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-3-a').click(function(){
  $('#tab-CV-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-4'>
<p><a id='tab-CV-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28710     2  0.1732      0.892 0.000 0.920 0.000 0.000 0.080
#&gt; GSM28711     4  0.6204      0.396 0.000 0.176 0.000 0.536 0.288
#&gt; GSM28712     2  0.2690      0.851 0.000 0.844 0.000 0.000 0.156
#&gt; GSM11222     4  0.3305      0.797 0.000 0.000 0.224 0.776 0.000
#&gt; GSM28720     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     5  0.0000      0.942 0.000 0.000 0.000 0.000 1.000
#&gt; GSM28714     5  0.0000      0.942 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11216     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28715     5  0.0000      0.942 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11234     2  0.0000      0.873 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28699     2  0.0000      0.873 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11233     5  0.2966      0.814 0.000 0.184 0.000 0.000 0.816
#&gt; GSM28718     5  0.0000      0.942 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11231     5  0.2648      0.841 0.000 0.000 0.000 0.152 0.848
#&gt; GSM11237     5  0.1197      0.915 0.000 0.000 0.000 0.048 0.952
#&gt; GSM11228     2  0.3837      0.668 0.000 0.692 0.000 0.308 0.000
#&gt; GSM28697     2  0.2036      0.891 0.000 0.920 0.000 0.024 0.056
#&gt; GSM28698     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11238     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11242     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28719     4  0.3427      0.608 0.000 0.192 0.000 0.796 0.012
#&gt; GSM28708     4  0.0000      0.756 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28722     2  0.2450      0.888 0.000 0.896 0.000 0.028 0.076
#&gt; GSM11232     2  0.4291      0.335 0.000 0.536 0.000 0.464 0.000
#&gt; GSM28709     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11226     4  0.3109      0.811 0.000 0.000 0.200 0.800 0.000
#&gt; GSM11239     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11225     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11220     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28701     2  0.2423      0.889 0.000 0.896 0.000 0.024 0.080
#&gt; GSM28721     4  0.3109      0.811 0.000 0.000 0.200 0.800 0.000
#&gt; GSM28713     2  0.1732      0.892 0.000 0.920 0.000 0.000 0.080
#&gt; GSM28716     2  0.2891      0.736 0.176 0.824 0.000 0.000 0.000
#&gt; GSM11221     2  0.1732      0.892 0.000 0.920 0.000 0.000 0.080
#&gt; GSM28717     2  0.0404      0.872 0.000 0.988 0.000 0.000 0.012
#&gt; GSM11223     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     4  0.3109      0.811 0.000 0.000 0.200 0.800 0.000
#&gt; GSM11219     2  0.3837      0.690 0.000 0.692 0.000 0.000 0.308
#&gt; GSM11236     4  0.3036      0.774 0.000 0.064 0.028 0.880 0.028
#&gt; GSM28702     4  0.3305      0.797 0.000 0.000 0.224 0.776 0.000
#&gt; GSM28705     4  0.4226      0.790 0.000 0.084 0.140 0.776 0.000
#&gt; GSM11230     5  0.0404      0.936 0.000 0.000 0.000 0.012 0.988
#&gt; GSM28704     2  0.1732      0.892 0.000 0.920 0.000 0.000 0.080
#&gt; GSM28700     2  0.0000      0.873 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11224     2  0.1732      0.892 0.000 0.920 0.000 0.000 0.080
</code></pre>

<script>
$('#tab-CV-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-4-a').click(function(){
  $('#tab-CV-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-5'>
<p><a id='tab-CV-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28710     2  0.1610      0.885 0.000 0.916 0.000 0.000 0.084 0.000
#&gt; GSM28711     6  0.4255      0.598 0.000 0.068 0.000 0.000 0.224 0.708
#&gt; GSM28712     2  0.2697      0.825 0.000 0.812 0.000 0.000 0.188 0.000
#&gt; GSM11222     6  0.2300      0.795 0.000 0.000 0.144 0.000 0.000 0.856
#&gt; GSM28720     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     5  0.0146      0.924 0.000 0.004 0.000 0.000 0.996 0.000
#&gt; GSM28714     5  0.0146      0.924 0.000 0.004 0.000 0.000 0.996 0.000
#&gt; GSM11216     3  0.0717      0.981 0.000 0.000 0.976 0.008 0.000 0.016
#&gt; GSM28715     5  0.0146      0.924 0.000 0.004 0.000 0.000 0.996 0.000
#&gt; GSM11234     2  0.0000      0.864 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28699     2  0.0146      0.861 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM11233     5  0.2562      0.792 0.000 0.172 0.000 0.000 0.828 0.000
#&gt; GSM28718     5  0.0146      0.924 0.000 0.004 0.000 0.000 0.996 0.000
#&gt; GSM11231     5  0.2854      0.743 0.000 0.000 0.000 0.208 0.792 0.000
#&gt; GSM11237     5  0.1588      0.873 0.000 0.004 0.000 0.072 0.924 0.000
#&gt; GSM11228     4  0.2404      0.881 0.000 0.036 0.000 0.884 0.000 0.080
#&gt; GSM28697     2  0.2052      0.881 0.000 0.912 0.000 0.028 0.056 0.004
#&gt; GSM28698     3  0.0260      0.987 0.000 0.000 0.992 0.000 0.000 0.008
#&gt; GSM11238     3  0.0458      0.986 0.000 0.000 0.984 0.000 0.000 0.016
#&gt; GSM11242     3  0.0260      0.987 0.000 0.000 0.992 0.000 0.000 0.008
#&gt; GSM28719     4  0.0260      0.943 0.000 0.000 0.000 0.992 0.000 0.008
#&gt; GSM28708     4  0.0260      0.943 0.000 0.000 0.000 0.992 0.000 0.008
#&gt; GSM28722     2  0.2122      0.884 0.000 0.900 0.000 0.008 0.084 0.008
#&gt; GSM11232     2  0.5654      0.331 0.000 0.524 0.000 0.192 0.000 0.284
#&gt; GSM28709     3  0.0717      0.981 0.000 0.000 0.976 0.008 0.000 0.016
#&gt; GSM11226     6  0.0363      0.848 0.000 0.000 0.012 0.000 0.000 0.988
#&gt; GSM11239     3  0.0260      0.987 0.000 0.000 0.992 0.000 0.000 0.008
#&gt; GSM11225     3  0.0260      0.987 0.000 0.000 0.992 0.000 0.000 0.008
#&gt; GSM11220     3  0.0717      0.981 0.000 0.000 0.976 0.008 0.000 0.016
#&gt; GSM28701     2  0.3757      0.811 0.000 0.780 0.000 0.136 0.084 0.000
#&gt; GSM28721     6  0.0363      0.848 0.000 0.000 0.012 0.000 0.000 0.988
#&gt; GSM28713     2  0.1753      0.885 0.000 0.912 0.000 0.000 0.084 0.004
#&gt; GSM28716     2  0.2664      0.699 0.184 0.816 0.000 0.000 0.000 0.000
#&gt; GSM11221     2  0.1753      0.885 0.000 0.912 0.000 0.000 0.084 0.004
#&gt; GSM28717     2  0.0363      0.860 0.000 0.988 0.000 0.000 0.012 0.000
#&gt; GSM11223     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     6  0.0363      0.848 0.000 0.000 0.012 0.000 0.000 0.988
#&gt; GSM11219     2  0.3499      0.682 0.000 0.680 0.000 0.000 0.320 0.000
#&gt; GSM11236     6  0.4142      0.718 0.000 0.076 0.004 0.136 0.012 0.772
#&gt; GSM28702     6  0.2491      0.779 0.000 0.000 0.164 0.000 0.000 0.836
#&gt; GSM28705     6  0.1411      0.815 0.000 0.060 0.004 0.000 0.000 0.936
#&gt; GSM11230     5  0.0291      0.923 0.000 0.004 0.000 0.000 0.992 0.004
#&gt; GSM28704     2  0.1866      0.885 0.000 0.908 0.000 0.000 0.084 0.008
#&gt; GSM28700     2  0.0000      0.864 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11224     2  0.1610      0.885 0.000 0.916 0.000 0.000 0.084 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-5-a').click(function(){
  $('#tab-CV-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-membership-heatmap'>
<ul>
<li><a href='#tab-CV-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-pam-signature_compare](figure_cola/CV-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-pam-dimension-reduction'>
<ul>
<li><a href='#tab-CV-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-pam-collect-classes](figure_cola/CV-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n tissue(p) k
#> CV:pam 47     0.391 2
#> CV:pam 52     0.372 3
#> CV:pam 48     0.346 4
#> CV:pam 52     0.334 5
#> CV:pam 53     0.320 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:mclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "mclust"]
# you can also extract it by
# res = res_list["CV:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21452 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-mclust-collect-plots](figure_cola/CV-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-mclust-select-partition-number](figure_cola/CV-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.527           0.896       0.928         0.3659 0.648   0.648
#> 3 3 0.791           0.902       0.947         0.5383 0.778   0.670
#> 4 4 0.710           0.795       0.855         0.2966 0.767   0.519
#> 5 5 0.748           0.799       0.852         0.0704 0.895   0.627
#> 6 6 0.751           0.510       0.740         0.0502 0.886   0.529
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-classes'>
<ul>
<li><a href='#tab-CV-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-mclust-get-classes-1'>
<p><a id='tab-CV-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28710     2  0.6438     0.8939 0.164 0.836
#&gt; GSM28711     2  0.5519     0.9065 0.128 0.872
#&gt; GSM28712     2  0.6438     0.8939 0.164 0.836
#&gt; GSM11222     2  0.0376     0.9085 0.004 0.996
#&gt; GSM28720     1  0.0000     0.9529 1.000 0.000
#&gt; GSM11217     1  0.0000     0.9529 1.000 0.000
#&gt; GSM28723     1  0.0000     0.9529 1.000 0.000
#&gt; GSM11241     1  0.0000     0.9529 1.000 0.000
#&gt; GSM28703     1  0.0000     0.9529 1.000 0.000
#&gt; GSM11227     1  0.0000     0.9529 1.000 0.000
#&gt; GSM28706     1  0.0000     0.9529 1.000 0.000
#&gt; GSM11229     1  0.0000     0.9529 1.000 0.000
#&gt; GSM11235     1  0.0000     0.9529 1.000 0.000
#&gt; GSM28707     1  0.0000     0.9529 1.000 0.000
#&gt; GSM11240     2  0.6438     0.8939 0.164 0.836
#&gt; GSM28714     2  0.6438     0.8939 0.164 0.836
#&gt; GSM11216     2  0.0376     0.9085 0.004 0.996
#&gt; GSM28715     2  0.6438     0.8939 0.164 0.836
#&gt; GSM11234     2  0.5294     0.9074 0.120 0.880
#&gt; GSM28699     2  0.6438     0.8939 0.164 0.836
#&gt; GSM11233     2  0.6438     0.8939 0.164 0.836
#&gt; GSM28718     2  0.6438     0.8939 0.164 0.836
#&gt; GSM11231     2  0.5737     0.9033 0.136 0.864
#&gt; GSM11237     2  0.6438     0.8939 0.164 0.836
#&gt; GSM11228     2  0.0000     0.9092 0.000 1.000
#&gt; GSM28697     2  0.0000     0.9092 0.000 1.000
#&gt; GSM28698     2  0.0376     0.9085 0.004 0.996
#&gt; GSM11238     2  0.0376     0.9085 0.004 0.996
#&gt; GSM11242     2  0.0376     0.9085 0.004 0.996
#&gt; GSM28719     2  0.0000     0.9092 0.000 1.000
#&gt; GSM28708     2  0.0000     0.9092 0.000 1.000
#&gt; GSM28722     2  0.3879     0.9102 0.076 0.924
#&gt; GSM11232     2  0.0000     0.9092 0.000 1.000
#&gt; GSM28709     2  0.0376     0.9085 0.004 0.996
#&gt; GSM11226     2  0.0000     0.9092 0.000 1.000
#&gt; GSM11239     2  0.0376     0.9085 0.004 0.996
#&gt; GSM11225     2  0.0376     0.9085 0.004 0.996
#&gt; GSM11220     2  0.0376     0.9085 0.004 0.996
#&gt; GSM28701     2  0.6247     0.8973 0.156 0.844
#&gt; GSM28721     2  0.0000     0.9092 0.000 1.000
#&gt; GSM28713     2  0.5408     0.9070 0.124 0.876
#&gt; GSM28716     1  0.9909    -0.0341 0.556 0.444
#&gt; GSM11221     2  0.6438     0.8939 0.164 0.836
#&gt; GSM28717     2  0.6438     0.8939 0.164 0.836
#&gt; GSM11223     1  0.0000     0.9529 1.000 0.000
#&gt; GSM11218     2  0.0000     0.9092 0.000 1.000
#&gt; GSM11219     2  0.6438     0.8939 0.164 0.836
#&gt; GSM11236     2  0.0000     0.9092 0.000 1.000
#&gt; GSM28702     2  0.0376     0.9085 0.004 0.996
#&gt; GSM28705     2  0.0000     0.9092 0.000 1.000
#&gt; GSM11230     2  0.6438     0.8939 0.164 0.836
#&gt; GSM28704     2  0.5737     0.9044 0.136 0.864
#&gt; GSM28700     2  0.5519     0.9064 0.128 0.872
#&gt; GSM11224     2  0.5408     0.9070 0.124 0.876
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-1-a').click(function(){
  $('#tab-CV-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-2'>
<p><a id='tab-CV-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28710     2  0.0000      0.912 0.000 1.000 0.000
#&gt; GSM28711     2  0.0000      0.912 0.000 1.000 0.000
#&gt; GSM28712     2  0.0000      0.912 0.000 1.000 0.000
#&gt; GSM11222     2  0.6126      0.494 0.000 0.600 0.400
#&gt; GSM28720     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11217     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28723     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11241     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28703     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11227     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28706     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11229     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11235     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28707     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11240     2  0.0000      0.912 0.000 1.000 0.000
#&gt; GSM28714     2  0.0000      0.912 0.000 1.000 0.000
#&gt; GSM11216     3  0.0000      0.994 0.000 0.000 1.000
#&gt; GSM28715     2  0.0000      0.912 0.000 1.000 0.000
#&gt; GSM11234     2  0.0424      0.910 0.008 0.992 0.000
#&gt; GSM28699     2  0.0424      0.910 0.008 0.992 0.000
#&gt; GSM11233     2  0.0000      0.912 0.000 1.000 0.000
#&gt; GSM28718     2  0.0000      0.912 0.000 1.000 0.000
#&gt; GSM11231     2  0.0892      0.907 0.000 0.980 0.020
#&gt; GSM11237     2  0.0000      0.912 0.000 1.000 0.000
#&gt; GSM11228     2  0.3038      0.869 0.000 0.896 0.104
#&gt; GSM28697     2  0.2878      0.873 0.000 0.904 0.096
#&gt; GSM28698     3  0.0000      0.994 0.000 0.000 1.000
#&gt; GSM11238     3  0.0000      0.994 0.000 0.000 1.000
#&gt; GSM11242     3  0.0000      0.994 0.000 0.000 1.000
#&gt; GSM28719     2  0.2878      0.873 0.000 0.904 0.096
#&gt; GSM28708     2  0.4887      0.771 0.000 0.772 0.228
#&gt; GSM28722     2  0.0000      0.912 0.000 1.000 0.000
#&gt; GSM11232     2  0.2959      0.871 0.000 0.900 0.100
#&gt; GSM28709     3  0.0000      0.994 0.000 0.000 1.000
#&gt; GSM11226     2  0.4605      0.792 0.000 0.796 0.204
#&gt; GSM11239     3  0.0000      0.994 0.000 0.000 1.000
#&gt; GSM11225     3  0.0000      0.994 0.000 0.000 1.000
#&gt; GSM11220     3  0.1163      0.958 0.000 0.028 0.972
#&gt; GSM28701     2  0.0237      0.912 0.000 0.996 0.004
#&gt; GSM28721     2  0.5178      0.735 0.000 0.744 0.256
#&gt; GSM28713     2  0.0424      0.910 0.008 0.992 0.000
#&gt; GSM28716     2  0.5621      0.532 0.308 0.692 0.000
#&gt; GSM11221     2  0.0000      0.912 0.000 1.000 0.000
#&gt; GSM28717     2  0.0424      0.910 0.008 0.992 0.000
#&gt; GSM11223     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11218     2  0.5016      0.755 0.000 0.760 0.240
#&gt; GSM11219     2  0.0000      0.912 0.000 1.000 0.000
#&gt; GSM11236     2  0.4452      0.806 0.000 0.808 0.192
#&gt; GSM28702     2  0.6008      0.553 0.000 0.628 0.372
#&gt; GSM28705     2  0.4452      0.804 0.000 0.808 0.192
#&gt; GSM11230     2  0.0000      0.912 0.000 1.000 0.000
#&gt; GSM28704     2  0.0000      0.912 0.000 1.000 0.000
#&gt; GSM28700     2  0.0424      0.910 0.008 0.992 0.000
#&gt; GSM11224     2  0.0424      0.910 0.008 0.992 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-2-a').click(function(){
  $('#tab-CV-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-3'>
<p><a id='tab-CV-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette   p1    p2    p3    p4
#&gt; GSM28710     2  0.3837      0.762 0.00 0.776 0.000 0.224
#&gt; GSM28711     2  0.4103      0.737 0.00 0.744 0.000 0.256
#&gt; GSM28712     2  0.1389      0.810 0.00 0.952 0.000 0.048
#&gt; GSM11222     3  0.4624      0.507 0.00 0.000 0.660 0.340
#&gt; GSM28720     1  0.0000      1.000 1.00 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      1.000 1.00 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      1.000 1.00 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      1.000 1.00 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      1.000 1.00 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      1.000 1.00 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      1.000 1.00 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      1.000 1.00 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      1.000 1.00 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      1.000 1.00 0.000 0.000 0.000
#&gt; GSM11240     2  0.1637      0.788 0.00 0.940 0.000 0.060
#&gt; GSM28714     2  0.1637      0.788 0.00 0.940 0.000 0.060
#&gt; GSM11216     3  0.0000      0.913 0.00 0.000 1.000 0.000
#&gt; GSM28715     2  0.3266      0.804 0.00 0.832 0.000 0.168
#&gt; GSM11234     2  0.4998      0.208 0.00 0.512 0.000 0.488
#&gt; GSM28699     2  0.1792      0.816 0.00 0.932 0.000 0.068
#&gt; GSM11233     2  0.1867      0.793 0.00 0.928 0.000 0.072
#&gt; GSM28718     2  0.1637      0.788 0.00 0.940 0.000 0.060
#&gt; GSM11231     2  0.4103      0.739 0.00 0.744 0.000 0.256
#&gt; GSM11237     2  0.2081      0.776 0.00 0.916 0.000 0.084
#&gt; GSM11228     4  0.2124      0.830 0.00 0.028 0.040 0.932
#&gt; GSM28697     4  0.2345      0.784 0.00 0.100 0.000 0.900
#&gt; GSM28698     3  0.0000      0.913 0.00 0.000 1.000 0.000
#&gt; GSM11238     3  0.0000      0.913 0.00 0.000 1.000 0.000
#&gt; GSM11242     3  0.0000      0.913 0.00 0.000 1.000 0.000
#&gt; GSM28719     4  0.0921      0.810 0.00 0.028 0.000 0.972
#&gt; GSM28708     4  0.2011      0.816 0.00 0.000 0.080 0.920
#&gt; GSM28722     4  0.4564      0.407 0.00 0.328 0.000 0.672
#&gt; GSM11232     4  0.1389      0.813 0.00 0.048 0.000 0.952
#&gt; GSM28709     3  0.0188      0.910 0.00 0.000 0.996 0.004
#&gt; GSM11226     4  0.2921      0.804 0.00 0.000 0.140 0.860
#&gt; GSM11239     3  0.0000      0.913 0.00 0.000 1.000 0.000
#&gt; GSM11225     3  0.0000      0.913 0.00 0.000 1.000 0.000
#&gt; GSM11220     3  0.0336      0.909 0.00 0.000 0.992 0.008
#&gt; GSM28701     4  0.4977     -0.199 0.00 0.460 0.000 0.540
#&gt; GSM28721     4  0.2921      0.804 0.00 0.000 0.140 0.860
#&gt; GSM28713     2  0.4250      0.712 0.00 0.724 0.000 0.276
#&gt; GSM28716     2  0.5855      0.706 0.16 0.704 0.000 0.136
#&gt; GSM11221     2  0.4164      0.727 0.00 0.736 0.000 0.264
#&gt; GSM28717     2  0.1792      0.816 0.00 0.932 0.000 0.068
#&gt; GSM11223     1  0.0000      1.000 1.00 0.000 0.000 0.000
#&gt; GSM11218     4  0.2921      0.804 0.00 0.000 0.140 0.860
#&gt; GSM11219     2  0.2345      0.819 0.00 0.900 0.000 0.100
#&gt; GSM11236     4  0.2329      0.825 0.00 0.012 0.072 0.916
#&gt; GSM28702     3  0.4661      0.489 0.00 0.000 0.652 0.348
#&gt; GSM28705     4  0.3249      0.807 0.00 0.008 0.140 0.852
#&gt; GSM11230     2  0.2868      0.814 0.00 0.864 0.000 0.136
#&gt; GSM28704     2  0.4967      0.330 0.00 0.548 0.000 0.452
#&gt; GSM28700     2  0.2921      0.812 0.00 0.860 0.000 0.140
#&gt; GSM11224     2  0.3074      0.808 0.00 0.848 0.000 0.152
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-3-a').click(function(){
  $('#tab-CV-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-4'>
<p><a id='tab-CV-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette   p1    p2    p3    p4    p5
#&gt; GSM28710     2  0.0162      0.808 0.00 0.996 0.000 0.000 0.004
#&gt; GSM28711     2  0.0451      0.807 0.00 0.988 0.000 0.008 0.004
#&gt; GSM28712     5  0.4114      0.709 0.00 0.376 0.000 0.000 0.624
#&gt; GSM11222     3  0.4734      0.741 0.00 0.000 0.704 0.232 0.064
#&gt; GSM28720     1  0.0000      1.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      1.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      1.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      1.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      1.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      1.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      1.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      1.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      1.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      1.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11240     5  0.2516      0.807 0.00 0.140 0.000 0.000 0.860
#&gt; GSM28714     5  0.2424      0.807 0.00 0.132 0.000 0.000 0.868
#&gt; GSM11216     3  0.0000      0.940 0.00 0.000 1.000 0.000 0.000
#&gt; GSM28715     2  0.4467      0.377 0.00 0.640 0.000 0.016 0.344
#&gt; GSM11234     2  0.1082      0.796 0.00 0.964 0.000 0.028 0.008
#&gt; GSM28699     5  0.4278      0.591 0.00 0.452 0.000 0.000 0.548
#&gt; GSM11233     5  0.3942      0.771 0.00 0.260 0.000 0.012 0.728
#&gt; GSM28718     5  0.2424      0.807 0.00 0.132 0.000 0.000 0.868
#&gt; GSM11231     2  0.4062      0.651 0.00 0.764 0.000 0.040 0.196
#&gt; GSM11237     5  0.2873      0.795 0.00 0.128 0.000 0.016 0.856
#&gt; GSM11228     4  0.5223      0.777 0.00 0.220 0.000 0.672 0.108
#&gt; GSM28697     4  0.5509      0.634 0.00 0.360 0.000 0.564 0.076
#&gt; GSM28698     3  0.0000      0.940 0.00 0.000 1.000 0.000 0.000
#&gt; GSM11238     3  0.0000      0.940 0.00 0.000 1.000 0.000 0.000
#&gt; GSM11242     3  0.0000      0.940 0.00 0.000 1.000 0.000 0.000
#&gt; GSM28719     4  0.6040      0.716 0.00 0.284 0.000 0.560 0.156
#&gt; GSM28708     4  0.6271      0.773 0.00 0.196 0.060 0.640 0.104
#&gt; GSM28722     2  0.1894      0.750 0.00 0.920 0.000 0.072 0.008
#&gt; GSM11232     4  0.5873      0.706 0.00 0.312 0.000 0.564 0.124
#&gt; GSM28709     3  0.0000      0.940 0.00 0.000 1.000 0.000 0.000
#&gt; GSM11226     4  0.3262      0.746 0.00 0.124 0.036 0.840 0.000
#&gt; GSM11239     3  0.0000      0.940 0.00 0.000 1.000 0.000 0.000
#&gt; GSM11225     3  0.0000      0.940 0.00 0.000 1.000 0.000 0.000
#&gt; GSM11220     3  0.0963      0.915 0.00 0.000 0.964 0.000 0.036
#&gt; GSM28701     2  0.5112      0.364 0.00 0.664 0.000 0.256 0.080
#&gt; GSM28721     4  0.1750      0.679 0.00 0.028 0.036 0.936 0.000
#&gt; GSM28713     2  0.0162      0.808 0.00 0.996 0.000 0.000 0.004
#&gt; GSM28716     2  0.3086      0.619 0.18 0.816 0.000 0.000 0.004
#&gt; GSM11221     2  0.0162      0.808 0.00 0.996 0.000 0.000 0.004
#&gt; GSM28717     5  0.4210      0.664 0.00 0.412 0.000 0.000 0.588
#&gt; GSM11223     1  0.0000      1.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11218     4  0.1750      0.679 0.00 0.028 0.036 0.936 0.000
#&gt; GSM11219     2  0.4150      0.285 0.00 0.612 0.000 0.000 0.388
#&gt; GSM11236     4  0.6145      0.750 0.00 0.264 0.040 0.612 0.084
#&gt; GSM28702     3  0.4734      0.741 0.00 0.000 0.704 0.232 0.064
#&gt; GSM28705     4  0.4264      0.767 0.00 0.196 0.036 0.760 0.008
#&gt; GSM11230     2  0.3684      0.490 0.00 0.720 0.000 0.000 0.280
#&gt; GSM28704     2  0.0807      0.804 0.00 0.976 0.000 0.012 0.012
#&gt; GSM28700     2  0.0162      0.808 0.00 0.996 0.000 0.000 0.004
#&gt; GSM11224     2  0.0162      0.808 0.00 0.996 0.000 0.000 0.004
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-4-a').click(function(){
  $('#tab-CV-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-5'>
<p><a id='tab-CV-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette   p1    p2    p3    p4    p5    p6
#&gt; GSM28710     2  0.5737     0.2454 0.00 0.440 0.000 0.392 0.168 0.000
#&gt; GSM28711     4  0.5422    -0.3173 0.00 0.436 0.000 0.448 0.116 0.000
#&gt; GSM28712     2  0.3470     0.1378 0.00 0.796 0.000 0.152 0.052 0.000
#&gt; GSM11222     3  0.4865     0.6367 0.00 0.000 0.652 0.004 0.248 0.096
#&gt; GSM28720     1  0.0000     1.0000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000     1.0000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000     1.0000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000     1.0000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000     1.0000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000     1.0000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000     1.0000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000     1.0000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000     1.0000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000     1.0000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.2618    -0.1175 0.00 0.860 0.000 0.024 0.116 0.000
#&gt; GSM28714     2  0.3756    -0.4495 0.00 0.600 0.000 0.000 0.400 0.000
#&gt; GSM11216     3  0.0000     0.9161 0.00 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28715     2  0.3499     0.3644 0.00 0.680 0.000 0.320 0.000 0.000
#&gt; GSM11234     4  0.3776     0.4191 0.00 0.052 0.000 0.760 0.188 0.000
#&gt; GSM28699     5  0.4996     0.7379 0.00 0.200 0.000 0.156 0.644 0.000
#&gt; GSM11233     5  0.5586     0.4999 0.00 0.420 0.000 0.140 0.440 0.000
#&gt; GSM28718     2  0.3756    -0.4495 0.00 0.600 0.000 0.000 0.400 0.000
#&gt; GSM11231     2  0.4076     0.3179 0.00 0.564 0.000 0.428 0.004 0.004
#&gt; GSM11237     2  0.4482    -0.4668 0.00 0.580 0.000 0.036 0.384 0.000
#&gt; GSM11228     4  0.4930    -0.2471 0.00 0.004 0.000 0.560 0.060 0.376
#&gt; GSM28697     4  0.3296     0.3342 0.00 0.008 0.000 0.828 0.048 0.116
#&gt; GSM28698     3  0.0000     0.9161 0.00 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11238     3  0.0000     0.9161 0.00 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11242     3  0.0000     0.9161 0.00 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28719     4  0.5557    -0.0913 0.00 0.068 0.000 0.572 0.040 0.320
#&gt; GSM28708     6  0.3821     0.6834 0.00 0.000 0.020 0.188 0.024 0.768
#&gt; GSM28722     4  0.3419     0.4313 0.00 0.040 0.000 0.804 0.152 0.004
#&gt; GSM11232     4  0.4271     0.1151 0.00 0.008 0.000 0.712 0.048 0.232
#&gt; GSM28709     3  0.0000     0.9161 0.00 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11226     6  0.0260     0.7717 0.00 0.000 0.000 0.008 0.000 0.992
#&gt; GSM11239     3  0.0000     0.9161 0.00 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11225     3  0.0000     0.9161 0.00 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11220     3  0.0508     0.9046 0.00 0.000 0.984 0.012 0.004 0.000
#&gt; GSM28701     4  0.1699     0.4587 0.00 0.032 0.000 0.936 0.016 0.016
#&gt; GSM28721     6  0.0260     0.7717 0.00 0.000 0.000 0.008 0.000 0.992
#&gt; GSM28713     4  0.5543    -0.1053 0.00 0.320 0.000 0.524 0.156 0.000
#&gt; GSM28716     4  0.6778     0.0627 0.36 0.048 0.000 0.372 0.220 0.000
#&gt; GSM11221     2  0.5686     0.2635 0.00 0.456 0.000 0.384 0.160 0.000
#&gt; GSM28717     5  0.5008     0.7470 0.00 0.212 0.000 0.148 0.640 0.000
#&gt; GSM11223     1  0.0000     1.0000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     6  0.0260     0.7717 0.00 0.000 0.000 0.008 0.000 0.992
#&gt; GSM11219     2  0.2527     0.2816 0.00 0.832 0.000 0.168 0.000 0.000
#&gt; GSM11236     6  0.4438     0.4570 0.00 0.004 0.008 0.360 0.016 0.612
#&gt; GSM28702     3  0.5794     0.4469 0.00 0.000 0.528 0.004 0.248 0.220
#&gt; GSM28705     6  0.4534     0.3009 0.00 0.000 0.000 0.380 0.040 0.580
#&gt; GSM11230     2  0.5571     0.3205 0.00 0.532 0.000 0.324 0.140 0.004
#&gt; GSM28704     4  0.3748     0.4102 0.00 0.064 0.000 0.784 0.148 0.004
#&gt; GSM28700     2  0.5791     0.2515 0.00 0.440 0.000 0.380 0.180 0.000
#&gt; GSM11224     2  0.5673     0.2543 0.00 0.448 0.000 0.396 0.156 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-5-a').click(function(){
  $('#tab-CV-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-mclust-signature_compare](figure_cola/CV-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-mclust-collect-classes](figure_cola/CV-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) k
#> CV:mclust 53     0.397 2
#> CV:mclust 53     0.373 3
#> CV:mclust 49     0.348 4
#> CV:mclust 50     0.434 5
#> CV:mclust 26     0.384 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:NMF*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "NMF"]
# you can also extract it by
# res = res_list["CV:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21452 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-NMF-collect-plots](figure_cola/CV-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-NMF-select-partition-number](figure_cola/CV-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.520           0.791       0.906         0.4667 0.535   0.535
#> 3 3 0.998           0.960       0.983         0.3633 0.701   0.501
#> 4 4 0.916           0.894       0.949         0.1776 0.841   0.588
#> 5 5 0.852           0.809       0.897         0.0548 0.944   0.786
#> 6 6 0.836           0.733       0.861         0.0435 0.945   0.753
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
#> attr(,"optional")
#> [1] 3
```

There is also optional best $k$ = 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-classes'>
<ul>
<li><a href='#tab-CV-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-NMF-get-classes-1'>
<p><a id='tab-CV-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28710     2  0.0376      0.886 0.004 0.996
#&gt; GSM28711     2  0.2423      0.874 0.040 0.960
#&gt; GSM28712     2  0.0376      0.886 0.004 0.996
#&gt; GSM11222     1  0.0000      0.884 1.000 0.000
#&gt; GSM28720     2  0.5519      0.839 0.128 0.872
#&gt; GSM11217     2  0.5519      0.839 0.128 0.872
#&gt; GSM28723     2  0.5519      0.839 0.128 0.872
#&gt; GSM11241     2  0.5519      0.839 0.128 0.872
#&gt; GSM28703     2  0.5519      0.839 0.128 0.872
#&gt; GSM11227     2  0.5519      0.839 0.128 0.872
#&gt; GSM28706     2  0.4161      0.861 0.084 0.916
#&gt; GSM11229     2  0.5519      0.839 0.128 0.872
#&gt; GSM11235     2  0.5519      0.839 0.128 0.872
#&gt; GSM28707     2  0.5519      0.839 0.128 0.872
#&gt; GSM11240     2  0.1633      0.882 0.024 0.976
#&gt; GSM28714     2  0.1843      0.880 0.028 0.972
#&gt; GSM11216     1  0.0000      0.884 1.000 0.000
#&gt; GSM28715     2  0.9710      0.248 0.400 0.600
#&gt; GSM11234     2  0.0376      0.886 0.004 0.996
#&gt; GSM28699     2  0.0000      0.885 0.000 1.000
#&gt; GSM11233     2  0.0376      0.886 0.004 0.996
#&gt; GSM28718     2  0.1633      0.882 0.024 0.976
#&gt; GSM11231     2  0.9460      0.358 0.364 0.636
#&gt; GSM11237     2  0.1184      0.884 0.016 0.984
#&gt; GSM11228     1  0.9996      0.135 0.512 0.488
#&gt; GSM28697     2  0.7056      0.714 0.192 0.808
#&gt; GSM28698     1  0.0000      0.884 1.000 0.000
#&gt; GSM11238     1  0.0000      0.884 1.000 0.000
#&gt; GSM11242     1  0.0000      0.884 1.000 0.000
#&gt; GSM28719     1  0.8608      0.626 0.716 0.284
#&gt; GSM28708     1  0.3431      0.847 0.936 0.064
#&gt; GSM28722     2  0.8267      0.599 0.260 0.740
#&gt; GSM11232     2  0.9732      0.234 0.404 0.596
#&gt; GSM28709     1  0.0000      0.884 1.000 0.000
#&gt; GSM11226     1  0.4431      0.825 0.908 0.092
#&gt; GSM11239     1  0.0000      0.884 1.000 0.000
#&gt; GSM11225     1  0.0000      0.884 1.000 0.000
#&gt; GSM11220     1  0.0000      0.884 1.000 0.000
#&gt; GSM28701     2  0.0672      0.886 0.008 0.992
#&gt; GSM28721     1  0.0376      0.882 0.996 0.004
#&gt; GSM28713     2  0.0376      0.886 0.004 0.996
#&gt; GSM28716     2  0.0000      0.885 0.000 1.000
#&gt; GSM11221     2  0.0376      0.886 0.004 0.996
#&gt; GSM28717     2  0.0376      0.886 0.004 0.996
#&gt; GSM11223     2  0.4562      0.856 0.096 0.904
#&gt; GSM11218     1  0.0000      0.884 1.000 0.000
#&gt; GSM11219     2  0.1414      0.883 0.020 0.980
#&gt; GSM11236     1  0.2948      0.850 0.948 0.052
#&gt; GSM28702     1  0.0000      0.884 1.000 0.000
#&gt; GSM28705     1  0.9922      0.274 0.552 0.448
#&gt; GSM11230     1  0.9850      0.333 0.572 0.428
#&gt; GSM28704     2  0.3274      0.861 0.060 0.940
#&gt; GSM28700     2  0.0376      0.886 0.004 0.996
#&gt; GSM11224     2  0.0376      0.886 0.004 0.996
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-1-a').click(function(){
  $('#tab-CV-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-2'>
<p><a id='tab-CV-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28710     2  0.0000      0.977 0.000 1.000 0.000
#&gt; GSM28711     2  0.0000      0.977 0.000 1.000 0.000
#&gt; GSM28712     2  0.0000      0.977 0.000 1.000 0.000
#&gt; GSM11222     3  0.0000      0.972 0.000 0.000 1.000
#&gt; GSM28720     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM11217     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM28723     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM11241     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM28703     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM11227     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM28706     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM11229     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM11235     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM28707     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM11240     2  0.0000      0.977 0.000 1.000 0.000
#&gt; GSM28714     2  0.0000      0.977 0.000 1.000 0.000
#&gt; GSM11216     3  0.0000      0.972 0.000 0.000 1.000
#&gt; GSM28715     2  0.0000      0.977 0.000 1.000 0.000
#&gt; GSM11234     2  0.0000      0.977 0.000 1.000 0.000
#&gt; GSM28699     2  0.2165      0.919 0.064 0.936 0.000
#&gt; GSM11233     2  0.0000      0.977 0.000 1.000 0.000
#&gt; GSM28718     2  0.0000      0.977 0.000 1.000 0.000
#&gt; GSM11231     2  0.0000      0.977 0.000 1.000 0.000
#&gt; GSM11237     2  0.0000      0.977 0.000 1.000 0.000
#&gt; GSM11228     2  0.4555      0.753 0.000 0.800 0.200
#&gt; GSM28697     2  0.0000      0.977 0.000 1.000 0.000
#&gt; GSM28698     3  0.0000      0.972 0.000 0.000 1.000
#&gt; GSM11238     3  0.0000      0.972 0.000 0.000 1.000
#&gt; GSM11242     3  0.0000      0.972 0.000 0.000 1.000
#&gt; GSM28719     2  0.0000      0.977 0.000 1.000 0.000
#&gt; GSM28708     3  0.2796      0.877 0.000 0.092 0.908
#&gt; GSM28722     2  0.0000      0.977 0.000 1.000 0.000
#&gt; GSM11232     2  0.1964      0.928 0.000 0.944 0.056
#&gt; GSM28709     3  0.0000      0.972 0.000 0.000 1.000
#&gt; GSM11226     3  0.4750      0.721 0.000 0.216 0.784
#&gt; GSM11239     3  0.0000      0.972 0.000 0.000 1.000
#&gt; GSM11225     3  0.0000      0.972 0.000 0.000 1.000
#&gt; GSM11220     3  0.0237      0.969 0.004 0.000 0.996
#&gt; GSM28701     2  0.0000      0.977 0.000 1.000 0.000
#&gt; GSM28721     3  0.0000      0.972 0.000 0.000 1.000
#&gt; GSM28713     2  0.0000      0.977 0.000 1.000 0.000
#&gt; GSM28716     1  0.1753      0.941 0.952 0.048 0.000
#&gt; GSM11221     2  0.0000      0.977 0.000 1.000 0.000
#&gt; GSM28717     2  0.0237      0.974 0.004 0.996 0.000
#&gt; GSM11223     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM11218     3  0.0000      0.972 0.000 0.000 1.000
#&gt; GSM11219     2  0.0000      0.977 0.000 1.000 0.000
#&gt; GSM11236     3  0.0000      0.972 0.000 0.000 1.000
#&gt; GSM28702     3  0.0000      0.972 0.000 0.000 1.000
#&gt; GSM28705     2  0.5138      0.669 0.000 0.748 0.252
#&gt; GSM11230     2  0.0000      0.977 0.000 1.000 0.000
#&gt; GSM28704     2  0.0000      0.977 0.000 1.000 0.000
#&gt; GSM28700     2  0.0000      0.977 0.000 1.000 0.000
#&gt; GSM11224     2  0.0000      0.977 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-2-a').click(function(){
  $('#tab-CV-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-3'>
<p><a id='tab-CV-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28710     2  0.0592      0.916 0.000 0.984 0.000 0.016
#&gt; GSM28711     2  0.1211      0.911 0.000 0.960 0.000 0.040
#&gt; GSM28712     2  0.0592      0.916 0.000 0.984 0.000 0.016
#&gt; GSM11222     3  0.0188      0.987 0.000 0.000 0.996 0.004
#&gt; GSM28720     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.0592      0.916 0.000 0.984 0.000 0.016
#&gt; GSM28714     2  0.0469      0.911 0.000 0.988 0.000 0.012
#&gt; GSM11216     3  0.0188      0.987 0.000 0.000 0.996 0.004
#&gt; GSM28715     2  0.1118      0.913 0.000 0.964 0.000 0.036
#&gt; GSM11234     4  0.1302      0.890 0.000 0.044 0.000 0.956
#&gt; GSM28699     2  0.0000      0.913 0.000 1.000 0.000 0.000
#&gt; GSM11233     2  0.0188      0.911 0.000 0.996 0.000 0.004
#&gt; GSM28718     2  0.0188      0.914 0.000 0.996 0.000 0.004
#&gt; GSM11231     2  0.3907      0.707 0.000 0.768 0.000 0.232
#&gt; GSM11237     2  0.0336      0.913 0.000 0.992 0.000 0.008
#&gt; GSM11228     4  0.0469      0.895 0.000 0.012 0.000 0.988
#&gt; GSM28697     4  0.0592      0.896 0.000 0.016 0.000 0.984
#&gt; GSM28698     3  0.0000      0.988 0.000 0.000 1.000 0.000
#&gt; GSM11238     3  0.0000      0.988 0.000 0.000 1.000 0.000
#&gt; GSM11242     3  0.0000      0.988 0.000 0.000 1.000 0.000
#&gt; GSM28719     4  0.3172      0.774 0.000 0.160 0.000 0.840
#&gt; GSM28708     4  0.3528      0.733 0.000 0.000 0.192 0.808
#&gt; GSM28722     4  0.1389      0.888 0.000 0.048 0.000 0.952
#&gt; GSM11232     4  0.0707      0.896 0.000 0.020 0.000 0.980
#&gt; GSM28709     3  0.0336      0.985 0.000 0.000 0.992 0.008
#&gt; GSM11226     4  0.2760      0.829 0.000 0.000 0.128 0.872
#&gt; GSM11239     3  0.0000      0.988 0.000 0.000 1.000 0.000
#&gt; GSM11225     3  0.0000      0.988 0.000 0.000 1.000 0.000
#&gt; GSM11220     3  0.0188      0.987 0.000 0.000 0.996 0.004
#&gt; GSM28701     2  0.4888      0.317 0.000 0.588 0.000 0.412
#&gt; GSM28721     4  0.1792      0.871 0.000 0.000 0.068 0.932
#&gt; GSM28713     2  0.4977      0.160 0.000 0.540 0.000 0.460
#&gt; GSM28716     1  0.0592      0.983 0.984 0.016 0.000 0.000
#&gt; GSM11221     2  0.1302      0.910 0.000 0.956 0.000 0.044
#&gt; GSM28717     2  0.0000      0.913 0.000 1.000 0.000 0.000
#&gt; GSM11223     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; GSM11218     4  0.1302      0.881 0.000 0.000 0.044 0.956
#&gt; GSM11219     2  0.0921      0.915 0.000 0.972 0.000 0.028
#&gt; GSM11236     3  0.1940      0.924 0.000 0.000 0.924 0.076
#&gt; GSM28702     3  0.0817      0.971 0.000 0.000 0.976 0.024
#&gt; GSM28705     4  0.0804      0.896 0.000 0.012 0.008 0.980
#&gt; GSM11230     2  0.1022      0.914 0.000 0.968 0.000 0.032
#&gt; GSM28704     4  0.4761      0.352 0.000 0.372 0.000 0.628
#&gt; GSM28700     2  0.1022      0.915 0.000 0.968 0.000 0.032
#&gt; GSM11224     2  0.2530      0.854 0.000 0.888 0.000 0.112
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-3-a').click(function(){
  $('#tab-CV-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-4'>
<p><a id='tab-CV-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28710     2  0.2286     0.8518 0.000 0.888 0.000 0.004 0.108
#&gt; GSM28711     2  0.1893     0.8711 0.000 0.928 0.000 0.048 0.024
#&gt; GSM28712     2  0.0566     0.8753 0.000 0.984 0.000 0.004 0.012
#&gt; GSM11222     3  0.0963     0.9585 0.000 0.000 0.964 0.000 0.036
#&gt; GSM28720     1  0.0000     0.9976 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000     0.9976 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000     0.9976 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000     0.9976 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000     0.9976 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000     0.9976 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000     0.9976 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000     0.9976 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000     0.9976 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000     0.9976 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.1386     0.8736 0.000 0.952 0.000 0.016 0.032
#&gt; GSM28714     2  0.1502     0.8662 0.000 0.940 0.000 0.004 0.056
#&gt; GSM11216     3  0.1892     0.9198 0.000 0.000 0.916 0.004 0.080
#&gt; GSM28715     2  0.2221     0.8602 0.000 0.912 0.000 0.036 0.052
#&gt; GSM11234     4  0.2727     0.7103 0.000 0.016 0.000 0.868 0.116
#&gt; GSM28699     2  0.2389     0.8464 0.004 0.880 0.000 0.000 0.116
#&gt; GSM11233     2  0.1341     0.8686 0.000 0.944 0.000 0.000 0.056
#&gt; GSM28718     2  0.1124     0.8708 0.000 0.960 0.000 0.004 0.036
#&gt; GSM11231     5  0.4883     0.6094 0.000 0.200 0.000 0.092 0.708
#&gt; GSM11237     2  0.2136     0.8467 0.000 0.904 0.000 0.008 0.088
#&gt; GSM11228     4  0.3607     0.5552 0.000 0.004 0.000 0.752 0.244
#&gt; GSM28697     4  0.4542     0.0707 0.000 0.008 0.000 0.536 0.456
#&gt; GSM28698     3  0.0162     0.9632 0.000 0.000 0.996 0.000 0.004
#&gt; GSM11238     3  0.0000     0.9636 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11242     3  0.0703     0.9636 0.000 0.000 0.976 0.000 0.024
#&gt; GSM28719     5  0.4303     0.6438 0.000 0.056 0.000 0.192 0.752
#&gt; GSM28708     5  0.4886     0.6713 0.000 0.028 0.064 0.160 0.748
#&gt; GSM28722     4  0.0510     0.7598 0.000 0.016 0.000 0.984 0.000
#&gt; GSM11232     4  0.2911     0.6872 0.000 0.008 0.004 0.852 0.136
#&gt; GSM28709     3  0.1197     0.9476 0.000 0.000 0.952 0.000 0.048
#&gt; GSM11226     4  0.1492     0.7566 0.000 0.004 0.040 0.948 0.008
#&gt; GSM11239     3  0.0609     0.9646 0.000 0.000 0.980 0.000 0.020
#&gt; GSM11225     3  0.0880     0.9616 0.000 0.000 0.968 0.000 0.032
#&gt; GSM11220     3  0.1704     0.9298 0.000 0.000 0.928 0.004 0.068
#&gt; GSM28701     2  0.6789    -0.0641 0.000 0.368 0.000 0.284 0.348
#&gt; GSM28721     4  0.1124     0.7563 0.000 0.004 0.036 0.960 0.000
#&gt; GSM28713     4  0.5065     0.1266 0.000 0.420 0.000 0.544 0.036
#&gt; GSM28716     1  0.0807     0.9735 0.976 0.012 0.000 0.000 0.012
#&gt; GSM11221     2  0.2592     0.8612 0.000 0.892 0.000 0.052 0.056
#&gt; GSM28717     2  0.2377     0.8412 0.000 0.872 0.000 0.000 0.128
#&gt; GSM11223     1  0.0000     0.9976 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     4  0.1469     0.7519 0.000 0.000 0.036 0.948 0.016
#&gt; GSM11219     2  0.0912     0.8760 0.000 0.972 0.000 0.016 0.012
#&gt; GSM11236     5  0.4798     0.2254 0.000 0.000 0.440 0.020 0.540
#&gt; GSM28702     3  0.0579     0.9639 0.000 0.000 0.984 0.008 0.008
#&gt; GSM28705     4  0.0324     0.7598 0.000 0.004 0.004 0.992 0.000
#&gt; GSM11230     2  0.1800     0.8708 0.000 0.932 0.000 0.048 0.020
#&gt; GSM28704     4  0.3883     0.6121 0.000 0.184 0.000 0.780 0.036
#&gt; GSM28700     2  0.3590     0.8257 0.000 0.828 0.000 0.092 0.080
#&gt; GSM11224     2  0.3561     0.6567 0.000 0.740 0.000 0.260 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-4-a').click(function(){
  $('#tab-CV-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-5'>
<p><a id='tab-CV-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28710     5  0.3995      0.368 0.000 0.480 0.000 0.004 0.516 0.000
#&gt; GSM28711     2  0.1500      0.743 0.000 0.936 0.000 0.000 0.052 0.012
#&gt; GSM28712     2  0.2389      0.669 0.000 0.864 0.000 0.000 0.128 0.008
#&gt; GSM11222     3  0.0858      0.887 0.000 0.000 0.968 0.000 0.028 0.004
#&gt; GSM28720     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.0551      0.759 0.000 0.984 0.000 0.004 0.004 0.008
#&gt; GSM28714     2  0.1173      0.752 0.000 0.960 0.000 0.016 0.016 0.008
#&gt; GSM11216     3  0.3897      0.740 0.000 0.000 0.684 0.008 0.300 0.008
#&gt; GSM28715     2  0.0862      0.755 0.000 0.972 0.000 0.008 0.004 0.016
#&gt; GSM11234     6  0.3833      0.630 0.000 0.016 0.000 0.004 0.272 0.708
#&gt; GSM28699     5  0.3899      0.552 0.000 0.404 0.000 0.004 0.592 0.000
#&gt; GSM11233     2  0.4062     -0.306 0.000 0.552 0.000 0.008 0.440 0.000
#&gt; GSM28718     2  0.0767      0.757 0.000 0.976 0.000 0.012 0.008 0.004
#&gt; GSM11231     4  0.1010      0.789 0.000 0.036 0.000 0.960 0.000 0.004
#&gt; GSM11237     2  0.1857      0.742 0.000 0.924 0.000 0.044 0.028 0.004
#&gt; GSM11228     4  0.3835      0.495 0.000 0.000 0.000 0.668 0.012 0.320
#&gt; GSM28697     4  0.3686      0.728 0.000 0.000 0.000 0.788 0.124 0.088
#&gt; GSM28698     3  0.1411      0.880 0.000 0.000 0.936 0.004 0.060 0.000
#&gt; GSM11238     3  0.1082      0.886 0.000 0.000 0.956 0.000 0.040 0.004
#&gt; GSM11242     3  0.0777      0.887 0.000 0.000 0.972 0.000 0.024 0.004
#&gt; GSM28719     4  0.0436      0.800 0.000 0.004 0.000 0.988 0.004 0.004
#&gt; GSM28708     4  0.0405      0.800 0.000 0.004 0.000 0.988 0.000 0.008
#&gt; GSM28722     6  0.1500      0.798 0.000 0.052 0.000 0.012 0.000 0.936
#&gt; GSM11232     6  0.3547      0.534 0.000 0.004 0.000 0.300 0.000 0.696
#&gt; GSM28709     3  0.3564      0.806 0.000 0.000 0.772 0.020 0.200 0.008
#&gt; GSM11226     6  0.3766      0.772 0.000 0.080 0.080 0.016 0.008 0.816
#&gt; GSM11239     3  0.0858      0.886 0.000 0.000 0.968 0.000 0.028 0.004
#&gt; GSM11225     3  0.1010      0.886 0.000 0.000 0.960 0.000 0.036 0.004
#&gt; GSM11220     3  0.4120      0.710 0.000 0.004 0.660 0.008 0.320 0.008
#&gt; GSM28701     5  0.5139      0.101 0.000 0.040 0.000 0.220 0.668 0.072
#&gt; GSM28721     6  0.1743      0.787 0.000 0.004 0.024 0.028 0.008 0.936
#&gt; GSM28713     6  0.3658      0.639 0.000 0.216 0.000 0.000 0.032 0.752
#&gt; GSM28716     1  0.0508      0.984 0.984 0.004 0.000 0.000 0.012 0.000
#&gt; GSM11221     2  0.4550      0.360 0.000 0.676 0.000 0.000 0.240 0.084
#&gt; GSM28717     5  0.3756      0.582 0.000 0.352 0.000 0.004 0.644 0.000
#&gt; GSM11223     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     6  0.3926      0.744 0.000 0.036 0.112 0.028 0.016 0.808
#&gt; GSM11219     2  0.0520      0.759 0.000 0.984 0.000 0.000 0.008 0.008
#&gt; GSM11236     4  0.5275      0.455 0.000 0.004 0.248 0.620 0.124 0.004
#&gt; GSM28702     3  0.0767      0.889 0.000 0.000 0.976 0.004 0.012 0.008
#&gt; GSM28705     6  0.1257      0.793 0.000 0.020 0.000 0.028 0.000 0.952
#&gt; GSM11230     2  0.1572      0.741 0.000 0.936 0.000 0.000 0.028 0.036
#&gt; GSM28704     6  0.2730      0.727 0.000 0.192 0.000 0.000 0.000 0.808
#&gt; GSM28700     2  0.4818     -0.125 0.000 0.572 0.000 0.000 0.364 0.064
#&gt; GSM11224     2  0.4002      0.467 0.000 0.704 0.000 0.000 0.036 0.260
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-5-a').click(function(){
  $('#tab-CV-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-NMF-signature_compare](figure_cola/CV-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-CV-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-NMF-collect-classes](figure_cola/CV-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n tissue(p) k
#> CV:NMF 48     0.432 2
#> CV:NMF 54     0.374 3
#> CV:NMF 51     0.350 4
#> CV:NMF 50     0.525 5
#> CV:NMF 46     0.455 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:hclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "hclust"]
# you can also extract it by
# res = res_list["MAD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21452 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-hclust-collect-plots](figure_cola/MAD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-hclust-select-partition-number](figure_cola/MAD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.325           0.688       0.864         0.4154 0.648   0.648
#> 3 3 0.536           0.702       0.824         0.4641 0.762   0.632
#> 4 4 0.821           0.792       0.913         0.2008 0.810   0.554
#> 5 5 0.828           0.731       0.811         0.0753 0.969   0.880
#> 6 6 0.809           0.659       0.778         0.0447 0.909   0.626
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-classes'>
<ul>
<li><a href='#tab-MAD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-hclust-get-classes-1'>
<p><a id='tab-MAD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28710     2  0.7883      0.629 0.236 0.764
#&gt; GSM28711     2  0.7883      0.629 0.236 0.764
#&gt; GSM28712     2  0.0000      0.799 0.000 1.000
#&gt; GSM11222     1  0.6973      0.690 0.812 0.188
#&gt; GSM28720     2  0.7056      0.703 0.192 0.808
#&gt; GSM11217     2  0.7056      0.703 0.192 0.808
#&gt; GSM28723     2  0.7056      0.703 0.192 0.808
#&gt; GSM11241     2  0.7056      0.703 0.192 0.808
#&gt; GSM28703     2  0.7056      0.703 0.192 0.808
#&gt; GSM11227     2  0.7056      0.703 0.192 0.808
#&gt; GSM28706     2  0.7056      0.703 0.192 0.808
#&gt; GSM11229     2  0.7056      0.703 0.192 0.808
#&gt; GSM11235     2  0.7056      0.703 0.192 0.808
#&gt; GSM28707     2  0.7056      0.703 0.192 0.808
#&gt; GSM11240     2  0.0000      0.799 0.000 1.000
#&gt; GSM28714     2  0.0000      0.799 0.000 1.000
#&gt; GSM11216     1  0.0000      0.865 1.000 0.000
#&gt; GSM28715     2  0.0672      0.797 0.008 0.992
#&gt; GSM11234     2  0.0000      0.799 0.000 1.000
#&gt; GSM28699     2  0.0000      0.799 0.000 1.000
#&gt; GSM11233     2  0.0000      0.799 0.000 1.000
#&gt; GSM28718     2  0.0000      0.799 0.000 1.000
#&gt; GSM11231     2  0.0672      0.797 0.008 0.992
#&gt; GSM11237     2  0.0000      0.799 0.000 1.000
#&gt; GSM11228     2  0.9866      0.217 0.432 0.568
#&gt; GSM28697     2  0.9866      0.217 0.432 0.568
#&gt; GSM28698     1  0.0000      0.865 1.000 0.000
#&gt; GSM11238     1  0.0000      0.865 1.000 0.000
#&gt; GSM11242     1  0.0000      0.865 1.000 0.000
#&gt; GSM28719     2  0.9881      0.206 0.436 0.564
#&gt; GSM28708     2  0.9881      0.206 0.436 0.564
#&gt; GSM28722     2  0.6623      0.698 0.172 0.828
#&gt; GSM11232     2  0.6973      0.683 0.188 0.812
#&gt; GSM28709     1  0.0000      0.865 1.000 0.000
#&gt; GSM11226     1  0.9580      0.398 0.620 0.380
#&gt; GSM11239     1  0.0000      0.865 1.000 0.000
#&gt; GSM11225     1  0.0000      0.865 1.000 0.000
#&gt; GSM11220     1  0.0000      0.865 1.000 0.000
#&gt; GSM28701     2  0.7883      0.629 0.236 0.764
#&gt; GSM28721     2  0.9866      0.217 0.432 0.568
#&gt; GSM28713     2  0.3584      0.772 0.068 0.932
#&gt; GSM28716     2  0.0000      0.799 0.000 1.000
#&gt; GSM11221     2  0.0376      0.798 0.004 0.996
#&gt; GSM28717     2  0.0000      0.799 0.000 1.000
#&gt; GSM11223     2  0.7056      0.703 0.192 0.808
#&gt; GSM11218     1  0.9580      0.398 0.620 0.380
#&gt; GSM11219     2  0.0000      0.799 0.000 1.000
#&gt; GSM11236     2  0.8081      0.612 0.248 0.752
#&gt; GSM28702     1  0.6973      0.690 0.812 0.188
#&gt; GSM28705     2  0.9795      0.261 0.416 0.584
#&gt; GSM11230     2  0.0376      0.798 0.004 0.996
#&gt; GSM28704     2  0.4562      0.757 0.096 0.904
#&gt; GSM28700     2  0.0000      0.799 0.000 1.000
#&gt; GSM11224     2  0.0000      0.799 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-1-a').click(function(){
  $('#tab-MAD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-2'>
<p><a id='tab-MAD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28710     2   0.355      0.562 0.000 0.868 0.132
#&gt; GSM28711     2   0.355      0.562 0.000 0.868 0.132
#&gt; GSM28712     2   0.529      0.707 0.268 0.732 0.000
#&gt; GSM11222     3   0.550      0.680 0.000 0.292 0.708
#&gt; GSM28720     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM11217     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM28723     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM11241     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM28703     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM11227     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM28706     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM11229     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM11235     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM28707     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM11240     2   0.522      0.711 0.260 0.740 0.000
#&gt; GSM28714     2   0.522      0.711 0.260 0.740 0.000
#&gt; GSM11216     3   0.000      0.846 0.000 0.000 1.000
#&gt; GSM28715     2   0.562      0.712 0.260 0.732 0.008
#&gt; GSM11234     2   0.529      0.707 0.268 0.732 0.000
#&gt; GSM28699     2   0.556      0.671 0.300 0.700 0.000
#&gt; GSM11233     2   0.556      0.671 0.300 0.700 0.000
#&gt; GSM28718     2   0.522      0.711 0.260 0.740 0.000
#&gt; GSM11231     2   0.562      0.712 0.260 0.732 0.008
#&gt; GSM11237     2   0.522      0.711 0.260 0.740 0.000
#&gt; GSM11228     2   0.576      0.228 0.000 0.672 0.328
#&gt; GSM28697     2   0.576      0.228 0.000 0.672 0.328
#&gt; GSM28698     3   0.000      0.846 0.000 0.000 1.000
#&gt; GSM11238     3   0.000      0.846 0.000 0.000 1.000
#&gt; GSM11242     3   0.000      0.846 0.000 0.000 1.000
#&gt; GSM28719     2   0.579      0.219 0.000 0.668 0.332
#&gt; GSM28708     2   0.579      0.219 0.000 0.668 0.332
#&gt; GSM28722     2   0.226      0.611 0.000 0.932 0.068
#&gt; GSM11232     2   0.263      0.601 0.000 0.916 0.084
#&gt; GSM28709     3   0.000      0.846 0.000 0.000 1.000
#&gt; GSM11226     3   0.630      0.337 0.000 0.484 0.516
#&gt; GSM11239     3   0.000      0.846 0.000 0.000 1.000
#&gt; GSM11225     3   0.000      0.846 0.000 0.000 1.000
#&gt; GSM11220     3   0.000      0.846 0.000 0.000 1.000
#&gt; GSM28701     2   0.355      0.562 0.000 0.868 0.132
#&gt; GSM28721     2   0.576      0.228 0.000 0.672 0.328
#&gt; GSM28713     2   0.141      0.666 0.036 0.964 0.000
#&gt; GSM28716     2   0.536      0.702 0.276 0.724 0.000
#&gt; GSM11221     2   0.544      0.712 0.260 0.736 0.004
#&gt; GSM28717     2   0.556      0.671 0.300 0.700 0.000
#&gt; GSM11223     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM11218     3   0.630      0.337 0.000 0.484 0.516
#&gt; GSM11219     2   0.522      0.711 0.260 0.740 0.000
#&gt; GSM11236     2   0.375      0.548 0.000 0.856 0.144
#&gt; GSM28702     3   0.550      0.680 0.000 0.292 0.708
#&gt; GSM28705     2   0.565      0.262 0.000 0.688 0.312
#&gt; GSM11230     2   0.544      0.712 0.260 0.736 0.004
#&gt; GSM28704     2   0.205      0.654 0.028 0.952 0.020
#&gt; GSM28700     2   0.529      0.707 0.268 0.732 0.000
#&gt; GSM11224     2   0.529      0.707 0.268 0.732 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-2-a').click(function(){
  $('#tab-MAD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-3'>
<p><a id='tab-MAD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28710     4  0.4522      0.620 0.000 0.320 0.000 0.680
#&gt; GSM28711     4  0.4522      0.620 0.000 0.320 0.000 0.680
#&gt; GSM28712     2  0.0000      0.918 0.000 1.000 0.000 0.000
#&gt; GSM11222     3  0.4955      0.238 0.000 0.000 0.556 0.444
#&gt; GSM28720     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.0336      0.919 0.000 0.992 0.000 0.008
#&gt; GSM28714     2  0.0336      0.919 0.000 0.992 0.000 0.008
#&gt; GSM11216     3  0.0000      0.887 0.000 0.000 1.000 0.000
#&gt; GSM28715     2  0.1302      0.894 0.000 0.956 0.000 0.044
#&gt; GSM11234     2  0.0000      0.918 0.000 1.000 0.000 0.000
#&gt; GSM28699     2  0.1022      0.897 0.032 0.968 0.000 0.000
#&gt; GSM11233     2  0.1022      0.897 0.032 0.968 0.000 0.000
#&gt; GSM28718     2  0.0336      0.919 0.000 0.992 0.000 0.008
#&gt; GSM11231     2  0.2081      0.856 0.000 0.916 0.000 0.084
#&gt; GSM11237     2  0.0336      0.919 0.000 0.992 0.000 0.008
#&gt; GSM11228     4  0.1022      0.727 0.000 0.032 0.000 0.968
#&gt; GSM28697     4  0.1022      0.727 0.000 0.032 0.000 0.968
#&gt; GSM28698     3  0.0000      0.887 0.000 0.000 1.000 0.000
#&gt; GSM11238     3  0.0000      0.887 0.000 0.000 1.000 0.000
#&gt; GSM11242     3  0.0000      0.887 0.000 0.000 1.000 0.000
#&gt; GSM28719     4  0.0000      0.711 0.000 0.000 0.000 1.000
#&gt; GSM28708     4  0.0000      0.711 0.000 0.000 0.000 1.000
#&gt; GSM28722     4  0.4961      0.385 0.000 0.448 0.000 0.552
#&gt; GSM11232     4  0.4925      0.434 0.000 0.428 0.000 0.572
#&gt; GSM28709     3  0.0000      0.887 0.000 0.000 1.000 0.000
#&gt; GSM11226     4  0.3710      0.510 0.000 0.004 0.192 0.804
#&gt; GSM11239     3  0.0000      0.887 0.000 0.000 1.000 0.000
#&gt; GSM11225     3  0.0000      0.887 0.000 0.000 1.000 0.000
#&gt; GSM11220     3  0.0000      0.887 0.000 0.000 1.000 0.000
#&gt; GSM28701     4  0.4522      0.620 0.000 0.320 0.000 0.680
#&gt; GSM28721     4  0.1022      0.727 0.000 0.032 0.000 0.968
#&gt; GSM28713     2  0.4761      0.179 0.000 0.628 0.000 0.372
#&gt; GSM28716     2  0.0336      0.916 0.008 0.992 0.000 0.000
#&gt; GSM11221     2  0.0469      0.918 0.000 0.988 0.000 0.012
#&gt; GSM28717     2  0.1022      0.897 0.032 0.968 0.000 0.000
#&gt; GSM11223     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11218     4  0.3710      0.510 0.000 0.004 0.192 0.804
#&gt; GSM11219     2  0.0592      0.916 0.000 0.984 0.000 0.016
#&gt; GSM11236     4  0.4222      0.663 0.000 0.272 0.000 0.728
#&gt; GSM28702     3  0.4955      0.238 0.000 0.000 0.556 0.444
#&gt; GSM28705     4  0.1474      0.729 0.000 0.052 0.000 0.948
#&gt; GSM11230     2  0.0592      0.917 0.000 0.984 0.000 0.016
#&gt; GSM28704     2  0.4961     -0.108 0.000 0.552 0.000 0.448
#&gt; GSM28700     2  0.0000      0.918 0.000 1.000 0.000 0.000
#&gt; GSM11224     2  0.0336      0.918 0.000 0.992 0.000 0.008
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-3-a').click(function(){
  $('#tab-MAD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-4'>
<p><a id='tab-MAD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28710     4  0.5661     0.6243 0.000 0.272 0.000 0.608 0.120
#&gt; GSM28711     4  0.5661     0.6243 0.000 0.272 0.000 0.608 0.120
#&gt; GSM28712     2  0.3999     0.6891 0.000 0.656 0.000 0.000 0.344
#&gt; GSM11222     5  0.6153     0.5241 0.000 0.000 0.380 0.136 0.484
#&gt; GSM28720     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.0000     0.7420 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28714     2  0.0000     0.7420 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11216     3  0.0290     0.9766 0.000 0.000 0.992 0.000 0.008
#&gt; GSM28715     2  0.1106     0.7170 0.000 0.964 0.000 0.012 0.024
#&gt; GSM11234     2  0.3999     0.6891 0.000 0.656 0.000 0.000 0.344
#&gt; GSM28699     2  0.4201     0.6553 0.000 0.592 0.000 0.000 0.408
#&gt; GSM11233     2  0.4201     0.6553 0.000 0.592 0.000 0.000 0.408
#&gt; GSM28718     2  0.0000     0.7420 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11231     2  0.1992     0.6900 0.000 0.924 0.000 0.044 0.032
#&gt; GSM11237     2  0.0000     0.7420 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11228     4  0.1168     0.5687 0.000 0.032 0.000 0.960 0.008
#&gt; GSM28697     4  0.1168     0.5687 0.000 0.032 0.000 0.960 0.008
#&gt; GSM28698     3  0.0000     0.9785 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11238     3  0.0703     0.9789 0.000 0.000 0.976 0.000 0.024
#&gt; GSM11242     3  0.0703     0.9789 0.000 0.000 0.976 0.000 0.024
#&gt; GSM28719     4  0.1608     0.4852 0.000 0.000 0.000 0.928 0.072
#&gt; GSM28708     4  0.1608     0.4852 0.000 0.000 0.000 0.928 0.072
#&gt; GSM28722     4  0.5604     0.4513 0.000 0.456 0.000 0.472 0.072
#&gt; GSM11232     4  0.5594     0.4868 0.000 0.436 0.000 0.492 0.072
#&gt; GSM28709     3  0.0290     0.9766 0.000 0.000 0.992 0.000 0.008
#&gt; GSM11226     5  0.4886     0.5634 0.000 0.004 0.016 0.468 0.512
#&gt; GSM11239     3  0.0703     0.9789 0.000 0.000 0.976 0.000 0.024
#&gt; GSM11225     3  0.0703     0.9789 0.000 0.000 0.976 0.000 0.024
#&gt; GSM11220     3  0.0290     0.9766 0.000 0.000 0.992 0.000 0.008
#&gt; GSM28701     4  0.5661     0.6243 0.000 0.272 0.000 0.608 0.120
#&gt; GSM28721     4  0.2712     0.5386 0.000 0.032 0.000 0.880 0.088
#&gt; GSM28713     2  0.6824     0.0528 0.000 0.344 0.000 0.328 0.328
#&gt; GSM28716     2  0.4298     0.6823 0.008 0.640 0.000 0.000 0.352
#&gt; GSM11221     2  0.0324     0.7409 0.000 0.992 0.000 0.004 0.004
#&gt; GSM28717     2  0.4201     0.6553 0.000 0.592 0.000 0.000 0.408
#&gt; GSM11223     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     5  0.4886     0.5634 0.000 0.004 0.016 0.468 0.512
#&gt; GSM11219     2  0.0609     0.7413 0.000 0.980 0.000 0.000 0.020
#&gt; GSM11236     4  0.5150     0.6238 0.000 0.272 0.000 0.652 0.076
#&gt; GSM28702     5  0.6153     0.5241 0.000 0.000 0.380 0.136 0.484
#&gt; GSM28705     4  0.2790     0.5612 0.000 0.052 0.000 0.880 0.068
#&gt; GSM11230     2  0.0290     0.7394 0.000 0.992 0.000 0.008 0.000
#&gt; GSM28704     2  0.5480    -0.2523 0.000 0.560 0.000 0.368 0.072
#&gt; GSM28700     2  0.3999     0.6891 0.000 0.656 0.000 0.000 0.344
#&gt; GSM11224     2  0.1544     0.7393 0.000 0.932 0.000 0.000 0.068
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-4-a').click(function(){
  $('#tab-MAD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-5'>
<p><a id='tab-MAD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28710     4  0.5697    0.58251 0.000 0.164 0.000 0.636 0.052 0.148
#&gt; GSM28711     4  0.5697    0.58251 0.000 0.164 0.000 0.636 0.052 0.148
#&gt; GSM28712     5  0.1863    0.81313 0.000 0.104 0.000 0.000 0.896 0.000
#&gt; GSM11222     6  0.3240    0.54894 0.000 0.000 0.244 0.004 0.000 0.752
#&gt; GSM28720     1  0.0000    1.00000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000    1.00000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000    1.00000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000    1.00000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000    1.00000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000    1.00000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000    1.00000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000    1.00000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000    1.00000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000    1.00000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.4482    0.52027 0.000 0.580 0.000 0.000 0.384 0.036
#&gt; GSM28714     2  0.4482    0.52027 0.000 0.580 0.000 0.000 0.384 0.036
#&gt; GSM11216     3  0.0146    0.87101 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM28715     2  0.5004    0.50619 0.000 0.568 0.000 0.008 0.364 0.060
#&gt; GSM11234     5  0.1863    0.81313 0.000 0.104 0.000 0.000 0.896 0.000
#&gt; GSM28699     5  0.0146    0.79358 0.000 0.000 0.000 0.000 0.996 0.004
#&gt; GSM11233     5  0.0146    0.79358 0.000 0.000 0.000 0.000 0.996 0.004
#&gt; GSM28718     2  0.4482    0.52027 0.000 0.580 0.000 0.000 0.384 0.036
#&gt; GSM11231     2  0.5719    0.47543 0.000 0.524 0.000 0.040 0.364 0.072
#&gt; GSM11237     2  0.4482    0.52027 0.000 0.580 0.000 0.000 0.384 0.036
#&gt; GSM11228     4  0.3421    0.52685 0.000 0.256 0.000 0.736 0.000 0.008
#&gt; GSM28697     4  0.3421    0.52685 0.000 0.256 0.000 0.736 0.000 0.008
#&gt; GSM28698     3  0.2092    0.90956 0.000 0.000 0.876 0.000 0.000 0.124
#&gt; GSM11238     3  0.2416    0.91240 0.000 0.000 0.844 0.000 0.000 0.156
#&gt; GSM11242     3  0.2416    0.91240 0.000 0.000 0.844 0.000 0.000 0.156
#&gt; GSM28719     4  0.0632    0.52310 0.000 0.000 0.000 0.976 0.000 0.024
#&gt; GSM28708     4  0.0713    0.52187 0.000 0.000 0.000 0.972 0.000 0.028
#&gt; GSM28722     2  0.3854    0.00195 0.000 0.772 0.000 0.136 0.000 0.092
#&gt; GSM11232     2  0.4085   -0.05429 0.000 0.748 0.000 0.156 0.000 0.096
#&gt; GSM28709     3  0.0146    0.87101 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM11226     6  0.3601    0.58378 0.000 0.004 0.000 0.312 0.000 0.684
#&gt; GSM11239     3  0.2416    0.91240 0.000 0.000 0.844 0.000 0.000 0.156
#&gt; GSM11225     3  0.2416    0.91240 0.000 0.000 0.844 0.000 0.000 0.156
#&gt; GSM11220     3  0.0146    0.87101 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM28701     4  0.5697    0.58251 0.000 0.164 0.000 0.636 0.052 0.148
#&gt; GSM28721     4  0.5217    0.37272 0.000 0.392 0.000 0.512 0.000 0.096
#&gt; GSM28713     2  0.4314   -0.15891 0.000 0.536 0.000 0.020 0.444 0.000
#&gt; GSM28716     5  0.1866    0.81541 0.008 0.084 0.000 0.000 0.908 0.000
#&gt; GSM11221     2  0.4419    0.52428 0.000 0.584 0.000 0.000 0.384 0.032
#&gt; GSM28717     5  0.0146    0.79358 0.000 0.000 0.000 0.000 0.996 0.004
#&gt; GSM11223     1  0.0000    1.00000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     6  0.3601    0.58378 0.000 0.004 0.000 0.312 0.000 0.684
#&gt; GSM11219     2  0.4224    0.44011 0.000 0.552 0.000 0.000 0.432 0.016
#&gt; GSM11236     4  0.4767    0.58411 0.000 0.168 0.000 0.676 0.000 0.156
#&gt; GSM28702     6  0.3240    0.54894 0.000 0.000 0.244 0.004 0.000 0.752
#&gt; GSM28705     4  0.4967    0.39610 0.000 0.420 0.000 0.512 0.000 0.068
#&gt; GSM11230     2  0.4473    0.52685 0.000 0.584 0.000 0.000 0.380 0.036
#&gt; GSM28704     2  0.2487    0.22079 0.000 0.876 0.000 0.032 0.000 0.092
#&gt; GSM28700     5  0.1863    0.81313 0.000 0.104 0.000 0.000 0.896 0.000
#&gt; GSM11224     5  0.4234   -0.24091 0.000 0.440 0.000 0.000 0.544 0.016
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-5-a').click(function(){
  $('#tab-MAD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-hclust-signature_compare](figure_cola/MAD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-hclust-collect-classes](figure_cola/MAD-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> MAD:hclust 46     0.389 2
#> MAD:hclust 46     0.364 3
#> MAD:hclust 48     0.346 4
#> MAD:hclust 48     0.328 5
#> MAD:hclust 45     0.394 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "kmeans"]
# you can also extract it by
# res = res_list["MAD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21452 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-kmeans-collect-plots](figure_cola/MAD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-kmeans-select-partition-number](figure_cola/MAD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.353           0.457       0.716         0.4144 0.628   0.628
#> 3 3 0.997           0.938       0.968         0.4016 0.725   0.590
#> 4 4 0.798           0.905       0.920         0.2507 0.818   0.586
#> 5 5 0.777           0.691       0.840         0.0858 0.929   0.734
#> 6 6 0.766           0.673       0.783         0.0470 0.945   0.759
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-kmeans-get-classes-1'>
<p><a id='tab-MAD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28710     2   0.971      0.584 0.400 0.600
#&gt; GSM28711     2   0.971      0.584 0.400 0.600
#&gt; GSM28712     2   0.971      0.584 0.400 0.600
#&gt; GSM11222     2   0.981     -0.148 0.420 0.580
#&gt; GSM28720     1   0.000      0.864 1.000 0.000
#&gt; GSM11217     1   0.000      0.864 1.000 0.000
#&gt; GSM28723     1   0.000      0.864 1.000 0.000
#&gt; GSM11241     1   0.000      0.864 1.000 0.000
#&gt; GSM28703     1   0.000      0.864 1.000 0.000
#&gt; GSM11227     1   0.000      0.864 1.000 0.000
#&gt; GSM28706     1   0.000      0.864 1.000 0.000
#&gt; GSM11229     1   0.000      0.864 1.000 0.000
#&gt; GSM11235     1   0.000      0.864 1.000 0.000
#&gt; GSM28707     1   0.000      0.864 1.000 0.000
#&gt; GSM11240     2   0.971      0.584 0.400 0.600
#&gt; GSM28714     2   0.971      0.584 0.400 0.600
#&gt; GSM11216     2   0.981     -0.148 0.420 0.580
#&gt; GSM28715     2   0.971      0.584 0.400 0.600
#&gt; GSM11234     2   0.971      0.584 0.400 0.600
#&gt; GSM28699     2   0.971      0.584 0.400 0.600
#&gt; GSM11233     2   0.971      0.584 0.400 0.600
#&gt; GSM28718     2   0.971      0.584 0.400 0.600
#&gt; GSM11231     2   0.971      0.584 0.400 0.600
#&gt; GSM11237     2   0.971      0.584 0.400 0.600
#&gt; GSM11228     2   0.971      0.584 0.400 0.600
#&gt; GSM28697     2   0.971      0.584 0.400 0.600
#&gt; GSM28698     2   0.981     -0.148 0.420 0.580
#&gt; GSM11238     2   0.981     -0.148 0.420 0.580
#&gt; GSM11242     2   0.981     -0.148 0.420 0.580
#&gt; GSM28719     2   0.971      0.584 0.400 0.600
#&gt; GSM28708     2   0.595      0.365 0.144 0.856
#&gt; GSM28722     2   0.971      0.584 0.400 0.600
#&gt; GSM11232     2   0.971      0.584 0.400 0.600
#&gt; GSM28709     2   0.981     -0.148 0.420 0.580
#&gt; GSM11226     2   0.584      0.362 0.140 0.860
#&gt; GSM11239     2   0.981     -0.148 0.420 0.580
#&gt; GSM11225     2   0.981     -0.148 0.420 0.580
#&gt; GSM11220     2   0.981     -0.148 0.420 0.580
#&gt; GSM28701     2   0.971      0.584 0.400 0.600
#&gt; GSM28721     2   0.584      0.362 0.140 0.860
#&gt; GSM28713     2   0.971      0.584 0.400 0.600
#&gt; GSM28716     1   0.981     -0.222 0.580 0.420
#&gt; GSM11221     2   0.971      0.584 0.400 0.600
#&gt; GSM28717     2   0.971      0.584 0.400 0.600
#&gt; GSM11223     1   0.000      0.864 1.000 0.000
#&gt; GSM11218     2   0.456      0.323 0.096 0.904
#&gt; GSM11219     2   0.971      0.584 0.400 0.600
#&gt; GSM11236     1   0.994     -0.330 0.544 0.456
#&gt; GSM28702     2   0.981     -0.148 0.420 0.580
#&gt; GSM28705     2   0.971      0.584 0.400 0.600
#&gt; GSM11230     2   0.971      0.584 0.400 0.600
#&gt; GSM28704     2   0.971      0.584 0.400 0.600
#&gt; GSM28700     2   0.971      0.584 0.400 0.600
#&gt; GSM11224     2   0.971      0.584 0.400 0.600
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-2'>
<p><a id='tab-MAD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28710     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM28711     2  0.0747      0.951 0.000 0.984 0.016
#&gt; GSM28712     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM11222     3  0.0237      0.981 0.004 0.000 0.996
#&gt; GSM28720     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11217     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28723     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11241     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28703     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11227     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28706     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11229     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11235     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28707     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11240     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM28714     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM11216     3  0.0892      0.995 0.020 0.000 0.980
#&gt; GSM28715     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM11234     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM28699     2  0.0237      0.952 0.000 0.996 0.004
#&gt; GSM11233     2  0.0237      0.952 0.000 0.996 0.004
#&gt; GSM28718     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM11231     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM11237     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM11228     2  0.1289      0.943 0.000 0.968 0.032
#&gt; GSM28697     2  0.0747      0.951 0.000 0.984 0.016
#&gt; GSM28698     3  0.0892      0.995 0.020 0.000 0.980
#&gt; GSM11238     3  0.0892      0.995 0.020 0.000 0.980
#&gt; GSM11242     3  0.0892      0.995 0.020 0.000 0.980
#&gt; GSM28719     2  0.1289      0.943 0.000 0.968 0.032
#&gt; GSM28708     2  0.6095      0.436 0.000 0.608 0.392
#&gt; GSM28722     2  0.0747      0.951 0.000 0.984 0.016
#&gt; GSM11232     2  0.0747      0.951 0.000 0.984 0.016
#&gt; GSM28709     3  0.0892      0.995 0.020 0.000 0.980
#&gt; GSM11226     2  0.2878      0.888 0.000 0.904 0.096
#&gt; GSM11239     3  0.0892      0.995 0.020 0.000 0.980
#&gt; GSM11225     3  0.0892      0.995 0.020 0.000 0.980
#&gt; GSM11220     3  0.0892      0.995 0.020 0.000 0.980
#&gt; GSM28701     2  0.0747      0.951 0.000 0.984 0.016
#&gt; GSM28721     2  0.5905      0.520 0.000 0.648 0.352
#&gt; GSM28713     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM28716     2  0.0237      0.952 0.000 0.996 0.004
#&gt; GSM11221     2  0.0747      0.951 0.000 0.984 0.016
#&gt; GSM28717     2  0.0237      0.952 0.000 0.996 0.004
#&gt; GSM11223     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11218     2  0.6235      0.324 0.000 0.564 0.436
#&gt; GSM11219     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM11236     2  0.1525      0.941 0.004 0.964 0.032
#&gt; GSM28702     3  0.0237      0.981 0.004 0.000 0.996
#&gt; GSM28705     2  0.1289      0.943 0.000 0.968 0.032
#&gt; GSM11230     2  0.0747      0.951 0.000 0.984 0.016
#&gt; GSM28704     2  0.0747      0.951 0.000 0.984 0.016
#&gt; GSM28700     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM11224     2  0.0000      0.953 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-3'>
<p><a id='tab-MAD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28710     2  0.4193     0.7396 0.000 0.732 0.000 0.268
#&gt; GSM28711     4  0.2704     0.8821 0.000 0.124 0.000 0.876
#&gt; GSM28712     2  0.1792     0.8939 0.000 0.932 0.000 0.068
#&gt; GSM11222     3  0.0000     0.9864 0.000 0.000 1.000 0.000
#&gt; GSM28720     1  0.0000     0.9956 1.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000     0.9956 1.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000     0.9956 1.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0469     0.9917 0.988 0.000 0.000 0.012
#&gt; GSM28703     1  0.0000     0.9956 1.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000     0.9956 1.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0469     0.9908 0.988 0.000 0.000 0.012
#&gt; GSM11229     1  0.0000     0.9956 1.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000     0.9956 1.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0469     0.9917 0.988 0.000 0.000 0.012
#&gt; GSM11240     2  0.2469     0.8969 0.000 0.892 0.000 0.108
#&gt; GSM28714     2  0.2469     0.8969 0.000 0.892 0.000 0.108
#&gt; GSM11216     3  0.1305     0.9811 0.004 0.000 0.960 0.036
#&gt; GSM28715     2  0.2469     0.8969 0.000 0.892 0.000 0.108
#&gt; GSM11234     2  0.3649     0.8182 0.000 0.796 0.000 0.204
#&gt; GSM28699     2  0.0336     0.8573 0.000 0.992 0.000 0.008
#&gt; GSM11233     2  0.0188     0.8558 0.000 0.996 0.000 0.004
#&gt; GSM28718     2  0.2469     0.8969 0.000 0.892 0.000 0.108
#&gt; GSM11231     2  0.3837     0.7852 0.000 0.776 0.000 0.224
#&gt; GSM11237     2  0.2469     0.8969 0.000 0.892 0.000 0.108
#&gt; GSM11228     4  0.1716     0.9306 0.000 0.064 0.000 0.936
#&gt; GSM28697     4  0.1716     0.9306 0.000 0.064 0.000 0.936
#&gt; GSM28698     3  0.1004     0.9842 0.004 0.000 0.972 0.024
#&gt; GSM11238     3  0.0188     0.9874 0.004 0.000 0.996 0.000
#&gt; GSM11242     3  0.0376     0.9875 0.004 0.000 0.992 0.004
#&gt; GSM28719     4  0.1716     0.9306 0.000 0.064 0.000 0.936
#&gt; GSM28708     4  0.2124     0.9182 0.000 0.040 0.028 0.932
#&gt; GSM28722     4  0.5105     0.0766 0.000 0.432 0.004 0.564
#&gt; GSM11232     4  0.1978     0.9296 0.000 0.068 0.004 0.928
#&gt; GSM28709     3  0.1305     0.9811 0.004 0.000 0.960 0.036
#&gt; GSM11226     4  0.2142     0.9261 0.000 0.056 0.016 0.928
#&gt; GSM11239     3  0.0376     0.9875 0.004 0.000 0.992 0.004
#&gt; GSM11225     3  0.0376     0.9875 0.004 0.000 0.992 0.004
#&gt; GSM11220     3  0.1305     0.9811 0.004 0.000 0.960 0.036
#&gt; GSM28701     4  0.2647     0.8887 0.000 0.120 0.000 0.880
#&gt; GSM28721     4  0.2214     0.9183 0.000 0.044 0.028 0.928
#&gt; GSM28713     2  0.2011     0.8953 0.000 0.920 0.000 0.080
#&gt; GSM28716     2  0.1389     0.8484 0.000 0.952 0.000 0.048
#&gt; GSM11221     2  0.3975     0.7810 0.000 0.760 0.000 0.240
#&gt; GSM28717     2  0.0336     0.8573 0.000 0.992 0.000 0.008
#&gt; GSM11223     1  0.0817     0.9860 0.976 0.000 0.000 0.024
#&gt; GSM11218     4  0.2142     0.8887 0.000 0.016 0.056 0.928
#&gt; GSM11219     2  0.2469     0.8969 0.000 0.892 0.000 0.108
#&gt; GSM11236     4  0.1716     0.9306 0.000 0.064 0.000 0.936
#&gt; GSM28702     3  0.0000     0.9864 0.000 0.000 1.000 0.000
#&gt; GSM28705     4  0.1978     0.9296 0.000 0.068 0.004 0.928
#&gt; GSM11230     2  0.4372     0.7585 0.000 0.728 0.004 0.268
#&gt; GSM28704     2  0.4222     0.7591 0.000 0.728 0.000 0.272
#&gt; GSM28700     2  0.1792     0.8939 0.000 0.932 0.000 0.068
#&gt; GSM11224     2  0.1867     0.8948 0.000 0.928 0.000 0.072
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-4'>
<p><a id='tab-MAD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28710     5  0.6526     0.4227 0.000 0.260 0.000 0.256 0.484
#&gt; GSM28711     4  0.4394     0.7122 0.000 0.048 0.000 0.732 0.220
#&gt; GSM28712     2  0.1608     0.5362 0.000 0.928 0.000 0.000 0.072
#&gt; GSM11222     3  0.2338     0.8722 0.000 0.000 0.884 0.004 0.112
#&gt; GSM28720     1  0.0000     0.9863 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000     0.9863 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000     0.9863 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0794     0.9780 0.972 0.000 0.000 0.000 0.028
#&gt; GSM28703     1  0.0000     0.9863 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000     0.9863 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.1341     0.9609 0.944 0.000 0.000 0.000 0.056
#&gt; GSM11229     1  0.0000     0.9863 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000     0.9863 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0794     0.9780 0.972 0.000 0.000 0.000 0.028
#&gt; GSM11240     2  0.0451     0.5980 0.000 0.988 0.000 0.008 0.004
#&gt; GSM28714     2  0.0451     0.5980 0.000 0.988 0.000 0.008 0.004
#&gt; GSM11216     3  0.2997     0.9016 0.000 0.000 0.840 0.012 0.148
#&gt; GSM28715     2  0.0807     0.5979 0.000 0.976 0.000 0.012 0.012
#&gt; GSM11234     5  0.5992     0.4014 0.000 0.416 0.000 0.112 0.472
#&gt; GSM28699     5  0.4300     0.5862 0.000 0.476 0.000 0.000 0.524
#&gt; GSM11233     2  0.4171    -0.3472 0.000 0.604 0.000 0.000 0.396
#&gt; GSM28718     2  0.0451     0.5980 0.000 0.988 0.000 0.008 0.004
#&gt; GSM11231     2  0.1952     0.5383 0.000 0.912 0.000 0.084 0.004
#&gt; GSM11237     2  0.0451     0.5980 0.000 0.988 0.000 0.008 0.004
#&gt; GSM11228     4  0.1211     0.8942 0.000 0.016 0.000 0.960 0.024
#&gt; GSM28697     4  0.1211     0.8942 0.000 0.016 0.000 0.960 0.024
#&gt; GSM28698     3  0.2771     0.9065 0.000 0.000 0.860 0.012 0.128
#&gt; GSM11238     3  0.0162     0.9208 0.000 0.000 0.996 0.004 0.000
#&gt; GSM11242     3  0.0451     0.9210 0.000 0.000 0.988 0.004 0.008
#&gt; GSM28719     4  0.1117     0.8921 0.000 0.016 0.000 0.964 0.020
#&gt; GSM28708     4  0.1549     0.8885 0.000 0.016 0.000 0.944 0.040
#&gt; GSM28722     2  0.5889     0.0877 0.000 0.504 0.000 0.392 0.104
#&gt; GSM11232     4  0.2208     0.8892 0.000 0.020 0.000 0.908 0.072
#&gt; GSM28709     3  0.2997     0.9016 0.000 0.000 0.840 0.012 0.148
#&gt; GSM11226     4  0.3343     0.8650 0.000 0.016 0.000 0.812 0.172
#&gt; GSM11239     3  0.0451     0.9210 0.000 0.000 0.988 0.004 0.008
#&gt; GSM11225     3  0.0451     0.9210 0.000 0.000 0.988 0.004 0.008
#&gt; GSM11220     3  0.3039     0.9015 0.000 0.000 0.836 0.012 0.152
#&gt; GSM28701     4  0.3359     0.7701 0.000 0.020 0.000 0.816 0.164
#&gt; GSM28721     4  0.3492     0.8629 0.000 0.016 0.000 0.796 0.188
#&gt; GSM28713     2  0.4977    -0.0981 0.000 0.604 0.000 0.040 0.356
#&gt; GSM28716     5  0.4436     0.6012 0.000 0.396 0.000 0.008 0.596
#&gt; GSM11221     2  0.6047    -0.2467 0.000 0.500 0.000 0.124 0.376
#&gt; GSM28717     5  0.4300     0.5862 0.000 0.476 0.000 0.000 0.524
#&gt; GSM11223     1  0.1671     0.9526 0.924 0.000 0.000 0.000 0.076
#&gt; GSM11218     4  0.3412     0.8602 0.000 0.008 0.008 0.812 0.172
#&gt; GSM11219     2  0.0898     0.5966 0.000 0.972 0.000 0.008 0.020
#&gt; GSM11236     4  0.1469     0.8881 0.000 0.016 0.000 0.948 0.036
#&gt; GSM28702     3  0.2338     0.8722 0.000 0.000 0.884 0.004 0.112
#&gt; GSM28705     4  0.2777     0.8848 0.000 0.016 0.000 0.864 0.120
#&gt; GSM11230     2  0.4386     0.4052 0.000 0.764 0.000 0.140 0.096
#&gt; GSM28704     2  0.5954     0.1301 0.000 0.576 0.000 0.152 0.272
#&gt; GSM28700     2  0.4161    -0.3532 0.000 0.608 0.000 0.000 0.392
#&gt; GSM11224     2  0.2966     0.3875 0.000 0.816 0.000 0.000 0.184
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-5'>
<p><a id='tab-MAD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; GSM28710     5  0.6697      0.491 0.000 0.056 0.000 0.200 0.456 NA
#&gt; GSM28711     4  0.5978      0.296 0.000 0.016 0.000 0.532 0.200 NA
#&gt; GSM28712     2  0.2537      0.646 0.000 0.872 0.000 0.000 0.096 NA
#&gt; GSM11222     3  0.2442      0.839 0.000 0.000 0.852 0.000 0.004 NA
#&gt; GSM28720     1  0.0000      0.948 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM11217     1  0.0000      0.948 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM28723     1  0.0000      0.948 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM11241     1  0.2631      0.909 0.876 0.004 0.000 0.000 0.044 NA
#&gt; GSM28703     1  0.0000      0.948 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM11227     1  0.0000      0.948 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM28706     1  0.3307      0.871 0.820 0.000 0.000 0.000 0.072 NA
#&gt; GSM11229     1  0.0000      0.948 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM11235     1  0.0000      0.948 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM28707     1  0.2631      0.909 0.876 0.004 0.000 0.000 0.044 NA
#&gt; GSM11240     2  0.0405      0.731 0.000 0.988 0.000 0.004 0.008 NA
#&gt; GSM28714     2  0.0405      0.731 0.000 0.988 0.000 0.004 0.008 NA
#&gt; GSM11216     3  0.3408      0.867 0.000 0.000 0.800 0.000 0.048 NA
#&gt; GSM28715     2  0.0692      0.726 0.000 0.976 0.000 0.004 0.020 NA
#&gt; GSM11234     5  0.7342      0.543 0.000 0.188 0.000 0.152 0.404 NA
#&gt; GSM28699     5  0.2994      0.528 0.000 0.208 0.000 0.004 0.788 NA
#&gt; GSM11233     5  0.3499      0.374 0.000 0.320 0.000 0.000 0.680 NA
#&gt; GSM28718     2  0.0405      0.731 0.000 0.988 0.000 0.004 0.008 NA
#&gt; GSM11231     2  0.1536      0.690 0.000 0.940 0.000 0.040 0.004 NA
#&gt; GSM11237     2  0.0405      0.731 0.000 0.988 0.000 0.004 0.008 NA
#&gt; GSM11228     4  0.2994      0.705 0.000 0.000 0.000 0.788 0.004 NA
#&gt; GSM28697     4  0.3023      0.705 0.000 0.000 0.000 0.784 0.004 NA
#&gt; GSM28698     3  0.2554      0.887 0.000 0.000 0.876 0.000 0.048 NA
#&gt; GSM11238     3  0.0000      0.897 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM11242     3  0.0713      0.896 0.000 0.000 0.972 0.000 0.000 NA
#&gt; GSM28719     4  0.3468      0.697 0.000 0.000 0.000 0.728 0.008 NA
#&gt; GSM28708     4  0.3713      0.693 0.000 0.000 0.004 0.704 0.008 NA
#&gt; GSM28722     4  0.6445     -0.115 0.000 0.292 0.000 0.480 0.040 NA
#&gt; GSM11232     4  0.0622      0.706 0.000 0.000 0.000 0.980 0.012 NA
#&gt; GSM28709     3  0.3408      0.867 0.000 0.000 0.800 0.000 0.048 NA
#&gt; GSM11226     4  0.3011      0.682 0.000 0.000 0.004 0.800 0.004 NA
#&gt; GSM11239     3  0.0713      0.896 0.000 0.000 0.972 0.000 0.000 NA
#&gt; GSM11225     3  0.0713      0.896 0.000 0.000 0.972 0.000 0.000 NA
#&gt; GSM11220     3  0.3408      0.867 0.000 0.000 0.800 0.000 0.048 NA
#&gt; GSM28701     4  0.5688      0.442 0.000 0.004 0.000 0.472 0.140 NA
#&gt; GSM28721     4  0.2773      0.676 0.000 0.000 0.004 0.836 0.008 NA
#&gt; GSM28713     5  0.7463      0.434 0.000 0.284 0.000 0.140 0.348 NA
#&gt; GSM28716     5  0.5132      0.577 0.000 0.112 0.000 0.020 0.664 NA
#&gt; GSM11221     5  0.7432      0.512 0.000 0.212 0.000 0.152 0.380 NA
#&gt; GSM28717     5  0.2994      0.528 0.000 0.208 0.000 0.004 0.788 NA
#&gt; GSM11223     1  0.4008      0.842 0.768 0.004 0.000 0.000 0.100 NA
#&gt; GSM11218     4  0.3011      0.682 0.000 0.000 0.004 0.800 0.004 NA
#&gt; GSM11219     2  0.0858      0.724 0.000 0.968 0.000 0.004 0.028 NA
#&gt; GSM11236     4  0.4107      0.665 0.000 0.000 0.000 0.684 0.036 NA
#&gt; GSM28702     3  0.2442      0.839 0.000 0.000 0.852 0.000 0.004 NA
#&gt; GSM28705     4  0.1196      0.704 0.000 0.000 0.000 0.952 0.008 NA
#&gt; GSM11230     2  0.5992      0.214 0.000 0.560 0.000 0.260 0.036 NA
#&gt; GSM28704     2  0.7455     -0.223 0.000 0.340 0.000 0.284 0.132 NA
#&gt; GSM28700     2  0.5689     -0.248 0.000 0.468 0.000 0.008 0.400 NA
#&gt; GSM11224     2  0.4520      0.436 0.000 0.716 0.000 0.004 0.156 NA
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-kmeans-signature_compare](figure_cola/MAD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-kmeans-collect-classes](figure_cola/MAD-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> MAD:kmeans 38     0.378 2
#> MAD:kmeans 52     0.372 3
#> MAD:kmeans 53     0.353 4
#> MAD:kmeans 44     0.321 5
#> MAD:kmeans 44     0.321 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "skmeans"]
# you can also extract it by
# res = res_list["MAD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21452 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-skmeans-collect-plots](figure_cola/MAD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-skmeans-select-partition-number](figure_cola/MAD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.998       0.999         0.4928 0.508   0.508
#> 3 3 0.999           0.959       0.983         0.3188 0.715   0.499
#> 4 4 0.973           0.945       0.972         0.1414 0.871   0.646
#> 5 5 0.821           0.728       0.861         0.0792 0.914   0.681
#> 6 6 0.826           0.671       0.820         0.0357 0.924   0.652
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-skmeans-get-classes-1'>
<p><a id='tab-MAD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28710     2   0.000      0.998 0.000 1.000
#&gt; GSM28711     2   0.000      0.998 0.000 1.000
#&gt; GSM28712     2   0.000      0.998 0.000 1.000
#&gt; GSM11222     1   0.000      1.000 1.000 0.000
#&gt; GSM28720     1   0.000      1.000 1.000 0.000
#&gt; GSM11217     1   0.000      1.000 1.000 0.000
#&gt; GSM28723     1   0.000      1.000 1.000 0.000
#&gt; GSM11241     1   0.000      1.000 1.000 0.000
#&gt; GSM28703     1   0.000      1.000 1.000 0.000
#&gt; GSM11227     1   0.000      1.000 1.000 0.000
#&gt; GSM28706     1   0.000      1.000 1.000 0.000
#&gt; GSM11229     1   0.000      1.000 1.000 0.000
#&gt; GSM11235     1   0.000      1.000 1.000 0.000
#&gt; GSM28707     1   0.000      1.000 1.000 0.000
#&gt; GSM11240     2   0.000      0.998 0.000 1.000
#&gt; GSM28714     2   0.000      0.998 0.000 1.000
#&gt; GSM11216     1   0.000      1.000 1.000 0.000
#&gt; GSM28715     2   0.000      0.998 0.000 1.000
#&gt; GSM11234     2   0.000      0.998 0.000 1.000
#&gt; GSM28699     2   0.000      0.998 0.000 1.000
#&gt; GSM11233     2   0.000      0.998 0.000 1.000
#&gt; GSM28718     2   0.000      0.998 0.000 1.000
#&gt; GSM11231     2   0.000      0.998 0.000 1.000
#&gt; GSM11237     2   0.000      0.998 0.000 1.000
#&gt; GSM11228     2   0.000      0.998 0.000 1.000
#&gt; GSM28697     2   0.000      0.998 0.000 1.000
#&gt; GSM28698     1   0.000      1.000 1.000 0.000
#&gt; GSM11238     1   0.000      1.000 1.000 0.000
#&gt; GSM11242     1   0.000      1.000 1.000 0.000
#&gt; GSM28719     2   0.000      0.998 0.000 1.000
#&gt; GSM28708     2   0.224      0.963 0.036 0.964
#&gt; GSM28722     2   0.000      0.998 0.000 1.000
#&gt; GSM11232     2   0.000      0.998 0.000 1.000
#&gt; GSM28709     1   0.000      1.000 1.000 0.000
#&gt; GSM11226     2   0.000      0.998 0.000 1.000
#&gt; GSM11239     1   0.000      1.000 1.000 0.000
#&gt; GSM11225     1   0.000      1.000 1.000 0.000
#&gt; GSM11220     1   0.000      1.000 1.000 0.000
#&gt; GSM28701     2   0.000      0.998 0.000 1.000
#&gt; GSM28721     2   0.000      0.998 0.000 1.000
#&gt; GSM28713     2   0.000      0.998 0.000 1.000
#&gt; GSM28716     2   0.000      0.998 0.000 1.000
#&gt; GSM11221     2   0.000      0.998 0.000 1.000
#&gt; GSM28717     2   0.000      0.998 0.000 1.000
#&gt; GSM11223     1   0.000      1.000 1.000 0.000
#&gt; GSM11218     2   0.163      0.976 0.024 0.976
#&gt; GSM11219     2   0.000      0.998 0.000 1.000
#&gt; GSM11236     1   0.000      1.000 1.000 0.000
#&gt; GSM28702     1   0.000      1.000 1.000 0.000
#&gt; GSM28705     2   0.000      0.998 0.000 1.000
#&gt; GSM11230     2   0.000      0.998 0.000 1.000
#&gt; GSM28704     2   0.000      0.998 0.000 1.000
#&gt; GSM28700     2   0.000      0.998 0.000 1.000
#&gt; GSM11224     2   0.000      0.998 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-2'>
<p><a id='tab-MAD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1    p2    p3
#&gt; GSM28710     2   0.000      0.994  0 1.000 0.000
#&gt; GSM28711     2   0.000      0.994  0 1.000 0.000
#&gt; GSM28712     2   0.000      0.994  0 1.000 0.000
#&gt; GSM11222     3   0.000      0.942  0 0.000 1.000
#&gt; GSM28720     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11217     1   0.000      1.000  1 0.000 0.000
#&gt; GSM28723     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11241     1   0.000      1.000  1 0.000 0.000
#&gt; GSM28703     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11227     1   0.000      1.000  1 0.000 0.000
#&gt; GSM28706     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11229     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11235     1   0.000      1.000  1 0.000 0.000
#&gt; GSM28707     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11240     2   0.000      0.994  0 1.000 0.000
#&gt; GSM28714     2   0.000      0.994  0 1.000 0.000
#&gt; GSM11216     3   0.000      0.942  0 0.000 1.000
#&gt; GSM28715     2   0.000      0.994  0 1.000 0.000
#&gt; GSM11234     2   0.000      0.994  0 1.000 0.000
#&gt; GSM28699     2   0.000      0.994  0 1.000 0.000
#&gt; GSM11233     2   0.000      0.994  0 1.000 0.000
#&gt; GSM28718     2   0.000      0.994  0 1.000 0.000
#&gt; GSM11231     2   0.000      0.994  0 1.000 0.000
#&gt; GSM11237     2   0.000      0.994  0 1.000 0.000
#&gt; GSM11228     3   0.546      0.629  0 0.288 0.712
#&gt; GSM28697     2   0.186      0.942  0 0.948 0.052
#&gt; GSM28698     3   0.000      0.942  0 0.000 1.000
#&gt; GSM11238     3   0.000      0.942  0 0.000 1.000
#&gt; GSM11242     3   0.000      0.942  0 0.000 1.000
#&gt; GSM28719     3   0.581      0.537  0 0.336 0.664
#&gt; GSM28708     3   0.000      0.942  0 0.000 1.000
#&gt; GSM28722     2   0.000      0.994  0 1.000 0.000
#&gt; GSM11232     2   0.000      0.994  0 1.000 0.000
#&gt; GSM28709     3   0.000      0.942  0 0.000 1.000
#&gt; GSM11226     3   0.000      0.942  0 0.000 1.000
#&gt; GSM11239     3   0.000      0.942  0 0.000 1.000
#&gt; GSM11225     3   0.000      0.942  0 0.000 1.000
#&gt; GSM11220     3   0.000      0.942  0 0.000 1.000
#&gt; GSM28701     2   0.236      0.918  0 0.928 0.072
#&gt; GSM28721     3   0.000      0.942  0 0.000 1.000
#&gt; GSM28713     2   0.000      0.994  0 1.000 0.000
#&gt; GSM28716     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11221     2   0.000      0.994  0 1.000 0.000
#&gt; GSM28717     2   0.000      0.994  0 1.000 0.000
#&gt; GSM11223     1   0.000      1.000  1 0.000 0.000
#&gt; GSM11218     3   0.000      0.942  0 0.000 1.000
#&gt; GSM11219     2   0.000      0.994  0 1.000 0.000
#&gt; GSM11236     3   0.000      0.942  0 0.000 1.000
#&gt; GSM28702     3   0.000      0.942  0 0.000 1.000
#&gt; GSM28705     3   0.450      0.760  0 0.196 0.804
#&gt; GSM11230     2   0.000      0.994  0 1.000 0.000
#&gt; GSM28704     2   0.000      0.994  0 1.000 0.000
#&gt; GSM28700     2   0.000      0.994  0 1.000 0.000
#&gt; GSM11224     2   0.000      0.994  0 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-3'>
<p><a id='tab-MAD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28710     2  0.0707      0.971 0.000 0.980 0.000 0.020
#&gt; GSM28711     2  0.3942      0.667 0.000 0.764 0.000 0.236
#&gt; GSM28712     2  0.0000      0.977 0.000 1.000 0.000 0.000
#&gt; GSM11222     3  0.0000      0.998 0.000 0.000 1.000 0.000
#&gt; GSM28720     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.0336      0.977 0.000 0.992 0.000 0.008
#&gt; GSM28714     2  0.0336      0.977 0.000 0.992 0.000 0.008
#&gt; GSM11216     3  0.0000      0.998 0.000 0.000 1.000 0.000
#&gt; GSM28715     2  0.0336      0.977 0.000 0.992 0.000 0.008
#&gt; GSM11234     2  0.0817      0.970 0.000 0.976 0.000 0.024
#&gt; GSM28699     2  0.0469      0.974 0.000 0.988 0.000 0.012
#&gt; GSM11233     2  0.0469      0.974 0.000 0.988 0.000 0.012
#&gt; GSM28718     2  0.0336      0.977 0.000 0.992 0.000 0.008
#&gt; GSM11231     2  0.0707      0.971 0.000 0.980 0.000 0.020
#&gt; GSM11237     2  0.0336      0.977 0.000 0.992 0.000 0.008
#&gt; GSM11228     4  0.0469      0.892 0.000 0.000 0.012 0.988
#&gt; GSM28697     4  0.0469      0.888 0.000 0.012 0.000 0.988
#&gt; GSM28698     3  0.0000      0.998 0.000 0.000 1.000 0.000
#&gt; GSM11238     3  0.0000      0.998 0.000 0.000 1.000 0.000
#&gt; GSM11242     3  0.0000      0.998 0.000 0.000 1.000 0.000
#&gt; GSM28719     4  0.0469      0.892 0.000 0.000 0.012 0.988
#&gt; GSM28708     4  0.0707      0.891 0.000 0.000 0.020 0.980
#&gt; GSM28722     4  0.4888      0.320 0.000 0.412 0.000 0.588
#&gt; GSM11232     4  0.0469      0.888 0.000 0.012 0.000 0.988
#&gt; GSM28709     3  0.0000      0.998 0.000 0.000 1.000 0.000
#&gt; GSM11226     4  0.2647      0.830 0.000 0.000 0.120 0.880
#&gt; GSM11239     3  0.0000      0.998 0.000 0.000 1.000 0.000
#&gt; GSM11225     3  0.0000      0.998 0.000 0.000 1.000 0.000
#&gt; GSM11220     3  0.0000      0.998 0.000 0.000 1.000 0.000
#&gt; GSM28701     4  0.3688      0.714 0.000 0.208 0.000 0.792
#&gt; GSM28721     4  0.0817      0.891 0.000 0.000 0.024 0.976
#&gt; GSM28713     2  0.0188      0.976 0.000 0.996 0.000 0.004
#&gt; GSM28716     1  0.0657      0.985 0.984 0.004 0.000 0.012
#&gt; GSM11221     2  0.0336      0.976 0.000 0.992 0.000 0.008
#&gt; GSM28717     2  0.0469      0.974 0.000 0.988 0.000 0.012
#&gt; GSM11223     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; GSM11218     4  0.3172      0.790 0.000 0.000 0.160 0.840
#&gt; GSM11219     2  0.0336      0.977 0.000 0.992 0.000 0.008
#&gt; GSM11236     3  0.0707      0.979 0.000 0.000 0.980 0.020
#&gt; GSM28702     3  0.0000      0.998 0.000 0.000 1.000 0.000
#&gt; GSM28705     4  0.0817      0.891 0.000 0.000 0.024 0.976
#&gt; GSM11230     2  0.0469      0.976 0.000 0.988 0.000 0.012
#&gt; GSM28704     2  0.0707      0.971 0.000 0.980 0.000 0.020
#&gt; GSM28700     2  0.0469      0.974 0.000 0.988 0.000 0.012
#&gt; GSM11224     2  0.0000      0.977 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-4'>
<p><a id='tab-MAD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28710     5  0.2813     0.5210 0.000 0.108 0.000 0.024 0.868
#&gt; GSM28711     5  0.5575     0.3951 0.000 0.280 0.000 0.108 0.612
#&gt; GSM28712     2  0.2377     0.6687 0.000 0.872 0.000 0.000 0.128
#&gt; GSM11222     3  0.0000     0.9939 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28720     1  0.0000     0.9624 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000     0.9624 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000     0.9624 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000     0.9624 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000     0.9624 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000     0.9624 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000     0.9624 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000     0.9624 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000     0.9624 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000     0.9624 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.0000     0.7507 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28714     2  0.0000     0.7507 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11216     3  0.0000     0.9939 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28715     2  0.0000     0.7507 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11234     5  0.4774     0.0582 0.000 0.424 0.000 0.020 0.556
#&gt; GSM28699     5  0.3816     0.4600 0.000 0.304 0.000 0.000 0.696
#&gt; GSM11233     2  0.4297    -0.1659 0.000 0.528 0.000 0.000 0.472
#&gt; GSM28718     2  0.0000     0.7507 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11231     2  0.0609     0.7384 0.000 0.980 0.000 0.020 0.000
#&gt; GSM11237     2  0.0000     0.7507 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11228     4  0.1197     0.8614 0.000 0.000 0.000 0.952 0.048
#&gt; GSM28697     4  0.1270     0.8604 0.000 0.000 0.000 0.948 0.052
#&gt; GSM28698     3  0.0000     0.9939 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11238     3  0.0000     0.9939 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11242     3  0.0000     0.9939 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28719     4  0.1608     0.8526 0.000 0.000 0.000 0.928 0.072
#&gt; GSM28708     4  0.1478     0.8563 0.000 0.000 0.000 0.936 0.064
#&gt; GSM28722     2  0.5804     0.2494 0.000 0.576 0.000 0.304 0.120
#&gt; GSM11232     4  0.1981     0.8437 0.000 0.016 0.000 0.920 0.064
#&gt; GSM28709     3  0.0000     0.9939 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11226     4  0.4199     0.7423 0.000 0.000 0.180 0.764 0.056
#&gt; GSM11239     3  0.0000     0.9939 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11225     3  0.0000     0.9939 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11220     3  0.0000     0.9939 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28701     5  0.4748    -0.2019 0.000 0.016 0.000 0.492 0.492
#&gt; GSM28721     4  0.2236     0.8523 0.000 0.000 0.024 0.908 0.068
#&gt; GSM28713     2  0.4451     0.3975 0.000 0.644 0.000 0.016 0.340
#&gt; GSM28716     1  0.4171     0.3868 0.604 0.000 0.000 0.000 0.396
#&gt; GSM11221     5  0.4644     0.0148 0.000 0.460 0.000 0.012 0.528
#&gt; GSM28717     5  0.3816     0.4600 0.000 0.304 0.000 0.000 0.696
#&gt; GSM11223     1  0.0000     0.9624 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     4  0.4477     0.6699 0.000 0.000 0.252 0.708 0.040
#&gt; GSM11219     2  0.0290     0.7491 0.000 0.992 0.000 0.000 0.008
#&gt; GSM11236     3  0.1557     0.9363 0.000 0.000 0.940 0.052 0.008
#&gt; GSM28702     3  0.0000     0.9939 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28705     4  0.1956     0.8541 0.000 0.000 0.008 0.916 0.076
#&gt; GSM11230     2  0.3197     0.6256 0.000 0.836 0.000 0.024 0.140
#&gt; GSM28704     2  0.4269     0.5383 0.000 0.732 0.000 0.036 0.232
#&gt; GSM28700     2  0.4210     0.1091 0.000 0.588 0.000 0.000 0.412
#&gt; GSM11224     2  0.2329     0.6826 0.000 0.876 0.000 0.000 0.124
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-5'>
<p><a id='tab-MAD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28710     5  0.4733     0.3940 0.000 0.072 0.000 0.276 0.648 0.004
#&gt; GSM28711     4  0.7141    -0.1411 0.000 0.136 0.000 0.408 0.312 0.144
#&gt; GSM28712     2  0.3284     0.6108 0.000 0.800 0.000 0.032 0.168 0.000
#&gt; GSM11222     3  0.1387     0.9230 0.000 0.000 0.932 0.000 0.000 0.068
#&gt; GSM28720     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.0260     0.7731 0.000 0.992 0.000 0.000 0.008 0.000
#&gt; GSM28714     2  0.0260     0.7731 0.000 0.992 0.000 0.000 0.008 0.000
#&gt; GSM11216     3  0.0146     0.9659 0.000 0.000 0.996 0.004 0.000 0.000
#&gt; GSM28715     2  0.0363     0.7718 0.000 0.988 0.000 0.000 0.012 0.000
#&gt; GSM11234     5  0.6145     0.3715 0.000 0.216 0.000 0.136 0.580 0.068
#&gt; GSM28699     5  0.2704     0.5818 0.000 0.140 0.000 0.016 0.844 0.000
#&gt; GSM11233     5  0.3695     0.3876 0.000 0.376 0.000 0.000 0.624 0.000
#&gt; GSM28718     2  0.0260     0.7731 0.000 0.992 0.000 0.000 0.008 0.000
#&gt; GSM11231     2  0.1349     0.7378 0.000 0.940 0.000 0.056 0.004 0.000
#&gt; GSM11237     2  0.0260     0.7731 0.000 0.992 0.000 0.000 0.008 0.000
#&gt; GSM11228     4  0.4169     0.4905 0.000 0.000 0.000 0.532 0.012 0.456
#&gt; GSM28697     4  0.4305     0.4928 0.000 0.000 0.000 0.544 0.020 0.436
#&gt; GSM28698     3  0.0000     0.9665 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11238     3  0.0000     0.9665 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11242     3  0.0000     0.9665 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28719     4  0.3706     0.5366 0.000 0.000 0.000 0.620 0.000 0.380
#&gt; GSM28708     4  0.4045     0.5171 0.000 0.000 0.008 0.564 0.000 0.428
#&gt; GSM28722     6  0.7284    -0.1122 0.000 0.332 0.000 0.192 0.120 0.356
#&gt; GSM11232     6  0.4887     0.2815 0.000 0.052 0.000 0.180 0.060 0.708
#&gt; GSM28709     3  0.0146     0.9659 0.000 0.000 0.996 0.004 0.000 0.000
#&gt; GSM11226     6  0.2213     0.5351 0.000 0.000 0.100 0.008 0.004 0.888
#&gt; GSM11239     3  0.0000     0.9665 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11225     3  0.0000     0.9665 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11220     3  0.0146     0.9659 0.000 0.000 0.996 0.004 0.000 0.000
#&gt; GSM28701     4  0.3694     0.3691 0.000 0.016 0.000 0.804 0.124 0.056
#&gt; GSM28721     6  0.0146     0.5437 0.000 0.000 0.004 0.000 0.000 0.996
#&gt; GSM28713     2  0.6647     0.0191 0.000 0.420 0.000 0.148 0.368 0.064
#&gt; GSM28716     5  0.3774     0.2134 0.408 0.000 0.000 0.000 0.592 0.000
#&gt; GSM11221     5  0.6688     0.1293 0.000 0.280 0.000 0.240 0.436 0.044
#&gt; GSM28717     5  0.2704     0.5818 0.000 0.140 0.000 0.016 0.844 0.000
#&gt; GSM11223     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     6  0.2703     0.4677 0.000 0.000 0.172 0.004 0.000 0.824
#&gt; GSM11219     2  0.1245     0.7620 0.000 0.952 0.000 0.016 0.032 0.000
#&gt; GSM11236     3  0.3290     0.8032 0.000 0.000 0.820 0.132 0.004 0.044
#&gt; GSM28702     3  0.1501     0.9170 0.000 0.000 0.924 0.000 0.000 0.076
#&gt; GSM28705     6  0.0806     0.5275 0.000 0.000 0.000 0.020 0.008 0.972
#&gt; GSM11230     2  0.4843     0.5712 0.000 0.732 0.000 0.108 0.100 0.060
#&gt; GSM28704     2  0.6844     0.3196 0.000 0.508 0.000 0.204 0.148 0.140
#&gt; GSM28700     5  0.5064     0.2540 0.000 0.392 0.000 0.060 0.540 0.008
#&gt; GSM11224     2  0.4257     0.5549 0.000 0.728 0.000 0.060 0.204 0.008
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-skmeans-signature_compare](figure_cola/MAD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-skmeans-collect-classes](figure_cola/MAD-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>              n tissue(p) k
#> MAD:skmeans 54     0.398 2
#> MAD:skmeans 54     0.374 3
#> MAD:skmeans 53     0.353 4
#> MAD:skmeans 43     0.319 5
#> MAD:skmeans 39     0.293 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:pam






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "pam"]
# you can also extract it by
# res = res_list["MAD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21452 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-pam-collect-plots](figure_cola/MAD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-pam-select-partition-number](figure_cola/MAD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.762           0.942       0.972         0.4573 0.535   0.535
#> 3 3 0.866           0.891       0.958         0.3331 0.770   0.596
#> 4 4 0.892           0.902       0.944         0.1848 0.865   0.658
#> 5 5 0.830           0.735       0.878         0.0855 0.864   0.560
#> 6 6 0.844           0.704       0.856         0.0306 0.897   0.607
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-classes'>
<ul>
<li><a href='#tab-MAD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-pam-get-classes-1'>
<p><a id='tab-MAD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28710     2  0.0000     0.9816 0.000 1.000
#&gt; GSM28711     2  0.0000     0.9816 0.000 1.000
#&gt; GSM28712     2  0.0000     0.9816 0.000 1.000
#&gt; GSM11222     2  0.0000     0.9816 0.000 1.000
#&gt; GSM28720     1  0.0000     0.9436 1.000 0.000
#&gt; GSM11217     1  0.0000     0.9436 1.000 0.000
#&gt; GSM28723     1  0.0000     0.9436 1.000 0.000
#&gt; GSM11241     1  0.0000     0.9436 1.000 0.000
#&gt; GSM28703     1  0.0000     0.9436 1.000 0.000
#&gt; GSM11227     1  0.0000     0.9436 1.000 0.000
#&gt; GSM28706     1  0.0000     0.9436 1.000 0.000
#&gt; GSM11229     1  0.0000     0.9436 1.000 0.000
#&gt; GSM11235     1  0.0000     0.9436 1.000 0.000
#&gt; GSM28707     1  0.0000     0.9436 1.000 0.000
#&gt; GSM11240     2  0.0000     0.9816 0.000 1.000
#&gt; GSM28714     2  0.0000     0.9816 0.000 1.000
#&gt; GSM11216     1  0.5737     0.8925 0.864 0.136
#&gt; GSM28715     2  0.0000     0.9816 0.000 1.000
#&gt; GSM11234     2  0.0000     0.9816 0.000 1.000
#&gt; GSM28699     2  0.5629     0.8318 0.132 0.868
#&gt; GSM11233     2  0.0000     0.9816 0.000 1.000
#&gt; GSM28718     2  0.0000     0.9816 0.000 1.000
#&gt; GSM11231     2  0.0000     0.9816 0.000 1.000
#&gt; GSM11237     2  0.0000     0.9816 0.000 1.000
#&gt; GSM11228     2  0.0000     0.9816 0.000 1.000
#&gt; GSM28697     2  0.0000     0.9816 0.000 1.000
#&gt; GSM28698     1  0.5737     0.8925 0.864 0.136
#&gt; GSM11238     1  0.5737     0.8925 0.864 0.136
#&gt; GSM11242     1  0.5737     0.8925 0.864 0.136
#&gt; GSM28719     2  0.0000     0.9816 0.000 1.000
#&gt; GSM28708     2  0.0000     0.9816 0.000 1.000
#&gt; GSM28722     2  0.0000     0.9816 0.000 1.000
#&gt; GSM11232     2  0.0000     0.9816 0.000 1.000
#&gt; GSM28709     2  0.9909     0.0912 0.444 0.556
#&gt; GSM11226     2  0.0000     0.9816 0.000 1.000
#&gt; GSM11239     1  0.5737     0.8925 0.864 0.136
#&gt; GSM11225     1  0.5737     0.8925 0.864 0.136
#&gt; GSM11220     1  0.5737     0.8925 0.864 0.136
#&gt; GSM28701     2  0.0000     0.9816 0.000 1.000
#&gt; GSM28721     2  0.0000     0.9816 0.000 1.000
#&gt; GSM28713     2  0.0000     0.9816 0.000 1.000
#&gt; GSM28716     1  0.0376     0.9420 0.996 0.004
#&gt; GSM11221     2  0.0000     0.9816 0.000 1.000
#&gt; GSM28717     2  0.0000     0.9816 0.000 1.000
#&gt; GSM11223     1  0.0000     0.9436 1.000 0.000
#&gt; GSM11218     2  0.0000     0.9816 0.000 1.000
#&gt; GSM11219     2  0.0000     0.9816 0.000 1.000
#&gt; GSM11236     2  0.0000     0.9816 0.000 1.000
#&gt; GSM28702     2  0.0000     0.9816 0.000 1.000
#&gt; GSM28705     2  0.0000     0.9816 0.000 1.000
#&gt; GSM11230     2  0.0000     0.9816 0.000 1.000
#&gt; GSM28704     2  0.0000     0.9816 0.000 1.000
#&gt; GSM28700     2  0.0000     0.9816 0.000 1.000
#&gt; GSM11224     2  0.0000     0.9816 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-1-a').click(function(){
  $('#tab-MAD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-2'>
<p><a id='tab-MAD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette   p1    p2    p3
#&gt; GSM28710     2   0.000      0.975 0.00 1.000 0.000
#&gt; GSM28711     2   0.000      0.975 0.00 1.000 0.000
#&gt; GSM28712     2   0.000      0.975 0.00 1.000 0.000
#&gt; GSM11222     3   0.000      0.829 0.00 0.000 1.000
#&gt; GSM28720     1   0.000      0.993 1.00 0.000 0.000
#&gt; GSM11217     1   0.000      0.993 1.00 0.000 0.000
#&gt; GSM28723     1   0.000      0.993 1.00 0.000 0.000
#&gt; GSM11241     1   0.000      0.993 1.00 0.000 0.000
#&gt; GSM28703     1   0.000      0.993 1.00 0.000 0.000
#&gt; GSM11227     1   0.000      0.993 1.00 0.000 0.000
#&gt; GSM28706     1   0.000      0.993 1.00 0.000 0.000
#&gt; GSM11229     1   0.000      0.993 1.00 0.000 0.000
#&gt; GSM11235     1   0.000      0.993 1.00 0.000 0.000
#&gt; GSM28707     1   0.000      0.993 1.00 0.000 0.000
#&gt; GSM11240     2   0.000      0.975 0.00 1.000 0.000
#&gt; GSM28714     2   0.000      0.975 0.00 1.000 0.000
#&gt; GSM11216     3   0.000      0.829 0.00 0.000 1.000
#&gt; GSM28715     2   0.000      0.975 0.00 1.000 0.000
#&gt; GSM11234     2   0.000      0.975 0.00 1.000 0.000
#&gt; GSM28699     2   0.000      0.975 0.00 1.000 0.000
#&gt; GSM11233     2   0.000      0.975 0.00 1.000 0.000
#&gt; GSM28718     2   0.000      0.975 0.00 1.000 0.000
#&gt; GSM11231     2   0.000      0.975 0.00 1.000 0.000
#&gt; GSM11237     2   0.000      0.975 0.00 1.000 0.000
#&gt; GSM11228     2   0.445      0.708 0.00 0.808 0.192
#&gt; GSM28697     2   0.000      0.975 0.00 1.000 0.000
#&gt; GSM28698     3   0.000      0.829 0.00 0.000 1.000
#&gt; GSM11238     3   0.000      0.829 0.00 0.000 1.000
#&gt; GSM11242     3   0.000      0.829 0.00 0.000 1.000
#&gt; GSM28719     2   0.000      0.975 0.00 1.000 0.000
#&gt; GSM28708     3   0.619      0.409 0.00 0.420 0.580
#&gt; GSM28722     2   0.000      0.975 0.00 1.000 0.000
#&gt; GSM11232     2   0.000      0.975 0.00 1.000 0.000
#&gt; GSM28709     3   0.000      0.829 0.00 0.000 1.000
#&gt; GSM11226     3   0.619      0.409 0.00 0.420 0.580
#&gt; GSM11239     3   0.000      0.829 0.00 0.000 1.000
#&gt; GSM11225     3   0.000      0.829 0.00 0.000 1.000
#&gt; GSM11220     3   0.000      0.829 0.00 0.000 1.000
#&gt; GSM28701     2   0.000      0.975 0.00 1.000 0.000
#&gt; GSM28721     3   0.619      0.409 0.00 0.420 0.580
#&gt; GSM28713     2   0.000      0.975 0.00 1.000 0.000
#&gt; GSM28716     1   0.207      0.916 0.94 0.060 0.000
#&gt; GSM11221     2   0.000      0.975 0.00 1.000 0.000
#&gt; GSM28717     2   0.000      0.975 0.00 1.000 0.000
#&gt; GSM11223     1   0.000      0.993 1.00 0.000 0.000
#&gt; GSM11218     3   0.613      0.447 0.00 0.400 0.600
#&gt; GSM11219     2   0.000      0.975 0.00 1.000 0.000
#&gt; GSM11236     2   0.000      0.975 0.00 1.000 0.000
#&gt; GSM28702     3   0.000      0.829 0.00 0.000 1.000
#&gt; GSM28705     2   0.599      0.277 0.00 0.632 0.368
#&gt; GSM11230     2   0.000      0.975 0.00 1.000 0.000
#&gt; GSM28704     2   0.000      0.975 0.00 1.000 0.000
#&gt; GSM28700     2   0.000      0.975 0.00 1.000 0.000
#&gt; GSM11224     2   0.000      0.975 0.00 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-2-a').click(function(){
  $('#tab-MAD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-3'>
<p><a id='tab-MAD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28710     2  0.1716      0.917 0.000 0.936 0.000 0.064
#&gt; GSM28711     4  0.2408      0.806 0.000 0.104 0.000 0.896
#&gt; GSM28712     2  0.0469      0.934 0.000 0.988 0.000 0.012
#&gt; GSM11222     4  0.4277      0.616 0.000 0.000 0.280 0.720
#&gt; GSM28720     1  0.0000      0.990 1.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      0.990 1.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      0.990 1.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      0.990 1.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      0.990 1.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      0.990 1.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      0.990 1.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      0.990 1.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      0.990 1.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      0.990 1.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.1118      0.926 0.000 0.964 0.000 0.036
#&gt; GSM28714     2  0.1118      0.926 0.000 0.964 0.000 0.036
#&gt; GSM11216     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28715     2  0.1118      0.926 0.000 0.964 0.000 0.036
#&gt; GSM11234     2  0.2868      0.869 0.000 0.864 0.000 0.136
#&gt; GSM28699     2  0.0817      0.932 0.000 0.976 0.000 0.024
#&gt; GSM11233     2  0.1118      0.926 0.000 0.964 0.000 0.036
#&gt; GSM28718     2  0.1118      0.926 0.000 0.964 0.000 0.036
#&gt; GSM11231     2  0.1118      0.926 0.000 0.964 0.000 0.036
#&gt; GSM11237     2  0.1118      0.926 0.000 0.964 0.000 0.036
#&gt; GSM11228     4  0.1302      0.847 0.000 0.044 0.000 0.956
#&gt; GSM28697     4  0.4948      0.042 0.000 0.440 0.000 0.560
#&gt; GSM28698     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11238     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11242     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28719     4  0.1118      0.850 0.000 0.036 0.000 0.964
#&gt; GSM28708     4  0.1474      0.853 0.000 0.000 0.052 0.948
#&gt; GSM28722     2  0.3311      0.836 0.000 0.828 0.000 0.172
#&gt; GSM11232     2  0.3528      0.816 0.000 0.808 0.000 0.192
#&gt; GSM28709     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11226     4  0.1807      0.856 0.000 0.008 0.052 0.940
#&gt; GSM11239     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11225     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11220     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28701     2  0.2345      0.897 0.000 0.900 0.000 0.100
#&gt; GSM28721     4  0.1807      0.856 0.000 0.008 0.052 0.940
#&gt; GSM28713     2  0.0817      0.932 0.000 0.976 0.000 0.024
#&gt; GSM28716     1  0.2670      0.882 0.904 0.072 0.000 0.024
#&gt; GSM11221     2  0.0817      0.932 0.000 0.976 0.000 0.024
#&gt; GSM28717     2  0.0469      0.934 0.000 0.988 0.000 0.012
#&gt; GSM11223     1  0.0000      0.990 1.000 0.000 0.000 0.000
#&gt; GSM11218     4  0.1807      0.856 0.000 0.008 0.052 0.940
#&gt; GSM11219     2  0.0469      0.934 0.000 0.988 0.000 0.012
#&gt; GSM11236     2  0.5195      0.656 0.000 0.692 0.032 0.276
#&gt; GSM28702     4  0.3311      0.755 0.000 0.000 0.172 0.828
#&gt; GSM28705     4  0.1118      0.850 0.000 0.036 0.000 0.964
#&gt; GSM11230     2  0.2081      0.914 0.000 0.916 0.000 0.084
#&gt; GSM28704     2  0.2345      0.897 0.000 0.900 0.000 0.100
#&gt; GSM28700     2  0.0469      0.934 0.000 0.988 0.000 0.012
#&gt; GSM11224     2  0.0469      0.934 0.000 0.988 0.000 0.012
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-3-a').click(function(){
  $('#tab-MAD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-4'>
<p><a id='tab-MAD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28710     2  0.0000      0.711 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28711     4  0.0880      0.871 0.000 0.032 0.000 0.968 0.000
#&gt; GSM28712     2  0.1043      0.690 0.000 0.960 0.000 0.000 0.040
#&gt; GSM11222     4  0.3452      0.628 0.000 0.000 0.244 0.756 0.000
#&gt; GSM28720     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     5  0.4304      0.673 0.000 0.484 0.000 0.000 0.516
#&gt; GSM28714     5  0.4304      0.673 0.000 0.484 0.000 0.000 0.516
#&gt; GSM11216     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28715     5  0.4304      0.673 0.000 0.484 0.000 0.000 0.516
#&gt; GSM11234     2  0.0290      0.710 0.000 0.992 0.000 0.008 0.000
#&gt; GSM28699     2  0.4304      0.162 0.000 0.516 0.000 0.000 0.484
#&gt; GSM11233     5  0.0162      0.302 0.000 0.004 0.000 0.000 0.996
#&gt; GSM28718     5  0.4304      0.673 0.000 0.484 0.000 0.000 0.516
#&gt; GSM11231     5  0.4304      0.673 0.000 0.484 0.000 0.000 0.516
#&gt; GSM11237     5  0.4297      0.666 0.000 0.472 0.000 0.000 0.528
#&gt; GSM11228     4  0.3395      0.622 0.000 0.236 0.000 0.764 0.000
#&gt; GSM28697     2  0.5406      0.236 0.000 0.572 0.000 0.360 0.068
#&gt; GSM28698     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11238     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11242     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28719     4  0.0290      0.882 0.000 0.008 0.000 0.992 0.000
#&gt; GSM28708     4  0.0000      0.885 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28722     2  0.1197      0.686 0.000 0.952 0.000 0.048 0.000
#&gt; GSM11232     2  0.3366      0.418 0.000 0.768 0.000 0.232 0.000
#&gt; GSM28709     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11226     4  0.0000      0.885 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11239     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11225     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11220     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28701     2  0.0162      0.710 0.000 0.996 0.000 0.004 0.000
#&gt; GSM28721     4  0.0000      0.885 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28713     2  0.0000      0.711 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28716     2  0.6229      0.180 0.164 0.516 0.000 0.000 0.320
#&gt; GSM11221     2  0.0000      0.711 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28717     5  0.4150     -0.129 0.000 0.388 0.000 0.000 0.612
#&gt; GSM11223     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     4  0.0000      0.885 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11219     2  0.1270      0.677 0.000 0.948 0.000 0.000 0.052
#&gt; GSM11236     4  0.4192      0.165 0.000 0.404 0.000 0.596 0.000
#&gt; GSM28702     4  0.0794      0.870 0.000 0.000 0.028 0.972 0.000
#&gt; GSM28705     4  0.0000      0.885 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11230     2  0.4305     -0.690 0.000 0.512 0.000 0.000 0.488
#&gt; GSM28704     2  0.0000      0.711 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28700     2  0.0794      0.699 0.000 0.972 0.000 0.000 0.028
#&gt; GSM11224     2  0.1043      0.690 0.000 0.960 0.000 0.000 0.040
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-4-a').click(function(){
  $('#tab-MAD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-5'>
<p><a id='tab-MAD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette   p1    p2    p3    p4    p5    p6
#&gt; GSM28710     2  0.0260     0.6428 0.00 0.992 0.000 0.000 0.008 0.000
#&gt; GSM28711     6  0.0260     0.7828 0.00 0.008 0.000 0.000 0.000 0.992
#&gt; GSM28712     2  0.0790     0.6482 0.00 0.968 0.000 0.000 0.032 0.000
#&gt; GSM11222     6  0.3101     0.5342 0.00 0.000 0.244 0.000 0.000 0.756
#&gt; GSM28720     1  0.0000     1.0000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000     1.0000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000     1.0000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000     1.0000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000     1.0000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000     1.0000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000     1.0000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000     1.0000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000     1.0000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000     1.0000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.5747     0.4767 0.00 0.500 0.000 0.200 0.300 0.000
#&gt; GSM28714     2  0.5861     0.4317 0.00 0.444 0.000 0.200 0.356 0.000
#&gt; GSM11216     3  0.0146     0.9969 0.00 0.000 0.996 0.000 0.000 0.004
#&gt; GSM28715     2  0.5747     0.4767 0.00 0.500 0.000 0.200 0.300 0.000
#&gt; GSM11234     2  0.0260     0.6428 0.00 0.992 0.000 0.000 0.008 0.000
#&gt; GSM28699     5  0.3634     0.5537 0.00 0.356 0.000 0.000 0.644 0.000
#&gt; GSM11233     5  0.0405     0.5139 0.00 0.004 0.000 0.008 0.988 0.000
#&gt; GSM28718     2  0.5861     0.4317 0.00 0.444 0.000 0.200 0.356 0.000
#&gt; GSM11231     2  0.5861     0.4317 0.00 0.444 0.000 0.200 0.356 0.000
#&gt; GSM11237     2  0.5873     0.4149 0.00 0.432 0.000 0.200 0.368 0.000
#&gt; GSM11228     4  0.3588     0.7051 0.00 0.152 0.000 0.788 0.000 0.060
#&gt; GSM28697     4  0.3428     0.5642 0.00 0.304 0.000 0.696 0.000 0.000
#&gt; GSM28698     3  0.0000     0.9982 0.00 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11238     3  0.0146     0.9969 0.00 0.000 0.996 0.000 0.000 0.004
#&gt; GSM11242     3  0.0000     0.9982 0.00 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28719     4  0.2823     0.5514 0.00 0.000 0.000 0.796 0.000 0.204
#&gt; GSM28708     6  0.3823     0.0906 0.00 0.000 0.000 0.436 0.000 0.564
#&gt; GSM28722     2  0.1434     0.6409 0.00 0.940 0.000 0.048 0.000 0.012
#&gt; GSM11232     2  0.3835     0.5178 0.00 0.748 0.000 0.048 0.000 0.204
#&gt; GSM28709     3  0.0146     0.9969 0.00 0.000 0.996 0.000 0.000 0.004
#&gt; GSM11226     6  0.0000     0.7878 0.00 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11239     3  0.0000     0.9982 0.00 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11225     3  0.0000     0.9982 0.00 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11220     3  0.0000     0.9982 0.00 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28701     2  0.3288     0.4677 0.00 0.724 0.000 0.276 0.000 0.000
#&gt; GSM28721     6  0.0000     0.7878 0.00 0.000 0.000 0.000 0.000 1.000
#&gt; GSM28713     2  0.0146     0.6453 0.00 0.996 0.000 0.004 0.000 0.000
#&gt; GSM28716     2  0.5808    -0.3704 0.22 0.492 0.000 0.000 0.288 0.000
#&gt; GSM11221     2  0.0603     0.6505 0.00 0.980 0.000 0.004 0.016 0.000
#&gt; GSM28717     5  0.2941     0.6760 0.00 0.220 0.000 0.000 0.780 0.000
#&gt; GSM11223     1  0.0000     1.0000 1.00 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     6  0.0000     0.7878 0.00 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11219     2  0.1584     0.6500 0.00 0.928 0.000 0.008 0.064 0.000
#&gt; GSM11236     6  0.4695    -0.0715 0.00 0.448 0.000 0.044 0.000 0.508
#&gt; GSM28702     6  0.0000     0.7878 0.00 0.000 0.000 0.000 0.000 1.000
#&gt; GSM28705     6  0.0146     0.7855 0.00 0.000 0.000 0.004 0.000 0.996
#&gt; GSM11230     2  0.5662     0.4789 0.00 0.524 0.000 0.196 0.280 0.000
#&gt; GSM28704     2  0.0937     0.6436 0.00 0.960 0.000 0.040 0.000 0.000
#&gt; GSM28700     2  0.0632     0.6483 0.00 0.976 0.000 0.000 0.024 0.000
#&gt; GSM11224     2  0.0713     0.6489 0.00 0.972 0.000 0.000 0.028 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-5-a').click(function(){
  $('#tab-MAD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-pam-signature_compare](figure_cola/MAD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-pam-collect-classes](figure_cola/MAD-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n tissue(p) k
#> MAD:pam 53     0.397 2
#> MAD:pam 49     0.368 3
#> MAD:pam 53     0.353 4
#> MAD:pam 46     0.324 5
#> MAD:pam 43     0.392 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "mclust"]
# you can also extract it by
# res = res_list["MAD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21452 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-mclust-collect-plots](figure_cola/MAD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-mclust-select-partition-number](figure_cola/MAD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.523           0.898       0.938         0.3562 0.669   0.669
#> 3 3 1.000           0.951       0.980         0.5248 0.804   0.708
#> 4 4 0.819           0.780       0.890         0.3234 0.795   0.566
#> 5 5 0.749           0.714       0.844         0.0717 0.905   0.674
#> 6 6 0.818           0.784       0.812         0.0489 0.929   0.702
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-classes'>
<ul>
<li><a href='#tab-MAD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-mclust-get-classes-1'>
<p><a id='tab-MAD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28710     2  0.7056      0.831 0.192 0.808
#&gt; GSM28711     2  0.0000      0.920 0.000 1.000
#&gt; GSM28712     2  0.7056      0.831 0.192 0.808
#&gt; GSM11222     2  0.0000      0.920 0.000 1.000
#&gt; GSM28720     1  0.0000      0.953 1.000 0.000
#&gt; GSM11217     1  0.0000      0.953 1.000 0.000
#&gt; GSM28723     1  0.4690      0.909 0.900 0.100
#&gt; GSM11241     1  0.4690      0.909 0.900 0.100
#&gt; GSM28703     1  0.0000      0.953 1.000 0.000
#&gt; GSM11227     1  0.0000      0.953 1.000 0.000
#&gt; GSM28706     1  0.0000      0.953 1.000 0.000
#&gt; GSM11229     1  0.0000      0.953 1.000 0.000
#&gt; GSM11235     1  0.0000      0.953 1.000 0.000
#&gt; GSM28707     1  0.4690      0.909 0.900 0.100
#&gt; GSM11240     2  0.7056      0.831 0.192 0.808
#&gt; GSM28714     2  0.7056      0.831 0.192 0.808
#&gt; GSM11216     2  0.0000      0.920 0.000 1.000
#&gt; GSM28715     2  0.7056      0.831 0.192 0.808
#&gt; GSM11234     2  0.0376      0.918 0.004 0.996
#&gt; GSM28699     2  0.7139      0.827 0.196 0.804
#&gt; GSM11233     2  0.7056      0.831 0.192 0.808
#&gt; GSM28718     2  0.7056      0.831 0.192 0.808
#&gt; GSM11231     2  0.7056      0.831 0.192 0.808
#&gt; GSM11237     2  0.7056      0.831 0.192 0.808
#&gt; GSM11228     2  0.0000      0.920 0.000 1.000
#&gt; GSM28697     2  0.0000      0.920 0.000 1.000
#&gt; GSM28698     2  0.0000      0.920 0.000 1.000
#&gt; GSM11238     2  0.0000      0.920 0.000 1.000
#&gt; GSM11242     2  0.0000      0.920 0.000 1.000
#&gt; GSM28719     2  0.0000      0.920 0.000 1.000
#&gt; GSM28708     2  0.0000      0.920 0.000 1.000
#&gt; GSM28722     2  0.0000      0.920 0.000 1.000
#&gt; GSM11232     2  0.0000      0.920 0.000 1.000
#&gt; GSM28709     2  0.0000      0.920 0.000 1.000
#&gt; GSM11226     2  0.0000      0.920 0.000 1.000
#&gt; GSM11239     2  0.0000      0.920 0.000 1.000
#&gt; GSM11225     2  0.0000      0.920 0.000 1.000
#&gt; GSM11220     2  0.0000      0.920 0.000 1.000
#&gt; GSM28701     2  0.0000      0.920 0.000 1.000
#&gt; GSM28721     2  0.0000      0.920 0.000 1.000
#&gt; GSM28713     2  0.0000      0.920 0.000 1.000
#&gt; GSM28716     2  0.7674      0.796 0.224 0.776
#&gt; GSM11221     2  0.7056      0.831 0.192 0.808
#&gt; GSM28717     2  0.7139      0.827 0.196 0.804
#&gt; GSM11223     1  0.4690      0.909 0.900 0.100
#&gt; GSM11218     2  0.0000      0.920 0.000 1.000
#&gt; GSM11219     2  0.7056      0.831 0.192 0.808
#&gt; GSM11236     2  0.0000      0.920 0.000 1.000
#&gt; GSM28702     2  0.0000      0.920 0.000 1.000
#&gt; GSM28705     2  0.0000      0.920 0.000 1.000
#&gt; GSM11230     2  0.7056      0.831 0.192 0.808
#&gt; GSM28704     2  0.0000      0.920 0.000 1.000
#&gt; GSM28700     2  0.0672      0.917 0.008 0.992
#&gt; GSM11224     2  0.0000      0.920 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-1-a').click(function(){
  $('#tab-MAD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-2'>
<p><a id='tab-MAD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28710     2  0.0000      0.968 0.000 1.000 0.000
#&gt; GSM28711     2  0.0424      0.968 0.000 0.992 0.008
#&gt; GSM28712     2  0.0000      0.968 0.000 1.000 0.000
#&gt; GSM11222     2  0.6252      0.242 0.000 0.556 0.444
#&gt; GSM28720     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11217     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28723     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11241     1  0.0424      0.992 0.992 0.008 0.000
#&gt; GSM28703     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11227     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28706     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11229     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11235     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28707     1  0.0424      0.992 0.992 0.008 0.000
#&gt; GSM11240     2  0.0000      0.968 0.000 1.000 0.000
#&gt; GSM28714     2  0.0000      0.968 0.000 1.000 0.000
#&gt; GSM11216     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM28715     2  0.0000      0.968 0.000 1.000 0.000
#&gt; GSM11234     2  0.0424      0.968 0.000 0.992 0.008
#&gt; GSM28699     2  0.0000      0.968 0.000 1.000 0.000
#&gt; GSM11233     2  0.0000      0.968 0.000 1.000 0.000
#&gt; GSM28718     2  0.0000      0.968 0.000 1.000 0.000
#&gt; GSM11231     2  0.0000      0.968 0.000 1.000 0.000
#&gt; GSM11237     2  0.0000      0.968 0.000 1.000 0.000
#&gt; GSM11228     2  0.0424      0.968 0.000 0.992 0.008
#&gt; GSM28697     2  0.0424      0.968 0.000 0.992 0.008
#&gt; GSM28698     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11238     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11242     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM28719     2  0.0424      0.968 0.000 0.992 0.008
#&gt; GSM28708     2  0.1753      0.933 0.000 0.952 0.048
#&gt; GSM28722     2  0.0424      0.968 0.000 0.992 0.008
#&gt; GSM11232     2  0.0424      0.968 0.000 0.992 0.008
#&gt; GSM28709     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11226     2  0.0424      0.968 0.000 0.992 0.008
#&gt; GSM11239     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11225     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11220     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM28701     2  0.0000      0.968 0.000 1.000 0.000
#&gt; GSM28721     2  0.0424      0.968 0.000 0.992 0.008
#&gt; GSM28713     2  0.0424      0.968 0.000 0.992 0.008
#&gt; GSM28716     2  0.0000      0.968 0.000 1.000 0.000
#&gt; GSM11221     2  0.0000      0.968 0.000 1.000 0.000
#&gt; GSM28717     2  0.0000      0.968 0.000 1.000 0.000
#&gt; GSM11223     1  0.0424      0.992 0.992 0.008 0.000
#&gt; GSM11218     2  0.0424      0.968 0.000 0.992 0.008
#&gt; GSM11219     2  0.0000      0.968 0.000 1.000 0.000
#&gt; GSM11236     2  0.0424      0.968 0.000 0.992 0.008
#&gt; GSM28702     2  0.6252      0.242 0.000 0.556 0.444
#&gt; GSM28705     2  0.0424      0.968 0.000 0.992 0.008
#&gt; GSM11230     2  0.0000      0.968 0.000 1.000 0.000
#&gt; GSM28704     2  0.0424      0.968 0.000 0.992 0.008
#&gt; GSM28700     2  0.0424      0.968 0.000 0.992 0.008
#&gt; GSM11224     2  0.0424      0.968 0.000 0.992 0.008
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-2-a').click(function(){
  $('#tab-MAD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-3'>
<p><a id='tab-MAD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28710     2  0.4933     0.5089 0.000 0.568 0.000 0.432
#&gt; GSM28711     2  0.4994     0.3944 0.000 0.520 0.000 0.480
#&gt; GSM28712     2  0.1118     0.7368 0.000 0.964 0.000 0.036
#&gt; GSM11222     4  0.4624     0.5294 0.000 0.000 0.340 0.660
#&gt; GSM28720     1  0.0000     0.9988 1.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000     0.9988 1.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0188     0.9979 0.996 0.000 0.000 0.004
#&gt; GSM11241     1  0.0188     0.9979 0.996 0.000 0.000 0.004
#&gt; GSM28703     1  0.0000     0.9988 1.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000     0.9988 1.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000     0.9988 1.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000     0.9988 1.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000     0.9988 1.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0188     0.9979 0.996 0.000 0.000 0.004
#&gt; GSM11240     2  0.0000     0.7217 0.000 1.000 0.000 0.000
#&gt; GSM28714     2  0.0000     0.7217 0.000 1.000 0.000 0.000
#&gt; GSM11216     3  0.0000     0.9994 0.000 0.000 1.000 0.000
#&gt; GSM28715     2  0.1716     0.7396 0.000 0.936 0.000 0.064
#&gt; GSM11234     2  0.4933     0.5089 0.000 0.568 0.000 0.432
#&gt; GSM28699     2  0.1474     0.7394 0.000 0.948 0.000 0.052
#&gt; GSM11233     2  0.1118     0.7368 0.000 0.964 0.000 0.036
#&gt; GSM28718     2  0.0000     0.7217 0.000 1.000 0.000 0.000
#&gt; GSM11231     2  0.1716     0.7396 0.000 0.936 0.000 0.064
#&gt; GSM11237     2  0.0000     0.7217 0.000 1.000 0.000 0.000
#&gt; GSM11228     4  0.0336     0.8558 0.000 0.008 0.000 0.992
#&gt; GSM28697     4  0.0469     0.8537 0.000 0.012 0.000 0.988
#&gt; GSM28698     3  0.0000     0.9994 0.000 0.000 1.000 0.000
#&gt; GSM11238     3  0.0000     0.9994 0.000 0.000 1.000 0.000
#&gt; GSM11242     3  0.0000     0.9994 0.000 0.000 1.000 0.000
#&gt; GSM28719     4  0.0336     0.8558 0.000 0.008 0.000 0.992
#&gt; GSM28708     4  0.0376     0.8536 0.000 0.004 0.004 0.992
#&gt; GSM28722     4  0.4746     0.0874 0.000 0.368 0.000 0.632
#&gt; GSM11232     4  0.0336     0.8558 0.000 0.008 0.000 0.992
#&gt; GSM28709     3  0.0000     0.9994 0.000 0.000 1.000 0.000
#&gt; GSM11226     4  0.0188     0.8542 0.000 0.004 0.000 0.996
#&gt; GSM11239     3  0.0000     0.9994 0.000 0.000 1.000 0.000
#&gt; GSM11225     3  0.0000     0.9994 0.000 0.000 1.000 0.000
#&gt; GSM11220     3  0.0188     0.9959 0.000 0.000 0.996 0.004
#&gt; GSM28701     4  0.4624     0.2244 0.000 0.340 0.000 0.660
#&gt; GSM28721     4  0.0336     0.8558 0.000 0.008 0.000 0.992
#&gt; GSM28713     2  0.4933     0.5089 0.000 0.568 0.000 0.432
#&gt; GSM28716     2  0.4933     0.5089 0.000 0.568 0.000 0.432
#&gt; GSM11221     2  0.4933     0.5089 0.000 0.568 0.000 0.432
#&gt; GSM28717     2  0.1474     0.7394 0.000 0.948 0.000 0.052
#&gt; GSM11223     1  0.0188     0.9979 0.996 0.000 0.000 0.004
#&gt; GSM11218     4  0.0188     0.8542 0.000 0.004 0.000 0.996
#&gt; GSM11219     2  0.0921     0.7320 0.000 0.972 0.000 0.028
#&gt; GSM11236     4  0.1302     0.8260 0.000 0.044 0.000 0.956
#&gt; GSM28702     4  0.4624     0.5294 0.000 0.000 0.340 0.660
#&gt; GSM28705     4  0.0336     0.8558 0.000 0.008 0.000 0.992
#&gt; GSM11230     2  0.1716     0.7396 0.000 0.936 0.000 0.064
#&gt; GSM28704     2  0.4941     0.5009 0.000 0.564 0.000 0.436
#&gt; GSM28700     2  0.4933     0.5089 0.000 0.568 0.000 0.432
#&gt; GSM11224     2  0.4933     0.5089 0.000 0.568 0.000 0.432
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-3-a').click(function(){
  $('#tab-MAD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-4'>
<p><a id='tab-MAD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28710     2  0.2077     0.5790 0.000 0.908 0.000 0.084 0.008
#&gt; GSM28711     2  0.4088     0.2348 0.000 0.632 0.000 0.368 0.000
#&gt; GSM28712     2  0.3796     0.1107 0.000 0.700 0.000 0.000 0.300
#&gt; GSM11222     3  0.6080     0.6138 0.000 0.000 0.528 0.140 0.332
#&gt; GSM28720     1  0.0000     0.9675 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000     0.9675 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0794     0.9605 0.972 0.000 0.000 0.028 0.000
#&gt; GSM11241     1  0.2989     0.9195 0.868 0.000 0.000 0.060 0.072
#&gt; GSM28703     1  0.0000     0.9675 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000     0.9675 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0162     0.9669 0.996 0.000 0.000 0.004 0.000
#&gt; GSM11229     1  0.0000     0.9675 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000     0.9675 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.2989     0.9195 0.868 0.000 0.000 0.060 0.072
#&gt; GSM11240     5  0.4219     0.9935 0.000 0.416 0.000 0.000 0.584
#&gt; GSM28714     5  0.4210     0.9978 0.000 0.412 0.000 0.000 0.588
#&gt; GSM11216     3  0.0000     0.9146 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28715     2  0.3336     0.3122 0.000 0.772 0.000 0.000 0.228
#&gt; GSM11234     2  0.3196     0.5352 0.000 0.804 0.000 0.192 0.004
#&gt; GSM28699     2  0.3774     0.1481 0.000 0.704 0.000 0.000 0.296
#&gt; GSM11233     2  0.3857     0.0829 0.000 0.688 0.000 0.000 0.312
#&gt; GSM28718     5  0.4210     0.9978 0.000 0.412 0.000 0.000 0.588
#&gt; GSM11231     2  0.1732     0.5290 0.000 0.920 0.000 0.000 0.080
#&gt; GSM11237     5  0.4210     0.9978 0.000 0.412 0.000 0.000 0.588
#&gt; GSM11228     4  0.1410     0.9033 0.000 0.060 0.000 0.940 0.000
#&gt; GSM28697     4  0.3074     0.7945 0.000 0.196 0.000 0.804 0.000
#&gt; GSM28698     3  0.0000     0.9146 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11238     3  0.0000     0.9146 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11242     3  0.0000     0.9146 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28719     4  0.1410     0.9033 0.000 0.060 0.000 0.940 0.000
#&gt; GSM28708     4  0.1571     0.9013 0.000 0.060 0.004 0.936 0.000
#&gt; GSM28722     2  0.4118     0.2995 0.000 0.660 0.000 0.336 0.004
#&gt; GSM11232     4  0.2424     0.8596 0.000 0.132 0.000 0.868 0.000
#&gt; GSM28709     3  0.0510     0.9103 0.000 0.000 0.984 0.000 0.016
#&gt; GSM11226     4  0.1410     0.9033 0.000 0.060 0.000 0.940 0.000
#&gt; GSM11239     3  0.0000     0.9146 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11225     3  0.0000     0.9146 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11220     3  0.0798     0.9073 0.000 0.000 0.976 0.008 0.016
#&gt; GSM28701     4  0.4367     0.4419 0.000 0.372 0.000 0.620 0.008
#&gt; GSM28721     4  0.1410     0.9033 0.000 0.060 0.000 0.940 0.000
#&gt; GSM28713     2  0.0324     0.5784 0.000 0.992 0.000 0.004 0.004
#&gt; GSM28716     2  0.0451     0.5787 0.000 0.988 0.000 0.004 0.008
#&gt; GSM11221     2  0.2806     0.5559 0.000 0.844 0.000 0.152 0.004
#&gt; GSM28717     2  0.3774     0.1481 0.000 0.704 0.000 0.000 0.296
#&gt; GSM11223     1  0.2989     0.9195 0.868 0.000 0.000 0.060 0.072
#&gt; GSM11218     4  0.1410     0.9033 0.000 0.060 0.000 0.940 0.000
#&gt; GSM11219     2  0.4074    -0.2714 0.000 0.636 0.000 0.000 0.364
#&gt; GSM11236     4  0.3452     0.7200 0.000 0.244 0.000 0.756 0.000
#&gt; GSM28702     3  0.6080     0.6138 0.000 0.000 0.528 0.140 0.332
#&gt; GSM28705     4  0.1410     0.9033 0.000 0.060 0.000 0.940 0.000
#&gt; GSM11230     2  0.3370     0.4610 0.000 0.824 0.000 0.028 0.148
#&gt; GSM28704     2  0.3969     0.3736 0.000 0.692 0.000 0.304 0.004
#&gt; GSM28700     2  0.0324     0.5791 0.000 0.992 0.000 0.004 0.004
#&gt; GSM11224     2  0.0324     0.5784 0.000 0.992 0.000 0.004 0.004
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-4-a').click(function(){
  $('#tab-MAD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-5'>
<p><a id='tab-MAD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28710     2  0.0291     0.7044 0.000 0.992 0.000 0.004 0.000 0.004
#&gt; GSM28711     2  0.1983     0.6716 0.000 0.908 0.000 0.020 0.000 0.072
#&gt; GSM28712     5  0.5481     0.9265 0.000 0.436 0.000 0.000 0.440 0.124
#&gt; GSM11222     3  0.6583     0.5401 0.000 0.000 0.484 0.052 0.204 0.260
#&gt; GSM28720     1  0.0000     0.9495 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000     0.9495 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000     0.9495 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.2793     0.8567 0.800 0.000 0.000 0.000 0.200 0.000
#&gt; GSM28703     1  0.0000     0.9495 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000     0.9495 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000     0.9495 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000     0.9495 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000     0.9495 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.2793     0.8567 0.800 0.000 0.000 0.000 0.200 0.000
#&gt; GSM11240     6  0.4513     1.0000 0.000 0.032 0.000 0.000 0.440 0.528
#&gt; GSM28714     6  0.4513     1.0000 0.000 0.032 0.000 0.000 0.440 0.528
#&gt; GSM11216     3  0.0000     0.8983 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28715     2  0.3707     0.3489 0.000 0.680 0.000 0.008 0.312 0.000
#&gt; GSM11234     2  0.0865     0.7039 0.000 0.964 0.000 0.036 0.000 0.000
#&gt; GSM28699     5  0.5436     0.9753 0.000 0.404 0.000 0.000 0.476 0.120
#&gt; GSM11233     5  0.5436     0.9753 0.000 0.404 0.000 0.000 0.476 0.120
#&gt; GSM28718     6  0.4513     1.0000 0.000 0.032 0.000 0.000 0.440 0.528
#&gt; GSM11231     2  0.3390     0.3864 0.000 0.704 0.000 0.000 0.296 0.000
#&gt; GSM11237     6  0.4513     1.0000 0.000 0.032 0.000 0.000 0.440 0.528
#&gt; GSM11228     4  0.1285     0.8253 0.000 0.004 0.000 0.944 0.000 0.052
#&gt; GSM28697     4  0.2631     0.7547 0.000 0.152 0.000 0.840 0.000 0.008
#&gt; GSM28698     3  0.0000     0.8983 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11238     3  0.0000     0.8983 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11242     3  0.0146     0.8986 0.000 0.000 0.996 0.004 0.000 0.000
#&gt; GSM28719     4  0.3939     0.8557 0.000 0.068 0.000 0.752 0.000 0.180
#&gt; GSM28708     4  0.3221     0.8628 0.000 0.020 0.000 0.792 0.000 0.188
#&gt; GSM28722     2  0.0937     0.7036 0.000 0.960 0.000 0.040 0.000 0.000
#&gt; GSM11232     4  0.2454     0.7758 0.000 0.160 0.000 0.840 0.000 0.000
#&gt; GSM28709     3  0.0458     0.8946 0.000 0.000 0.984 0.000 0.000 0.016
#&gt; GSM11226     4  0.3221     0.8628 0.000 0.020 0.000 0.792 0.000 0.188
#&gt; GSM11239     3  0.0146     0.8986 0.000 0.000 0.996 0.004 0.000 0.000
#&gt; GSM11225     3  0.0146     0.8986 0.000 0.000 0.996 0.004 0.000 0.000
#&gt; GSM11220     3  0.0777     0.8899 0.000 0.000 0.972 0.004 0.000 0.024
#&gt; GSM28701     2  0.5265     0.3510 0.000 0.604 0.000 0.220 0.000 0.176
#&gt; GSM28721     4  0.2491     0.8655 0.000 0.020 0.000 0.868 0.000 0.112
#&gt; GSM28713     2  0.0458     0.7024 0.000 0.984 0.000 0.016 0.000 0.000
#&gt; GSM28716     2  0.1490     0.6932 0.008 0.948 0.000 0.024 0.016 0.004
#&gt; GSM11221     2  0.0291     0.7044 0.000 0.992 0.000 0.004 0.000 0.004
#&gt; GSM28717     5  0.5436     0.9753 0.000 0.404 0.000 0.000 0.476 0.120
#&gt; GSM11223     1  0.2793     0.8567 0.800 0.000 0.000 0.000 0.200 0.000
#&gt; GSM11218     4  0.3221     0.8628 0.000 0.020 0.000 0.792 0.000 0.188
#&gt; GSM11219     2  0.5569    -0.0554 0.000 0.520 0.000 0.000 0.320 0.160
#&gt; GSM11236     2  0.5843     0.2569 0.000 0.512 0.008 0.308 0.000 0.172
#&gt; GSM28702     3  0.6622     0.5285 0.000 0.000 0.472 0.052 0.204 0.272
#&gt; GSM28705     4  0.1285     0.8253 0.000 0.004 0.000 0.944 0.000 0.052
#&gt; GSM11230     2  0.3888     0.3541 0.000 0.672 0.000 0.016 0.312 0.000
#&gt; GSM28704     2  0.0865     0.7037 0.000 0.964 0.000 0.036 0.000 0.000
#&gt; GSM28700     2  0.2446     0.6652 0.000 0.864 0.000 0.012 0.124 0.000
#&gt; GSM11224     2  0.2494     0.6671 0.000 0.864 0.000 0.016 0.120 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-5-a').click(function(){
  $('#tab-MAD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-mclust-signature_compare](figure_cola/MAD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-mclust-collect-classes](figure_cola/MAD-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> MAD:mclust 54     0.398 2
#> MAD:mclust 52     0.372 3
#> MAD:mclust 51     0.350 4
#> MAD:mclust 43     0.487 5
#> MAD:mclust 48     0.398 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:NMF*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "NMF"]
# you can also extract it by
# res = res_list["MAD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21452 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-NMF-collect-plots](figure_cola/MAD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-NMF-select-partition-number](figure_cola/MAD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.634           0.766       0.907         0.4307 0.575   0.575
#> 3 3 0.970           0.957       0.981         0.4444 0.743   0.574
#> 4 4 0.931           0.898       0.947         0.2010 0.830   0.570
#> 5 5 0.842           0.752       0.885         0.0566 0.941   0.769
#> 6 6 0.805           0.792       0.864         0.0341 0.964   0.829
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
#> attr(,"optional")
#> [1] 3
```

There is also optional best $k$ = 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-classes'>
<ul>
<li><a href='#tab-MAD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-NMF-get-classes-1'>
<p><a id='tab-MAD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28710     2  0.0376     0.8958 0.004 0.996
#&gt; GSM28711     2  0.0000     0.8984 0.000 1.000
#&gt; GSM28712     2  0.0000     0.8984 0.000 1.000
#&gt; GSM11222     2  0.9170     0.5005 0.332 0.668
#&gt; GSM28720     1  0.1184     0.8562 0.984 0.016
#&gt; GSM11217     1  0.1184     0.8562 0.984 0.016
#&gt; GSM28723     1  0.1184     0.8562 0.984 0.016
#&gt; GSM11241     1  0.1184     0.8562 0.984 0.016
#&gt; GSM28703     1  0.1184     0.8562 0.984 0.016
#&gt; GSM11227     1  0.1184     0.8562 0.984 0.016
#&gt; GSM28706     1  0.1184     0.8562 0.984 0.016
#&gt; GSM11229     1  0.1184     0.8562 0.984 0.016
#&gt; GSM11235     1  0.1184     0.8562 0.984 0.016
#&gt; GSM28707     1  0.1184     0.8562 0.984 0.016
#&gt; GSM11240     2  0.0000     0.8984 0.000 1.000
#&gt; GSM28714     2  0.0000     0.8984 0.000 1.000
#&gt; GSM11216     1  0.9087     0.4899 0.676 0.324
#&gt; GSM28715     2  0.0000     0.8984 0.000 1.000
#&gt; GSM11234     2  0.0000     0.8984 0.000 1.000
#&gt; GSM28699     2  0.9996    -0.1183 0.488 0.512
#&gt; GSM11233     2  0.0000     0.8984 0.000 1.000
#&gt; GSM28718     2  0.0000     0.8984 0.000 1.000
#&gt; GSM11231     2  0.0000     0.8984 0.000 1.000
#&gt; GSM11237     2  0.0000     0.8984 0.000 1.000
#&gt; GSM11228     2  0.0000     0.8984 0.000 1.000
#&gt; GSM28697     2  0.0000     0.8984 0.000 1.000
#&gt; GSM28698     2  0.9552     0.3979 0.376 0.624
#&gt; GSM11238     1  0.9983     0.0525 0.524 0.476
#&gt; GSM11242     2  0.9170     0.5005 0.332 0.668
#&gt; GSM28719     2  0.0672     0.8942 0.008 0.992
#&gt; GSM28708     2  0.1633     0.8857 0.024 0.976
#&gt; GSM28722     2  0.0000     0.8984 0.000 1.000
#&gt; GSM11232     2  0.0000     0.8984 0.000 1.000
#&gt; GSM28709     2  0.9170     0.5005 0.332 0.668
#&gt; GSM11226     2  0.1184     0.8892 0.016 0.984
#&gt; GSM11239     2  0.9170     0.5005 0.332 0.668
#&gt; GSM11225     1  0.9909     0.1710 0.556 0.444
#&gt; GSM11220     1  0.8661     0.5545 0.712 0.288
#&gt; GSM28701     2  0.0000     0.8984 0.000 1.000
#&gt; GSM28721     2  0.1184     0.8892 0.016 0.984
#&gt; GSM28713     2  0.0000     0.8984 0.000 1.000
#&gt; GSM28716     1  0.8386     0.5741 0.732 0.268
#&gt; GSM11221     2  0.0000     0.8984 0.000 1.000
#&gt; GSM28717     2  0.5629     0.7602 0.132 0.868
#&gt; GSM11223     1  0.1184     0.8562 0.984 0.016
#&gt; GSM11218     2  0.1184     0.8892 0.016 0.984
#&gt; GSM11219     2  0.0000     0.8984 0.000 1.000
#&gt; GSM11236     2  0.8713     0.5611 0.292 0.708
#&gt; GSM28702     2  0.9170     0.5005 0.332 0.668
#&gt; GSM28705     2  0.0000     0.8984 0.000 1.000
#&gt; GSM11230     2  0.0672     0.8942 0.008 0.992
#&gt; GSM28704     2  0.0000     0.8984 0.000 1.000
#&gt; GSM28700     2  0.0000     0.8984 0.000 1.000
#&gt; GSM11224     2  0.0000     0.8984 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-1-a').click(function(){
  $('#tab-MAD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-2'>
<p><a id='tab-MAD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28710     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28711     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28712     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM11222     3  0.0000      0.937 0.000 0.000 1.000
#&gt; GSM28720     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11217     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28723     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11241     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28703     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11227     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28706     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11229     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11235     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28707     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11240     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28714     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM11216     3  0.0000      0.937 0.000 0.000 1.000
#&gt; GSM28715     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM11234     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28699     2  0.2165      0.923 0.064 0.936 0.000
#&gt; GSM11233     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28718     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM11231     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM11237     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM11228     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28697     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28698     3  0.0000      0.937 0.000 0.000 1.000
#&gt; GSM11238     3  0.0000      0.937 0.000 0.000 1.000
#&gt; GSM11242     3  0.0000      0.937 0.000 0.000 1.000
#&gt; GSM28719     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28708     3  0.3686      0.827 0.000 0.140 0.860
#&gt; GSM28722     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM11232     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28709     3  0.0000      0.937 0.000 0.000 1.000
#&gt; GSM11226     2  0.5138      0.631 0.000 0.748 0.252
#&gt; GSM11239     3  0.0000      0.937 0.000 0.000 1.000
#&gt; GSM11225     3  0.0000      0.937 0.000 0.000 1.000
#&gt; GSM11220     3  0.0000      0.937 0.000 0.000 1.000
#&gt; GSM28701     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28721     3  0.5216      0.684 0.000 0.260 0.740
#&gt; GSM28713     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28716     1  0.1031      0.969 0.976 0.024 0.000
#&gt; GSM11221     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28717     2  0.0592      0.976 0.012 0.988 0.000
#&gt; GSM11223     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11218     3  0.1860      0.903 0.000 0.052 0.948
#&gt; GSM11219     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM11236     3  0.4504      0.767 0.000 0.196 0.804
#&gt; GSM28702     3  0.0000      0.937 0.000 0.000 1.000
#&gt; GSM28705     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM11230     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28704     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM28700     2  0.0000      0.987 0.000 1.000 0.000
#&gt; GSM11224     2  0.0000      0.987 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-2-a').click(function(){
  $('#tab-MAD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-3'>
<p><a id='tab-MAD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28710     2  0.0817     0.9353 0.000 0.976 0.000 0.024
#&gt; GSM28711     2  0.2589     0.8753 0.000 0.884 0.000 0.116
#&gt; GSM28712     2  0.0817     0.9351 0.000 0.976 0.000 0.024
#&gt; GSM11222     3  0.0188     0.9895 0.000 0.000 0.996 0.004
#&gt; GSM28720     1  0.0000     0.9994 1.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000     0.9994 1.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000     0.9994 1.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000     0.9994 1.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000     0.9994 1.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000     0.9994 1.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000     0.9994 1.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000     0.9994 1.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000     0.9994 1.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000     0.9994 1.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.0817     0.9351 0.000 0.976 0.000 0.024
#&gt; GSM28714     2  0.0707     0.9320 0.000 0.980 0.000 0.020
#&gt; GSM11216     3  0.0469     0.9861 0.000 0.000 0.988 0.012
#&gt; GSM28715     2  0.1389     0.9314 0.000 0.952 0.000 0.048
#&gt; GSM11234     4  0.1022     0.8846 0.000 0.032 0.000 0.968
#&gt; GSM28699     2  0.0469     0.9171 0.000 0.988 0.000 0.012
#&gt; GSM11233     2  0.0592     0.9145 0.000 0.984 0.000 0.016
#&gt; GSM28718     2  0.0707     0.9345 0.000 0.980 0.000 0.020
#&gt; GSM11231     2  0.1940     0.9136 0.000 0.924 0.000 0.076
#&gt; GSM11237     2  0.0592     0.9334 0.000 0.984 0.000 0.016
#&gt; GSM11228     4  0.0817     0.8834 0.000 0.024 0.000 0.976
#&gt; GSM28697     4  0.1022     0.8846 0.000 0.032 0.000 0.968
#&gt; GSM28698     3  0.0000     0.9890 0.000 0.000 1.000 0.000
#&gt; GSM11238     3  0.0188     0.9895 0.000 0.000 0.996 0.004
#&gt; GSM11242     3  0.0188     0.9895 0.000 0.000 0.996 0.004
#&gt; GSM28719     4  0.4331     0.6006 0.000 0.288 0.000 0.712
#&gt; GSM28708     4  0.4193     0.6149 0.000 0.000 0.268 0.732
#&gt; GSM28722     4  0.1474     0.8778 0.000 0.052 0.000 0.948
#&gt; GSM11232     4  0.1118     0.8840 0.000 0.036 0.000 0.964
#&gt; GSM28709     3  0.0469     0.9861 0.000 0.000 0.988 0.012
#&gt; GSM11226     4  0.1388     0.8745 0.000 0.012 0.028 0.960
#&gt; GSM11239     3  0.0188     0.9895 0.000 0.000 0.996 0.004
#&gt; GSM11225     3  0.0188     0.9895 0.000 0.000 0.996 0.004
#&gt; GSM11220     3  0.0804     0.9826 0.008 0.000 0.980 0.012
#&gt; GSM28701     2  0.4992     0.0111 0.000 0.524 0.000 0.476
#&gt; GSM28721     4  0.1284     0.8718 0.000 0.012 0.024 0.964
#&gt; GSM28713     4  0.4985     0.0930 0.000 0.468 0.000 0.532
#&gt; GSM28716     1  0.0336     0.9931 0.992 0.008 0.000 0.000
#&gt; GSM11221     2  0.1557     0.9276 0.000 0.944 0.000 0.056
#&gt; GSM28717     2  0.0592     0.9145 0.000 0.984 0.000 0.016
#&gt; GSM11223     1  0.0000     0.9994 1.000 0.000 0.000 0.000
#&gt; GSM11218     4  0.1557     0.8533 0.000 0.000 0.056 0.944
#&gt; GSM11219     2  0.1211     0.9341 0.000 0.960 0.000 0.040
#&gt; GSM11236     3  0.1059     0.9744 0.000 0.016 0.972 0.012
#&gt; GSM28702     3  0.1022     0.9673 0.000 0.000 0.968 0.032
#&gt; GSM28705     4  0.0921     0.8844 0.000 0.028 0.000 0.972
#&gt; GSM11230     2  0.1302     0.9329 0.000 0.956 0.000 0.044
#&gt; GSM28704     4  0.2973     0.8068 0.000 0.144 0.000 0.856
#&gt; GSM28700     2  0.1211     0.9343 0.000 0.960 0.000 0.040
#&gt; GSM11224     2  0.2868     0.8521 0.000 0.864 0.000 0.136
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-3-a').click(function(){
  $('#tab-MAD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-4'>
<p><a id='tab-MAD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28710     2  0.1043     0.9297 0.000 0.960 0.000 0.000 0.040
#&gt; GSM28711     2  0.3452     0.8050 0.000 0.820 0.000 0.148 0.032
#&gt; GSM28712     2  0.0290     0.9402 0.000 0.992 0.000 0.008 0.000
#&gt; GSM11222     3  0.0510     0.8387 0.000 0.000 0.984 0.000 0.016
#&gt; GSM28720     1  0.0000     0.9971 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000     0.9971 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000     0.9971 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000     0.9971 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000     0.9971 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000     0.9971 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000     0.9971 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000     0.9971 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000     0.9971 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000     0.9971 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.0290     0.9402 0.000 0.992 0.000 0.008 0.000
#&gt; GSM28714     2  0.0162     0.9397 0.000 0.996 0.000 0.004 0.000
#&gt; GSM11216     3  0.4278     0.3298 0.000 0.000 0.548 0.000 0.452
#&gt; GSM28715     2  0.1571     0.9187 0.000 0.936 0.000 0.060 0.004
#&gt; GSM11234     4  0.3724     0.6319 0.000 0.028 0.000 0.788 0.184
#&gt; GSM28699     2  0.1197     0.9250 0.000 0.952 0.000 0.000 0.048
#&gt; GSM11233     2  0.0703     0.9337 0.000 0.976 0.000 0.000 0.024
#&gt; GSM28718     2  0.0290     0.9402 0.000 0.992 0.000 0.008 0.000
#&gt; GSM11231     5  0.5858     0.0176 0.000 0.452 0.000 0.096 0.452
#&gt; GSM11237     2  0.0404     0.9377 0.000 0.988 0.000 0.000 0.012
#&gt; GSM11228     4  0.2516     0.6868 0.000 0.000 0.000 0.860 0.140
#&gt; GSM28697     4  0.4251     0.3259 0.000 0.004 0.000 0.624 0.372
#&gt; GSM28698     3  0.1043     0.8322 0.000 0.000 0.960 0.000 0.040
#&gt; GSM11238     3  0.0609     0.8434 0.000 0.000 0.980 0.000 0.020
#&gt; GSM11242     3  0.0000     0.8469 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28719     5  0.5026     0.3052 0.000 0.028 0.012 0.328 0.632
#&gt; GSM28708     5  0.5592     0.3253 0.000 0.004 0.084 0.312 0.600
#&gt; GSM28722     4  0.0798     0.7601 0.000 0.016 0.000 0.976 0.008
#&gt; GSM11232     4  0.1124     0.7556 0.000 0.004 0.000 0.960 0.036
#&gt; GSM28709     5  0.4227    -0.1555 0.000 0.000 0.420 0.000 0.580
#&gt; GSM11226     4  0.2917     0.7294 0.000 0.028 0.052 0.888 0.032
#&gt; GSM11239     3  0.0162     0.8468 0.000 0.000 0.996 0.000 0.004
#&gt; GSM11225     3  0.0162     0.8468 0.000 0.000 0.996 0.000 0.004
#&gt; GSM11220     3  0.4452     0.2275 0.000 0.000 0.500 0.004 0.496
#&gt; GSM28701     5  0.3719     0.4441 0.000 0.068 0.000 0.116 0.816
#&gt; GSM28721     4  0.0000     0.7594 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28713     4  0.5459     0.0109 0.000 0.468 0.000 0.472 0.060
#&gt; GSM28716     1  0.0932     0.9680 0.972 0.020 0.000 0.004 0.004
#&gt; GSM11221     2  0.1981     0.9259 0.000 0.924 0.000 0.048 0.028
#&gt; GSM28717     2  0.1544     0.9122 0.000 0.932 0.000 0.000 0.068
#&gt; GSM11223     1  0.0000     0.9971 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     4  0.4114     0.6076 0.000 0.004 0.176 0.776 0.044
#&gt; GSM11219     2  0.0963     0.9339 0.000 0.964 0.000 0.036 0.000
#&gt; GSM11236     5  0.4934    -0.0305 0.000 0.020 0.432 0.004 0.544
#&gt; GSM28702     3  0.0798     0.8401 0.000 0.000 0.976 0.016 0.008
#&gt; GSM28705     4  0.0162     0.7602 0.000 0.004 0.000 0.996 0.000
#&gt; GSM11230     2  0.2206     0.9047 0.000 0.912 0.004 0.068 0.016
#&gt; GSM28704     4  0.2953     0.6652 0.000 0.144 0.000 0.844 0.012
#&gt; GSM28700     2  0.1168     0.9371 0.000 0.960 0.000 0.032 0.008
#&gt; GSM11224     2  0.2891     0.8030 0.000 0.824 0.000 0.176 0.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-4-a').click(function(){
  $('#tab-MAD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-5'>
<p><a id='tab-MAD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28710     2  0.4314    0.76370 0.000 0.736 0.000 0.068 0.184 0.012
#&gt; GSM28711     2  0.3611    0.79677 0.000 0.812 0.000 0.104 0.012 0.072
#&gt; GSM28712     2  0.1180    0.86845 0.000 0.960 0.000 0.012 0.016 0.012
#&gt; GSM11222     3  0.1367    0.90359 0.000 0.000 0.944 0.012 0.044 0.000
#&gt; GSM28720     1  0.0000    0.99468 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000    0.99468 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000    0.99468 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000    0.99468 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000    0.99468 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000    0.99468 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000    0.99468 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000    0.99468 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000    0.99468 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000    0.99468 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.0951    0.86934 0.000 0.968 0.000 0.008 0.004 0.020
#&gt; GSM28714     2  0.1275    0.86768 0.000 0.956 0.000 0.016 0.012 0.016
#&gt; GSM11216     5  0.4352    0.75986 0.000 0.000 0.280 0.052 0.668 0.000
#&gt; GSM28715     2  0.1857    0.86235 0.000 0.928 0.000 0.032 0.012 0.028
#&gt; GSM11234     6  0.3930    0.62361 0.000 0.028 0.000 0.012 0.216 0.744
#&gt; GSM28699     2  0.3765    0.79013 0.000 0.780 0.000 0.060 0.156 0.004
#&gt; GSM11233     2  0.2937    0.82782 0.000 0.852 0.000 0.044 0.100 0.004
#&gt; GSM28718     2  0.1275    0.86768 0.000 0.956 0.000 0.016 0.012 0.016
#&gt; GSM11231     4  0.3510    0.53886 0.000 0.204 0.000 0.772 0.008 0.016
#&gt; GSM11237     2  0.1471    0.85361 0.000 0.932 0.000 0.064 0.004 0.000
#&gt; GSM11228     4  0.4482    0.45800 0.000 0.000 0.000 0.580 0.036 0.384
#&gt; GSM28697     4  0.4616    0.61402 0.000 0.000 0.004 0.684 0.084 0.228
#&gt; GSM28698     3  0.2768    0.73051 0.000 0.000 0.832 0.012 0.156 0.000
#&gt; GSM11238     3  0.1500    0.89526 0.000 0.000 0.936 0.012 0.052 0.000
#&gt; GSM11242     3  0.0291    0.91291 0.000 0.000 0.992 0.004 0.004 0.000
#&gt; GSM28719     4  0.2125    0.65092 0.000 0.016 0.004 0.908 0.004 0.068
#&gt; GSM28708     4  0.2369    0.63337 0.000 0.008 0.032 0.904 0.008 0.048
#&gt; GSM28722     6  0.0858    0.77034 0.000 0.028 0.000 0.004 0.000 0.968
#&gt; GSM11232     6  0.1625    0.74465 0.000 0.012 0.000 0.060 0.000 0.928
#&gt; GSM28709     5  0.5153    0.75653 0.000 0.000 0.288 0.120 0.592 0.000
#&gt; GSM11226     6  0.3844    0.68726 0.000 0.024 0.152 0.008 0.024 0.792
#&gt; GSM11239     3  0.0909    0.90625 0.000 0.000 0.968 0.012 0.020 0.000
#&gt; GSM11225     3  0.1320    0.89302 0.000 0.000 0.948 0.016 0.036 0.000
#&gt; GSM11220     5  0.4239    0.72631 0.000 0.000 0.156 0.088 0.748 0.008
#&gt; GSM28701     4  0.5191   -0.00313 0.000 0.028 0.000 0.480 0.456 0.036
#&gt; GSM28721     6  0.0551    0.76583 0.000 0.008 0.000 0.004 0.004 0.984
#&gt; GSM28713     6  0.4554    0.29293 0.000 0.360 0.000 0.004 0.036 0.600
#&gt; GSM28716     1  0.1375    0.94351 0.952 0.028 0.000 0.008 0.004 0.008
#&gt; GSM11221     2  0.4378    0.80918 0.000 0.764 0.000 0.036 0.088 0.112
#&gt; GSM28717     2  0.4230    0.75090 0.000 0.728 0.000 0.068 0.200 0.004
#&gt; GSM11223     1  0.0146    0.99195 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM11218     6  0.4812    0.53094 0.000 0.024 0.300 0.008 0.024 0.644
#&gt; GSM11219     2  0.0972    0.87018 0.000 0.964 0.000 0.008 0.000 0.028
#&gt; GSM11236     5  0.6002    0.54017 0.000 0.008 0.192 0.328 0.472 0.000
#&gt; GSM28702     3  0.1767    0.90051 0.000 0.000 0.932 0.012 0.036 0.020
#&gt; GSM28705     6  0.0603    0.76913 0.000 0.016 0.000 0.000 0.004 0.980
#&gt; GSM11230     2  0.2872    0.83608 0.000 0.868 0.016 0.004 0.024 0.088
#&gt; GSM28704     6  0.2278    0.72097 0.000 0.128 0.000 0.004 0.000 0.868
#&gt; GSM28700     2  0.3316    0.84407 0.000 0.840 0.000 0.020 0.056 0.084
#&gt; GSM11224     2  0.3314    0.67646 0.000 0.740 0.000 0.000 0.004 0.256
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-5-a').click(function(){
  $('#tab-MAD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-NMF-signature_compare](figure_cola/MAD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-NMF-collect-classes](figure_cola/MAD-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n tissue(p) k
#> MAD:NMF 49     0.393 2
#> MAD:NMF 54     0.374 3
#> MAD:NMF 52     0.352 4
#> MAD:NMF 44     0.410 5
#> MAD:NMF 51     0.452 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "hclust"]
# you can also extract it by
# res = res_list["ATC:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21452 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-hclust-collect-plots](figure_cola/ATC-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-hclust-select-partition-number](figure_cola/ATC-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000        0.30802 0.693   0.693
#> 3 3 1.000           0.981       0.993        0.84115 0.746   0.634
#> 4 4 0.803           0.916       0.931        0.26231 0.839   0.634
#> 5 5 0.961           0.947       0.976        0.08340 0.947   0.809
#> 6 6 0.925           0.913       0.963        0.00707 0.994   0.972
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-classes'>
<ul>
<li><a href='#tab-ATC-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-hclust-get-classes-1'>
<p><a id='tab-ATC-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28710     2  0.0000      1.000 0.000 1.000
#&gt; GSM28711     2  0.0000      1.000 0.000 1.000
#&gt; GSM28712     2  0.0000      1.000 0.000 1.000
#&gt; GSM11222     1  0.0376      0.996 0.996 0.004
#&gt; GSM28720     2  0.0000      1.000 0.000 1.000
#&gt; GSM11217     2  0.0000      1.000 0.000 1.000
#&gt; GSM28723     2  0.0000      1.000 0.000 1.000
#&gt; GSM11241     2  0.0000      1.000 0.000 1.000
#&gt; GSM28703     2  0.0000      1.000 0.000 1.000
#&gt; GSM11227     2  0.0000      1.000 0.000 1.000
#&gt; GSM28706     2  0.0000      1.000 0.000 1.000
#&gt; GSM11229     2  0.0000      1.000 0.000 1.000
#&gt; GSM11235     2  0.0000      1.000 0.000 1.000
#&gt; GSM28707     2  0.0000      1.000 0.000 1.000
#&gt; GSM11240     2  0.0000      1.000 0.000 1.000
#&gt; GSM28714     2  0.0000      1.000 0.000 1.000
#&gt; GSM11216     1  0.0000      0.999 1.000 0.000
#&gt; GSM28715     2  0.0000      1.000 0.000 1.000
#&gt; GSM11234     2  0.0000      1.000 0.000 1.000
#&gt; GSM28699     2  0.0000      1.000 0.000 1.000
#&gt; GSM11233     2  0.0000      1.000 0.000 1.000
#&gt; GSM28718     2  0.0000      1.000 0.000 1.000
#&gt; GSM11231     2  0.0000      1.000 0.000 1.000
#&gt; GSM11237     2  0.0000      1.000 0.000 1.000
#&gt; GSM11228     2  0.0000      1.000 0.000 1.000
#&gt; GSM28697     2  0.0000      1.000 0.000 1.000
#&gt; GSM28698     1  0.0000      0.999 1.000 0.000
#&gt; GSM11238     1  0.0000      0.999 1.000 0.000
#&gt; GSM11242     1  0.0000      0.999 1.000 0.000
#&gt; GSM28719     2  0.0000      1.000 0.000 1.000
#&gt; GSM28708     2  0.0000      1.000 0.000 1.000
#&gt; GSM28722     2  0.0000      1.000 0.000 1.000
#&gt; GSM11232     2  0.0000      1.000 0.000 1.000
#&gt; GSM28709     1  0.0000      0.999 1.000 0.000
#&gt; GSM11226     2  0.0000      1.000 0.000 1.000
#&gt; GSM11239     1  0.0000      0.999 1.000 0.000
#&gt; GSM11225     1  0.0000      0.999 1.000 0.000
#&gt; GSM11220     1  0.0000      0.999 1.000 0.000
#&gt; GSM28701     2  0.0000      1.000 0.000 1.000
#&gt; GSM28721     2  0.0000      1.000 0.000 1.000
#&gt; GSM28713     2  0.0000      1.000 0.000 1.000
#&gt; GSM28716     2  0.0000      1.000 0.000 1.000
#&gt; GSM11221     2  0.0000      1.000 0.000 1.000
#&gt; GSM28717     2  0.0000      1.000 0.000 1.000
#&gt; GSM11223     2  0.0000      1.000 0.000 1.000
#&gt; GSM11218     2  0.0000      1.000 0.000 1.000
#&gt; GSM11219     2  0.0000      1.000 0.000 1.000
#&gt; GSM11236     2  0.0000      1.000 0.000 1.000
#&gt; GSM28702     1  0.0376      0.996 0.996 0.004
#&gt; GSM28705     2  0.0000      1.000 0.000 1.000
#&gt; GSM11230     2  0.0000      1.000 0.000 1.000
#&gt; GSM28704     2  0.0000      1.000 0.000 1.000
#&gt; GSM28700     2  0.0000      1.000 0.000 1.000
#&gt; GSM11224     2  0.0000      1.000 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-1-a').click(function(){
  $('#tab-ATC-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-2'>
<p><a id='tab-ATC-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28710     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM28711     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM28712     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM11222     3  0.0237      0.995 0.000 0.004 0.996
#&gt; GSM28720     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11217     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28723     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11241     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28703     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11227     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28706     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11229     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11235     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28707     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11240     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM28714     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM11216     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM28715     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM11234     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM28699     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM11233     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM28718     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM11231     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM11237     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM11228     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM28697     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM28698     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM11238     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM11242     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM28719     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM28708     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM28722     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM11232     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM28709     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM11226     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM11239     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM11225     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM11220     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM28701     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM28721     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM28713     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM28716     2  0.6062      0.377 0.384 0.616 0.000
#&gt; GSM11221     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM28717     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM11223     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11218     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM11219     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM11236     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM28702     3  0.0237      0.995 0.000 0.004 0.996
#&gt; GSM28705     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM11230     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM28704     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM28700     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM11224     2  0.0000      0.988 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-2-a').click(function(){
  $('#tab-ATC-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-3'>
<p><a id='tab-ATC-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28710     2  0.4804      0.683 0.000 0.616 0.000 0.384
#&gt; GSM28711     2  0.4804      0.683 0.000 0.616 0.000 0.384
#&gt; GSM28712     2  0.3356      0.891 0.000 0.824 0.000 0.176
#&gt; GSM11222     3  0.0188      0.996 0.000 0.000 0.996 0.004
#&gt; GSM28720     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.3356      0.891 0.000 0.824 0.000 0.176
#&gt; GSM28714     2  0.3356      0.891 0.000 0.824 0.000 0.176
#&gt; GSM11216     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; GSM28715     2  0.3356      0.891 0.000 0.824 0.000 0.176
#&gt; GSM11234     2  0.3356      0.891 0.000 0.824 0.000 0.176
#&gt; GSM28699     2  0.0000      0.758 0.000 1.000 0.000 0.000
#&gt; GSM11233     2  0.0000      0.758 0.000 1.000 0.000 0.000
#&gt; GSM28718     2  0.3356      0.891 0.000 0.824 0.000 0.176
#&gt; GSM11231     2  0.3356      0.891 0.000 0.824 0.000 0.176
#&gt; GSM11237     2  0.3356      0.891 0.000 0.824 0.000 0.176
#&gt; GSM11228     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; GSM28697     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; GSM28698     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; GSM11238     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; GSM11242     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; GSM28719     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; GSM28708     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; GSM28722     2  0.3356      0.891 0.000 0.824 0.000 0.176
#&gt; GSM11232     2  0.3356      0.891 0.000 0.824 0.000 0.176
#&gt; GSM28709     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; GSM11226     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; GSM11239     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; GSM11225     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; GSM11220     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; GSM28701     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; GSM28721     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; GSM28713     2  0.3942      0.848 0.000 0.764 0.000 0.236
#&gt; GSM28716     2  0.4804      0.129 0.384 0.616 0.000 0.000
#&gt; GSM11221     2  0.4761      0.701 0.000 0.628 0.000 0.372
#&gt; GSM28717     2  0.0000      0.758 0.000 1.000 0.000 0.000
#&gt; GSM11223     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11218     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; GSM11219     2  0.3356      0.891 0.000 0.824 0.000 0.176
#&gt; GSM11236     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; GSM28702     3  0.0188      0.996 0.000 0.000 0.996 0.004
#&gt; GSM28705     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; GSM11230     2  0.3356      0.891 0.000 0.824 0.000 0.176
#&gt; GSM28704     2  0.4761      0.701 0.000 0.628 0.000 0.372
#&gt; GSM28700     2  0.3356      0.891 0.000 0.824 0.000 0.176
#&gt; GSM11224     2  0.3356      0.891 0.000 0.824 0.000 0.176
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-3-a').click(function(){
  $('#tab-ATC-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-4'>
<p><a id='tab-ATC-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28710     2  0.3177      0.781 0.000 0.792 0.000 0.208 0.000
#&gt; GSM28711     2  0.3177      0.781 0.000 0.792 0.000 0.208 0.000
#&gt; GSM28712     2  0.0000      0.944 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11222     3  0.0162      0.997 0.000 0.000 0.996 0.000 0.004
#&gt; GSM28720     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.0000      0.944 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28714     2  0.0000      0.944 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11216     3  0.0000      0.999 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28715     2  0.0000      0.944 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11234     2  0.0000      0.944 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28699     5  0.0162      0.851 0.000 0.004 0.000 0.000 0.996
#&gt; GSM11233     5  0.0963      0.837 0.000 0.036 0.000 0.000 0.964
#&gt; GSM28718     2  0.0000      0.944 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11231     2  0.0000      0.944 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11237     2  0.0000      0.944 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11228     4  0.0000      0.999 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28697     4  0.0000      0.999 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28698     3  0.0000      0.999 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11238     3  0.0000      0.999 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11242     3  0.0000      0.999 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28719     4  0.0000      0.999 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28708     4  0.0000      0.999 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28722     2  0.0000      0.944 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11232     2  0.0000      0.944 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28709     3  0.0000      0.999 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11226     4  0.0162      0.996 0.000 0.000 0.000 0.996 0.004
#&gt; GSM11239     3  0.0000      0.999 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11225     3  0.0000      0.999 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11220     3  0.0000      0.999 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28701     4  0.0000      0.999 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28721     4  0.0000      0.999 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28713     2  0.1410      0.904 0.000 0.940 0.000 0.060 0.000
#&gt; GSM28716     5  0.4138      0.373 0.384 0.000 0.000 0.000 0.616
#&gt; GSM11221     2  0.3074      0.794 0.000 0.804 0.000 0.196 0.000
#&gt; GSM28717     5  0.0162      0.851 0.000 0.004 0.000 0.000 0.996
#&gt; GSM11223     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     4  0.0162      0.996 0.000 0.000 0.000 0.996 0.004
#&gt; GSM11219     2  0.0000      0.944 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11236     4  0.0000      0.999 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28702     3  0.0162      0.997 0.000 0.000 0.996 0.000 0.004
#&gt; GSM28705     4  0.0000      0.999 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11230     2  0.0000      0.944 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28704     2  0.3074      0.794 0.000 0.804 0.000 0.196 0.000
#&gt; GSM28700     2  0.0000      0.944 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11224     2  0.0000      0.944 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-4-a').click(function(){
  $('#tab-ATC-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-5'>
<p><a id='tab-ATC-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28710     2  0.2854      0.768 0.000 0.792 0.000 0.208 0.000 0.000
#&gt; GSM28711     2  0.2854      0.768 0.000 0.792 0.000 0.208 0.000 0.000
#&gt; GSM28712     2  0.0000      0.943 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11222     3  0.0146      0.996 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM28720     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.0000      0.943 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28714     2  0.0000      0.943 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11216     3  0.0000      0.999 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28715     2  0.0000      0.943 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11234     2  0.0000      0.943 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28699     5  0.0000      0.648 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11233     5  0.0790      0.643 0.000 0.032 0.000 0.000 0.968 0.000
#&gt; GSM28718     2  0.0000      0.943 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11231     2  0.0000      0.943 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11237     2  0.0000      0.943 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11228     4  0.0000      0.998 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28697     4  0.0000      0.998 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28698     3  0.0000      0.999 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11238     3  0.0000      0.999 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11242     3  0.0000      0.999 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28719     4  0.0000      0.998 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28708     4  0.0000      0.998 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28722     2  0.0000      0.943 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11232     2  0.0000      0.943 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28709     3  0.0000      0.999 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11226     4  0.0146      0.993 0.000 0.000 0.000 0.996 0.000 0.004
#&gt; GSM11239     3  0.0000      0.999 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11225     3  0.0000      0.999 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11220     3  0.0000      0.999 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28701     4  0.0000      0.998 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28721     4  0.0000      0.998 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28713     2  0.1267      0.903 0.000 0.940 0.000 0.060 0.000 0.000
#&gt; GSM28716     5  0.3717      0.333 0.384 0.000 0.000 0.000 0.616 0.000
#&gt; GSM11221     2  0.2762      0.783 0.000 0.804 0.000 0.196 0.000 0.000
#&gt; GSM28717     5  0.3620      0.492 0.000 0.000 0.000 0.000 0.648 0.352
#&gt; GSM11223     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     4  0.0146      0.993 0.000 0.000 0.000 0.996 0.000 0.004
#&gt; GSM11219     2  0.0000      0.943 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11236     6  0.3634      0.000 0.000 0.000 0.000 0.356 0.000 0.644
#&gt; GSM28702     3  0.0146      0.996 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM28705     4  0.0000      0.998 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11230     2  0.0000      0.943 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28704     2  0.2762      0.783 0.000 0.804 0.000 0.196 0.000 0.000
#&gt; GSM28700     2  0.0000      0.943 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11224     2  0.0000      0.943 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-5-a').click(function(){
  $('#tab-ATC-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-hclust-signature_compare](figure_cola/ATC-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-hclust-collect-classes](figure_cola/ATC-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> ATC:hclust 54     0.398 2
#> ATC:hclust 53     0.373 3
#> ATC:hclust 53     0.353 4
#> ATC:hclust 53     0.336 5
#> ATC:hclust 51     0.333 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "kmeans"]
# you can also extract it by
# res = res_list["ATC:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21452 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-kmeans-collect-plots](figure_cola/ATC-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-kmeans-select-partition-number](figure_cola/ATC-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.484           0.750       0.820         0.3479 0.693   0.693
#> 3 3 1.000           1.000       1.000         0.6565 0.732   0.613
#> 4 4 0.780           0.918       0.932         0.2546 0.806   0.558
#> 5 5 0.795           0.722       0.878         0.0714 0.987   0.948
#> 6 6 0.766           0.762       0.851         0.0400 0.915   0.685
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-kmeans-get-classes-1'>
<p><a id='tab-ATC-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28710     2  0.9580      0.779 0.380 0.620
#&gt; GSM28711     2  0.9580      0.779 0.380 0.620
#&gt; GSM28712     2  0.9580      0.779 0.380 0.620
#&gt; GSM11222     1  0.0376      0.994 0.996 0.004
#&gt; GSM28720     2  0.3584      0.504 0.068 0.932
#&gt; GSM11217     2  0.3584      0.504 0.068 0.932
#&gt; GSM28723     2  0.3584      0.504 0.068 0.932
#&gt; GSM11241     2  0.3584      0.504 0.068 0.932
#&gt; GSM28703     2  0.3584      0.504 0.068 0.932
#&gt; GSM11227     2  0.3584      0.504 0.068 0.932
#&gt; GSM28706     2  0.3584      0.504 0.068 0.932
#&gt; GSM11229     2  0.3584      0.504 0.068 0.932
#&gt; GSM11235     2  0.3584      0.504 0.068 0.932
#&gt; GSM28707     2  0.3584      0.504 0.068 0.932
#&gt; GSM11240     2  0.9580      0.779 0.380 0.620
#&gt; GSM28714     2  0.9580      0.779 0.380 0.620
#&gt; GSM11216     1  0.0000      0.998 1.000 0.000
#&gt; GSM28715     2  0.9580      0.779 0.380 0.620
#&gt; GSM11234     2  0.9580      0.779 0.380 0.620
#&gt; GSM28699     2  0.6438      0.638 0.164 0.836
#&gt; GSM11233     2  0.9000      0.742 0.316 0.684
#&gt; GSM28718     2  0.9580      0.779 0.380 0.620
#&gt; GSM11231     2  0.9580      0.779 0.380 0.620
#&gt; GSM11237     2  0.9580      0.779 0.380 0.620
#&gt; GSM11228     2  0.9580      0.779 0.380 0.620
#&gt; GSM28697     2  0.9580      0.779 0.380 0.620
#&gt; GSM28698     1  0.0000      0.998 1.000 0.000
#&gt; GSM11238     1  0.0000      0.998 1.000 0.000
#&gt; GSM11242     1  0.0000      0.998 1.000 0.000
#&gt; GSM28719     2  0.9732      0.758 0.404 0.596
#&gt; GSM28708     2  0.9815      0.738 0.420 0.580
#&gt; GSM28722     2  0.9580      0.779 0.380 0.620
#&gt; GSM11232     2  0.9580      0.779 0.380 0.620
#&gt; GSM28709     1  0.0000      0.998 1.000 0.000
#&gt; GSM11226     2  0.9815      0.738 0.420 0.580
#&gt; GSM11239     1  0.0000      0.998 1.000 0.000
#&gt; GSM11225     1  0.0000      0.998 1.000 0.000
#&gt; GSM11220     1  0.0000      0.998 1.000 0.000
#&gt; GSM28701     2  0.9732      0.758 0.404 0.596
#&gt; GSM28721     2  0.9732      0.758 0.404 0.596
#&gt; GSM28713     2  0.9580      0.779 0.380 0.620
#&gt; GSM28716     2  0.0000      0.537 0.000 1.000
#&gt; GSM11221     2  0.9580      0.779 0.380 0.620
#&gt; GSM28717     2  0.9580      0.779 0.380 0.620
#&gt; GSM11223     2  0.3584      0.504 0.068 0.932
#&gt; GSM11218     2  0.9815      0.738 0.420 0.580
#&gt; GSM11219     2  0.9580      0.779 0.380 0.620
#&gt; GSM11236     2  0.9970      0.696 0.468 0.532
#&gt; GSM28702     1  0.0376      0.994 0.996 0.004
#&gt; GSM28705     2  0.9732      0.758 0.404 0.596
#&gt; GSM11230     2  0.9732      0.758 0.404 0.596
#&gt; GSM28704     2  0.9580      0.779 0.380 0.620
#&gt; GSM28700     2  0.9580      0.779 0.380 0.620
#&gt; GSM11224     2  0.9580      0.779 0.380 0.620
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-2'>
<p><a id='tab-ATC-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1 p2 p3
#&gt; GSM28710     2       0          1  0  1  0
#&gt; GSM28711     2       0          1  0  1  0
#&gt; GSM28712     2       0          1  0  1  0
#&gt; GSM11222     3       0          1  0  0  1
#&gt; GSM28720     1       0          1  1  0  0
#&gt; GSM11217     1       0          1  1  0  0
#&gt; GSM28723     1       0          1  1  0  0
#&gt; GSM11241     1       0          1  1  0  0
#&gt; GSM28703     1       0          1  1  0  0
#&gt; GSM11227     1       0          1  1  0  0
#&gt; GSM28706     1       0          1  1  0  0
#&gt; GSM11229     1       0          1  1  0  0
#&gt; GSM11235     1       0          1  1  0  0
#&gt; GSM28707     1       0          1  1  0  0
#&gt; GSM11240     2       0          1  0  1  0
#&gt; GSM28714     2       0          1  0  1  0
#&gt; GSM11216     3       0          1  0  0  1
#&gt; GSM28715     2       0          1  0  1  0
#&gt; GSM11234     2       0          1  0  1  0
#&gt; GSM28699     2       0          1  0  1  0
#&gt; GSM11233     2       0          1  0  1  0
#&gt; GSM28718     2       0          1  0  1  0
#&gt; GSM11231     2       0          1  0  1  0
#&gt; GSM11237     2       0          1  0  1  0
#&gt; GSM11228     2       0          1  0  1  0
#&gt; GSM28697     2       0          1  0  1  0
#&gt; GSM28698     3       0          1  0  0  1
#&gt; GSM11238     3       0          1  0  0  1
#&gt; GSM11242     3       0          1  0  0  1
#&gt; GSM28719     2       0          1  0  1  0
#&gt; GSM28708     2       0          1  0  1  0
#&gt; GSM28722     2       0          1  0  1  0
#&gt; GSM11232     2       0          1  0  1  0
#&gt; GSM28709     3       0          1  0  0  1
#&gt; GSM11226     2       0          1  0  1  0
#&gt; GSM11239     3       0          1  0  0  1
#&gt; GSM11225     3       0          1  0  0  1
#&gt; GSM11220     3       0          1  0  0  1
#&gt; GSM28701     2       0          1  0  1  0
#&gt; GSM28721     2       0          1  0  1  0
#&gt; GSM28713     2       0          1  0  1  0
#&gt; GSM28716     1       0          1  1  0  0
#&gt; GSM11221     2       0          1  0  1  0
#&gt; GSM28717     2       0          1  0  1  0
#&gt; GSM11223     1       0          1  1  0  0
#&gt; GSM11218     2       0          1  0  1  0
#&gt; GSM11219     2       0          1  0  1  0
#&gt; GSM11236     2       0          1  0  1  0
#&gt; GSM28702     3       0          1  0  0  1
#&gt; GSM28705     2       0          1  0  1  0
#&gt; GSM11230     2       0          1  0  1  0
#&gt; GSM28704     2       0          1  0  1  0
#&gt; GSM28700     2       0          1  0  1  0
#&gt; GSM11224     2       0          1  0  1  0
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-3'>
<p><a id='tab-ATC-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28710     2  0.3172      0.780 0.000 0.840 0.000 0.160
#&gt; GSM28711     4  0.4989      0.320 0.000 0.472 0.000 0.528
#&gt; GSM28712     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; GSM11222     3  0.3975      0.796 0.000 0.000 0.760 0.240
#&gt; GSM28720     1  0.0000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.1716      0.959 0.936 0.000 0.000 0.064
#&gt; GSM11241     1  0.0000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.1716      0.959 0.936 0.000 0.000 0.064
#&gt; GSM11229     1  0.0000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      0.985 1.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.0592      0.953 0.000 0.984 0.000 0.016
#&gt; GSM28714     2  0.0592      0.953 0.000 0.984 0.000 0.016
#&gt; GSM11216     3  0.1557      0.934 0.000 0.000 0.944 0.056
#&gt; GSM28715     2  0.0592      0.953 0.000 0.984 0.000 0.016
#&gt; GSM11234     2  0.0592      0.953 0.000 0.984 0.000 0.016
#&gt; GSM28699     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; GSM11233     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; GSM28718     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; GSM11231     2  0.0592      0.953 0.000 0.984 0.000 0.016
#&gt; GSM11237     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; GSM11228     4  0.2647      0.942 0.000 0.120 0.000 0.880
#&gt; GSM28697     4  0.2647      0.942 0.000 0.120 0.000 0.880
#&gt; GSM28698     3  0.0000      0.942 0.000 0.000 1.000 0.000
#&gt; GSM11238     3  0.0000      0.942 0.000 0.000 1.000 0.000
#&gt; GSM11242     3  0.0000      0.942 0.000 0.000 1.000 0.000
#&gt; GSM28719     4  0.2647      0.942 0.000 0.120 0.000 0.880
#&gt; GSM28708     4  0.2647      0.942 0.000 0.120 0.000 0.880
#&gt; GSM28722     2  0.0921      0.945 0.000 0.972 0.000 0.028
#&gt; GSM11232     4  0.4431      0.707 0.000 0.304 0.000 0.696
#&gt; GSM28709     3  0.1118      0.938 0.000 0.000 0.964 0.036
#&gt; GSM11226     4  0.2647      0.942 0.000 0.120 0.000 0.880
#&gt; GSM11239     3  0.0000      0.942 0.000 0.000 1.000 0.000
#&gt; GSM11225     3  0.0000      0.942 0.000 0.000 1.000 0.000
#&gt; GSM11220     3  0.1557      0.934 0.000 0.000 0.944 0.056
#&gt; GSM28701     4  0.2647      0.942 0.000 0.120 0.000 0.880
#&gt; GSM28721     4  0.2647      0.942 0.000 0.120 0.000 0.880
#&gt; GSM28713     2  0.0469      0.953 0.000 0.988 0.000 0.012
#&gt; GSM28716     2  0.5464      0.581 0.228 0.708 0.000 0.064
#&gt; GSM11221     2  0.3074      0.793 0.000 0.848 0.000 0.152
#&gt; GSM28717     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; GSM11223     1  0.1716      0.959 0.936 0.000 0.000 0.064
#&gt; GSM11218     4  0.2647      0.942 0.000 0.120 0.000 0.880
#&gt; GSM11219     2  0.0592      0.953 0.000 0.984 0.000 0.016
#&gt; GSM11236     4  0.2647      0.942 0.000 0.120 0.000 0.880
#&gt; GSM28702     3  0.3975      0.796 0.000 0.000 0.760 0.240
#&gt; GSM28705     4  0.2647      0.942 0.000 0.120 0.000 0.880
#&gt; GSM11230     4  0.2647      0.942 0.000 0.120 0.000 0.880
#&gt; GSM28704     2  0.0921      0.945 0.000 0.972 0.000 0.028
#&gt; GSM28700     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; GSM11224     2  0.0469      0.953 0.000 0.988 0.000 0.012
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-4'>
<p><a id='tab-ATC-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28710     2  0.3980     0.3892 0.000 0.708 0.000 0.284 0.008
#&gt; GSM28711     4  0.4434     0.2371 0.000 0.460 0.000 0.536 0.004
#&gt; GSM28712     2  0.2020     0.7392 0.000 0.900 0.000 0.000 0.100
#&gt; GSM11222     3  0.6229     0.6099 0.000 0.000 0.516 0.164 0.320
#&gt; GSM28720     1  0.0000     0.9239 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000     0.9239 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.3508     0.7662 0.748 0.000 0.000 0.000 0.252
#&gt; GSM11241     1  0.0000     0.9239 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000     0.9239 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000     0.9239 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.3508     0.7662 0.748 0.000 0.000 0.000 0.252
#&gt; GSM11229     1  0.0000     0.9239 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000     0.9239 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000     0.9239 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.2068     0.7503 0.000 0.904 0.000 0.004 0.092
#&gt; GSM28714     2  0.2068     0.7503 0.000 0.904 0.000 0.004 0.092
#&gt; GSM11216     3  0.3086     0.8287 0.000 0.000 0.816 0.004 0.180
#&gt; GSM28715     2  0.0162     0.7730 0.000 0.996 0.000 0.004 0.000
#&gt; GSM11234     2  0.0451     0.7717 0.000 0.988 0.000 0.004 0.008
#&gt; GSM28699     2  0.4227    -0.0552 0.000 0.580 0.000 0.000 0.420
#&gt; GSM11233     2  0.4219    -0.0297 0.000 0.584 0.000 0.000 0.416
#&gt; GSM28718     2  0.1965     0.7437 0.000 0.904 0.000 0.000 0.096
#&gt; GSM11231     2  0.0324     0.7730 0.000 0.992 0.000 0.004 0.004
#&gt; GSM11237     2  0.2020     0.7397 0.000 0.900 0.000 0.000 0.100
#&gt; GSM11228     4  0.1341     0.8638 0.000 0.056 0.000 0.944 0.000
#&gt; GSM28697     4  0.1341     0.8638 0.000 0.056 0.000 0.944 0.000
#&gt; GSM28698     3  0.0162     0.8612 0.000 0.000 0.996 0.004 0.000
#&gt; GSM11238     3  0.0000     0.8614 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11242     3  0.0000     0.8614 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28719     4  0.0898     0.8616 0.000 0.020 0.000 0.972 0.008
#&gt; GSM28708     4  0.0579     0.8573 0.000 0.008 0.000 0.984 0.008
#&gt; GSM28722     2  0.0324     0.7730 0.000 0.992 0.000 0.004 0.004
#&gt; GSM11232     4  0.4060     0.4911 0.000 0.360 0.000 0.640 0.000
#&gt; GSM28709     3  0.2179     0.8491 0.000 0.000 0.896 0.004 0.100
#&gt; GSM11226     4  0.2612     0.7841 0.000 0.008 0.000 0.868 0.124
#&gt; GSM11239     3  0.0000     0.8614 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11225     3  0.0162     0.8612 0.000 0.000 0.996 0.004 0.000
#&gt; GSM11220     3  0.3086     0.8287 0.000 0.000 0.816 0.004 0.180
#&gt; GSM28701     4  0.1341     0.8638 0.000 0.056 0.000 0.944 0.000
#&gt; GSM28721     4  0.0579     0.8573 0.000 0.008 0.000 0.984 0.008
#&gt; GSM28713     2  0.0451     0.7717 0.000 0.988 0.000 0.004 0.008
#&gt; GSM28716     5  0.4836     0.0000 0.044 0.304 0.000 0.000 0.652
#&gt; GSM11221     2  0.3835     0.4305 0.000 0.732 0.000 0.260 0.008
#&gt; GSM28717     2  0.4227    -0.0552 0.000 0.580 0.000 0.000 0.420
#&gt; GSM11223     1  0.3508     0.7662 0.748 0.000 0.000 0.000 0.252
#&gt; GSM11218     4  0.2612     0.7841 0.000 0.008 0.000 0.868 0.124
#&gt; GSM11219     2  0.0324     0.7730 0.000 0.992 0.000 0.004 0.004
#&gt; GSM11236     4  0.0290     0.8592 0.000 0.008 0.000 0.992 0.000
#&gt; GSM28702     3  0.6229     0.6099 0.000 0.000 0.516 0.164 0.320
#&gt; GSM28705     4  0.1341     0.8638 0.000 0.056 0.000 0.944 0.000
#&gt; GSM11230     4  0.2286     0.8253 0.000 0.108 0.000 0.888 0.004
#&gt; GSM28704     2  0.2077     0.6883 0.000 0.908 0.000 0.084 0.008
#&gt; GSM28700     2  0.1544     0.7555 0.000 0.932 0.000 0.000 0.068
#&gt; GSM11224     2  0.0324     0.7726 0.000 0.992 0.000 0.004 0.004
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-5'>
<p><a id='tab-ATC-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28710     2  0.4047      0.587 0.000 0.720 0.000 0.244 0.016 0.020
#&gt; GSM28711     2  0.4495      0.456 0.000 0.624 0.000 0.340 0.016 0.020
#&gt; GSM28712     2  0.2859      0.682 0.000 0.828 0.000 0.000 0.156 0.016
#&gt; GSM11222     6  0.5808      1.000 0.000 0.000 0.316 0.204 0.000 0.480
#&gt; GSM28720     1  0.0632      0.885 0.976 0.000 0.000 0.000 0.000 0.024
#&gt; GSM11217     1  0.0000      0.890 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.4687      0.671 0.632 0.000 0.000 0.000 0.072 0.296
#&gt; GSM11241     1  0.0000      0.890 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0632      0.885 0.976 0.000 0.000 0.000 0.000 0.024
#&gt; GSM11227     1  0.0000      0.890 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.4775      0.669 0.632 0.000 0.000 0.000 0.084 0.284
#&gt; GSM11229     1  0.0000      0.890 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      0.890 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      0.890 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.4209      0.620 0.000 0.736 0.000 0.000 0.160 0.104
#&gt; GSM28714     2  0.4243      0.616 0.000 0.732 0.000 0.000 0.164 0.104
#&gt; GSM11216     3  0.3651      0.737 0.000 0.000 0.772 0.000 0.048 0.180
#&gt; GSM28715     2  0.0146      0.779 0.000 0.996 0.000 0.004 0.000 0.000
#&gt; GSM11234     2  0.0665      0.777 0.000 0.980 0.000 0.008 0.008 0.004
#&gt; GSM28699     5  0.2697      0.858 0.000 0.188 0.000 0.000 0.812 0.000
#&gt; GSM11233     5  0.2631      0.853 0.000 0.180 0.000 0.000 0.820 0.000
#&gt; GSM28718     2  0.4243      0.616 0.000 0.732 0.000 0.000 0.164 0.104
#&gt; GSM11231     2  0.0551      0.779 0.000 0.984 0.000 0.004 0.008 0.004
#&gt; GSM11237     2  0.4276      0.610 0.000 0.728 0.000 0.000 0.168 0.104
#&gt; GSM11228     4  0.1787      0.814 0.000 0.068 0.000 0.920 0.008 0.004
#&gt; GSM28697     4  0.1787      0.814 0.000 0.068 0.000 0.920 0.008 0.004
#&gt; GSM28698     3  0.0260      0.878 0.000 0.000 0.992 0.000 0.008 0.000
#&gt; GSM11238     3  0.0000      0.879 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11242     3  0.0000      0.879 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28719     4  0.0909      0.811 0.000 0.000 0.000 0.968 0.012 0.020
#&gt; GSM28708     4  0.1334      0.805 0.000 0.000 0.000 0.948 0.020 0.032
#&gt; GSM28722     2  0.1078      0.776 0.000 0.964 0.000 0.012 0.008 0.016
#&gt; GSM11232     2  0.4321      0.497 0.000 0.652 0.000 0.316 0.012 0.020
#&gt; GSM28709     3  0.3044      0.796 0.000 0.000 0.836 0.000 0.048 0.116
#&gt; GSM11226     4  0.3802      0.455 0.000 0.000 0.000 0.676 0.012 0.312
#&gt; GSM11239     3  0.0000      0.879 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11225     3  0.0260      0.878 0.000 0.000 0.992 0.000 0.008 0.000
#&gt; GSM11220     3  0.3651      0.737 0.000 0.000 0.772 0.000 0.048 0.180
#&gt; GSM28701     4  0.1787      0.814 0.000 0.068 0.000 0.920 0.008 0.004
#&gt; GSM28721     4  0.1245      0.805 0.000 0.000 0.000 0.952 0.016 0.032
#&gt; GSM28713     2  0.0665      0.777 0.000 0.980 0.000 0.004 0.008 0.008
#&gt; GSM28716     5  0.5264      0.567 0.012 0.104 0.000 0.000 0.612 0.272
#&gt; GSM11221     2  0.3801      0.605 0.000 0.740 0.000 0.232 0.016 0.012
#&gt; GSM28717     5  0.3014      0.857 0.000 0.184 0.000 0.000 0.804 0.012
#&gt; GSM11223     1  0.4775      0.669 0.632 0.000 0.000 0.000 0.084 0.284
#&gt; GSM11218     4  0.3802      0.455 0.000 0.000 0.000 0.676 0.012 0.312
#&gt; GSM11219     2  0.0405      0.778 0.000 0.988 0.000 0.000 0.004 0.008
#&gt; GSM11236     4  0.1924      0.800 0.000 0.004 0.000 0.920 0.028 0.048
#&gt; GSM28702     6  0.5808      1.000 0.000 0.000 0.316 0.204 0.000 0.480
#&gt; GSM28705     4  0.1584      0.816 0.000 0.064 0.000 0.928 0.008 0.000
#&gt; GSM11230     4  0.4796      0.616 0.000 0.176 0.000 0.708 0.024 0.092
#&gt; GSM28704     2  0.1692      0.757 0.000 0.932 0.000 0.048 0.008 0.012
#&gt; GSM28700     2  0.2070      0.726 0.000 0.892 0.000 0.000 0.100 0.008
#&gt; GSM11224     2  0.0405      0.777 0.000 0.988 0.000 0.000 0.004 0.008
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-kmeans-signature_compare](figure_cola/ATC-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-kmeans-collect-classes](figure_cola/ATC-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> ATC:kmeans 54     0.398 2
#> ATC:kmeans 54     0.374 3
#> ATC:kmeans 53     0.353 4
#> ATC:kmeans 46     0.343 5
#> ATC:kmeans 50     0.315 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:skmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "skmeans"]
# you can also extract it by
# res = res_list["ATC:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21452 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-skmeans-collect-plots](figure_cola/ATC-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-skmeans-select-partition-number](figure_cola/ATC-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.993       0.997         0.4862 0.516   0.516
#> 3 3 1.000           0.959       0.982         0.3647 0.795   0.614
#> 4 4 0.941           0.921       0.964         0.1062 0.931   0.797
#> 5 5 0.881           0.838       0.861         0.0701 0.950   0.814
#> 6 6 0.868           0.772       0.866         0.0375 0.973   0.879
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-skmeans-get-classes-1'>
<p><a id='tab-ATC-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette   p1   p2
#&gt; GSM28710     2    0.00      0.994 0.00 1.00
#&gt; GSM28711     2    0.00      0.994 0.00 1.00
#&gt; GSM28712     2    0.00      0.994 0.00 1.00
#&gt; GSM11222     1    0.00      1.000 1.00 0.00
#&gt; GSM28720     2    0.00      0.994 0.00 1.00
#&gt; GSM11217     2    0.00      0.994 0.00 1.00
#&gt; GSM28723     2    0.00      0.994 0.00 1.00
#&gt; GSM11241     2    0.00      0.994 0.00 1.00
#&gt; GSM28703     2    0.00      0.994 0.00 1.00
#&gt; GSM11227     2    0.00      0.994 0.00 1.00
#&gt; GSM28706     2    0.00      0.994 0.00 1.00
#&gt; GSM11229     2    0.00      0.994 0.00 1.00
#&gt; GSM11235     2    0.00      0.994 0.00 1.00
#&gt; GSM28707     2    0.00      0.994 0.00 1.00
#&gt; GSM11240     2    0.00      0.994 0.00 1.00
#&gt; GSM28714     2    0.00      0.994 0.00 1.00
#&gt; GSM11216     1    0.00      1.000 1.00 0.00
#&gt; GSM28715     2    0.00      0.994 0.00 1.00
#&gt; GSM11234     2    0.00      0.994 0.00 1.00
#&gt; GSM28699     2    0.00      0.994 0.00 1.00
#&gt; GSM11233     2    0.00      0.994 0.00 1.00
#&gt; GSM28718     2    0.00      0.994 0.00 1.00
#&gt; GSM11231     2    0.00      0.994 0.00 1.00
#&gt; GSM11237     2    0.00      0.994 0.00 1.00
#&gt; GSM11228     1    0.00      1.000 1.00 0.00
#&gt; GSM28697     1    0.00      1.000 1.00 0.00
#&gt; GSM28698     1    0.00      1.000 1.00 0.00
#&gt; GSM11238     1    0.00      1.000 1.00 0.00
#&gt; GSM11242     1    0.00      1.000 1.00 0.00
#&gt; GSM28719     1    0.00      1.000 1.00 0.00
#&gt; GSM28708     1    0.00      1.000 1.00 0.00
#&gt; GSM28722     2    0.00      0.994 0.00 1.00
#&gt; GSM11232     2    0.68      0.780 0.18 0.82
#&gt; GSM28709     1    0.00      1.000 1.00 0.00
#&gt; GSM11226     1    0.00      1.000 1.00 0.00
#&gt; GSM11239     1    0.00      1.000 1.00 0.00
#&gt; GSM11225     1    0.00      1.000 1.00 0.00
#&gt; GSM11220     1    0.00      1.000 1.00 0.00
#&gt; GSM28701     1    0.00      1.000 1.00 0.00
#&gt; GSM28721     1    0.00      1.000 1.00 0.00
#&gt; GSM28713     2    0.00      0.994 0.00 1.00
#&gt; GSM28716     2    0.00      0.994 0.00 1.00
#&gt; GSM11221     2    0.00      0.994 0.00 1.00
#&gt; GSM28717     2    0.00      0.994 0.00 1.00
#&gt; GSM11223     2    0.00      0.994 0.00 1.00
#&gt; GSM11218     1    0.00      1.000 1.00 0.00
#&gt; GSM11219     2    0.00      0.994 0.00 1.00
#&gt; GSM11236     1    0.00      1.000 1.00 0.00
#&gt; GSM28702     1    0.00      1.000 1.00 0.00
#&gt; GSM28705     1    0.00      1.000 1.00 0.00
#&gt; GSM11230     1    0.00      1.000 1.00 0.00
#&gt; GSM28704     2    0.00      0.994 0.00 1.00
#&gt; GSM28700     2    0.00      0.994 0.00 1.00
#&gt; GSM11224     2    0.00      0.994 0.00 1.00
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-2'>
<p><a id='tab-ATC-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28710     2   0.000      0.953 0.000 1.000 0.000
#&gt; GSM28711     2   0.000      0.953 0.000 1.000 0.000
#&gt; GSM28712     2   0.000      0.953 0.000 1.000 0.000
#&gt; GSM11222     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM28720     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM11217     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM28723     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM11241     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM28703     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM11227     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM28706     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM11229     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM11235     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM28707     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM11240     2   0.000      0.953 0.000 1.000 0.000
#&gt; GSM28714     2   0.000      0.953 0.000 1.000 0.000
#&gt; GSM11216     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM28715     2   0.000      0.953 0.000 1.000 0.000
#&gt; GSM11234     2   0.000      0.953 0.000 1.000 0.000
#&gt; GSM28699     2   0.588      0.498 0.348 0.652 0.000
#&gt; GSM11233     2   0.000      0.953 0.000 1.000 0.000
#&gt; GSM28718     2   0.000      0.953 0.000 1.000 0.000
#&gt; GSM11231     2   0.000      0.953 0.000 1.000 0.000
#&gt; GSM11237     2   0.000      0.953 0.000 1.000 0.000
#&gt; GSM11228     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM28697     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM28698     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM11238     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM11242     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM28719     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM28708     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM28722     2   0.000      0.953 0.000 1.000 0.000
#&gt; GSM11232     2   0.000      0.953 0.000 1.000 0.000
#&gt; GSM28709     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM11226     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM11239     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM11225     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM11220     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM28701     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM28721     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM28713     2   0.000      0.953 0.000 1.000 0.000
#&gt; GSM28716     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM11221     2   0.000      0.953 0.000 1.000 0.000
#&gt; GSM28717     2   0.543      0.618 0.284 0.716 0.000
#&gt; GSM11223     1   0.000      1.000 1.000 0.000 0.000
#&gt; GSM11218     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM11219     2   0.000      0.953 0.000 1.000 0.000
#&gt; GSM11236     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM28702     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM28705     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM11230     2   0.568      0.549 0.000 0.684 0.316
#&gt; GSM28704     2   0.000      0.953 0.000 1.000 0.000
#&gt; GSM28700     2   0.000      0.953 0.000 1.000 0.000
#&gt; GSM11224     2   0.000      0.953 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-3'>
<p><a id='tab-ATC-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28710     2  0.0000      0.942 0.000 1.000 0.000 0.000
#&gt; GSM28711     2  0.0000      0.942 0.000 1.000 0.000 0.000
#&gt; GSM28712     2  0.0000      0.942 0.000 1.000 0.000 0.000
#&gt; GSM11222     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28720     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.0000      0.942 0.000 1.000 0.000 0.000
#&gt; GSM28714     2  0.0000      0.942 0.000 1.000 0.000 0.000
#&gt; GSM11216     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28715     2  0.0000      0.942 0.000 1.000 0.000 0.000
#&gt; GSM11234     2  0.0000      0.942 0.000 1.000 0.000 0.000
#&gt; GSM28699     2  0.4917      0.514 0.336 0.656 0.000 0.008
#&gt; GSM11233     2  0.0336      0.937 0.000 0.992 0.000 0.008
#&gt; GSM28718     2  0.0000      0.942 0.000 1.000 0.000 0.000
#&gt; GSM11231     2  0.0000      0.942 0.000 1.000 0.000 0.000
#&gt; GSM11237     2  0.0000      0.942 0.000 1.000 0.000 0.000
#&gt; GSM11228     4  0.0336      0.906 0.000 0.000 0.008 0.992
#&gt; GSM28697     4  0.0336      0.906 0.000 0.000 0.008 0.992
#&gt; GSM28698     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11238     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11242     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28719     4  0.0336      0.906 0.000 0.000 0.008 0.992
#&gt; GSM28708     4  0.2345      0.856 0.000 0.000 0.100 0.900
#&gt; GSM28722     2  0.0000      0.942 0.000 1.000 0.000 0.000
#&gt; GSM11232     2  0.1867      0.880 0.000 0.928 0.000 0.072
#&gt; GSM28709     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11226     4  0.4543      0.613 0.000 0.000 0.324 0.676
#&gt; GSM11239     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11225     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11220     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28701     4  0.0336      0.906 0.000 0.000 0.008 0.992
#&gt; GSM28721     4  0.0336      0.906 0.000 0.000 0.008 0.992
#&gt; GSM28713     2  0.0000      0.942 0.000 1.000 0.000 0.000
#&gt; GSM28716     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11221     2  0.0000      0.942 0.000 1.000 0.000 0.000
#&gt; GSM28717     2  0.4567      0.624 0.276 0.716 0.000 0.008
#&gt; GSM11223     1  0.0000      1.000 1.000 0.000 0.000 0.000
#&gt; GSM11218     4  0.4543      0.613 0.000 0.000 0.324 0.676
#&gt; GSM11219     2  0.0000      0.942 0.000 1.000 0.000 0.000
#&gt; GSM11236     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28702     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28705     4  0.0336      0.906 0.000 0.000 0.008 0.992
#&gt; GSM11230     2  0.6574      0.239 0.000 0.548 0.088 0.364
#&gt; GSM28704     2  0.0000      0.942 0.000 1.000 0.000 0.000
#&gt; GSM28700     2  0.0000      0.942 0.000 1.000 0.000 0.000
#&gt; GSM11224     2  0.0000      0.942 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-4'>
<p><a id='tab-ATC-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28710     2  0.2069      0.807 0.000 0.912 0.000 0.012 0.076
#&gt; GSM28711     2  0.4086      0.665 0.000 0.736 0.000 0.024 0.240
#&gt; GSM28712     2  0.3210      0.719 0.000 0.788 0.000 0.000 0.212
#&gt; GSM11222     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28720     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.3837      0.568 0.000 0.692 0.000 0.000 0.308
#&gt; GSM28714     2  0.3913      0.544 0.000 0.676 0.000 0.000 0.324
#&gt; GSM11216     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28715     2  0.0162      0.833 0.000 0.996 0.000 0.000 0.004
#&gt; GSM11234     2  0.0880      0.830 0.000 0.968 0.000 0.000 0.032
#&gt; GSM28699     5  0.2769      0.774 0.032 0.092 0.000 0.000 0.876
#&gt; GSM11233     5  0.2127      0.757 0.000 0.108 0.000 0.000 0.892
#&gt; GSM28718     2  0.4192      0.407 0.000 0.596 0.000 0.000 0.404
#&gt; GSM11231     2  0.0963      0.833 0.000 0.964 0.000 0.000 0.036
#&gt; GSM11237     2  0.4192      0.407 0.000 0.596 0.000 0.000 0.404
#&gt; GSM11228     4  0.0510      0.809 0.000 0.000 0.000 0.984 0.016
#&gt; GSM28697     4  0.0912      0.805 0.000 0.012 0.000 0.972 0.016
#&gt; GSM28698     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11238     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11242     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28719     4  0.1043      0.808 0.000 0.000 0.000 0.960 0.040
#&gt; GSM28708     4  0.3641      0.743 0.000 0.000 0.120 0.820 0.060
#&gt; GSM28722     2  0.1341      0.829 0.000 0.944 0.000 0.000 0.056
#&gt; GSM11232     2  0.1444      0.814 0.000 0.948 0.000 0.012 0.040
#&gt; GSM28709     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11226     4  0.5518      0.440 0.000 0.000 0.384 0.544 0.072
#&gt; GSM11239     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11225     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11220     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28701     4  0.0912      0.805 0.000 0.012 0.000 0.972 0.016
#&gt; GSM28721     4  0.1502      0.804 0.000 0.000 0.004 0.940 0.056
#&gt; GSM28713     2  0.0794      0.830 0.000 0.972 0.000 0.000 0.028
#&gt; GSM28716     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11221     2  0.1478      0.816 0.000 0.936 0.000 0.000 0.064
#&gt; GSM28717     5  0.2654      0.775 0.032 0.084 0.000 0.000 0.884
#&gt; GSM11223     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     4  0.5510      0.449 0.000 0.000 0.380 0.548 0.072
#&gt; GSM11219     2  0.0963      0.832 0.000 0.964 0.000 0.000 0.036
#&gt; GSM11236     3  0.0162      0.996 0.000 0.000 0.996 0.000 0.004
#&gt; GSM28702     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28705     4  0.0162      0.812 0.000 0.000 0.000 0.996 0.004
#&gt; GSM11230     5  0.8172      0.244 0.000 0.280 0.112 0.248 0.360
#&gt; GSM28704     2  0.0880      0.827 0.000 0.968 0.000 0.000 0.032
#&gt; GSM28700     2  0.1341      0.831 0.000 0.944 0.000 0.000 0.056
#&gt; GSM11224     2  0.1043      0.833 0.000 0.960 0.000 0.000 0.040
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-5'>
<p><a id='tab-ATC-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28710     2  0.3927      0.653 0.000 0.776 0.000 0.028 0.032 0.164
#&gt; GSM28711     6  0.5959     -0.336 0.000 0.396 0.000 0.032 0.104 0.468
#&gt; GSM28712     2  0.3136      0.654 0.000 0.768 0.000 0.000 0.228 0.004
#&gt; GSM11222     3  0.0713      0.968 0.000 0.000 0.972 0.000 0.000 0.028
#&gt; GSM28720     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.5767      0.375 0.000 0.484 0.000 0.000 0.192 0.324
#&gt; GSM28714     2  0.5942      0.305 0.000 0.424 0.000 0.000 0.220 0.356
#&gt; GSM11216     3  0.0000      0.989 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28715     2  0.1168      0.749 0.000 0.956 0.000 0.000 0.016 0.028
#&gt; GSM11234     2  0.0972      0.748 0.000 0.964 0.000 0.000 0.008 0.028
#&gt; GSM28699     5  0.0914      0.972 0.016 0.016 0.000 0.000 0.968 0.000
#&gt; GSM11233     5  0.0547      0.978 0.000 0.020 0.000 0.000 0.980 0.000
#&gt; GSM28718     2  0.6076      0.251 0.000 0.384 0.000 0.000 0.272 0.344
#&gt; GSM11231     2  0.1745      0.748 0.000 0.924 0.000 0.000 0.020 0.056
#&gt; GSM11237     2  0.6083      0.253 0.000 0.388 0.000 0.000 0.280 0.332
#&gt; GSM11228     4  0.0458      0.814 0.000 0.000 0.000 0.984 0.000 0.016
#&gt; GSM28697     4  0.0603      0.808 0.000 0.004 0.000 0.980 0.000 0.016
#&gt; GSM28698     3  0.0000      0.989 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11238     3  0.0000      0.989 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11242     3  0.0000      0.989 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28719     4  0.3101      0.733 0.000 0.000 0.000 0.756 0.000 0.244
#&gt; GSM28708     4  0.5121      0.525 0.000 0.000 0.124 0.604 0.000 0.272
#&gt; GSM28722     2  0.3104      0.692 0.000 0.800 0.000 0.000 0.016 0.184
#&gt; GSM11232     2  0.2376      0.723 0.000 0.884 0.000 0.012 0.008 0.096
#&gt; GSM28709     3  0.0000      0.989 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11226     6  0.6031      0.170 0.000 0.000 0.296 0.280 0.000 0.424
#&gt; GSM11239     3  0.0000      0.989 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11225     3  0.0000      0.989 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11220     3  0.0000      0.989 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28701     4  0.0458      0.811 0.000 0.000 0.000 0.984 0.000 0.016
#&gt; GSM28721     4  0.3215      0.726 0.000 0.000 0.004 0.756 0.000 0.240
#&gt; GSM28713     2  0.0547      0.747 0.000 0.980 0.000 0.000 0.000 0.020
#&gt; GSM28716     1  0.0363      0.988 0.988 0.000 0.000 0.000 0.012 0.000
#&gt; GSM11221     2  0.3092      0.697 0.000 0.852 0.000 0.016 0.044 0.088
#&gt; GSM28717     5  0.0692      0.983 0.004 0.020 0.000 0.000 0.976 0.000
#&gt; GSM11223     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     6  0.6032      0.160 0.000 0.000 0.288 0.288 0.000 0.424
#&gt; GSM11219     2  0.2088      0.743 0.000 0.904 0.000 0.000 0.028 0.068
#&gt; GSM11236     3  0.1082      0.953 0.000 0.000 0.956 0.000 0.004 0.040
#&gt; GSM28702     3  0.0790      0.965 0.000 0.000 0.968 0.000 0.000 0.032
#&gt; GSM28705     4  0.0713      0.816 0.000 0.000 0.000 0.972 0.000 0.028
#&gt; GSM11230     6  0.4304      0.205 0.000 0.072 0.040 0.040 0.048 0.800
#&gt; GSM28704     2  0.1668      0.730 0.000 0.928 0.000 0.004 0.008 0.060
#&gt; GSM28700     2  0.2145      0.744 0.000 0.900 0.000 0.000 0.072 0.028
#&gt; GSM11224     2  0.1572      0.751 0.000 0.936 0.000 0.000 0.036 0.028
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-skmeans-signature_compare](figure_cola/ATC-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-skmeans-collect-classes](figure_cola/ATC-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>              n tissue(p) k
#> ATC:skmeans 54     0.398 2
#> ATC:skmeans 53     0.373 3
#> ATC:skmeans 53     0.353 4
#> ATC:skmeans 49     0.406 5
#> ATC:skmeans 46     0.403 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:pam*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "pam"]
# you can also extract it by
# res = res_list["ATC:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21452 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-pam-collect-plots](figure_cola/ATC-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-pam-select-partition-number](figure_cola/ATC-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.993       0.997         0.3493 0.648   0.648
#> 3 3 1.000           0.991       0.997         0.6426 0.776   0.655
#> 4 4 0.909           0.918       0.964         0.2356 0.798   0.555
#> 5 5 0.784           0.804       0.888         0.0790 0.917   0.717
#> 6 6 0.749           0.770       0.870         0.0448 0.926   0.700
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-classes'>
<ul>
<li><a href='#tab-ATC-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-pam-get-classes-1'>
<p><a id='tab-ATC-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28710     2   0.000      1.000 0.000 1.000
#&gt; GSM28711     2   0.000      1.000 0.000 1.000
#&gt; GSM28712     2   0.000      1.000 0.000 1.000
#&gt; GSM11222     2   0.000      1.000 0.000 1.000
#&gt; GSM28720     1   0.000      0.985 1.000 0.000
#&gt; GSM11217     1   0.000      0.985 1.000 0.000
#&gt; GSM28723     1   0.000      0.985 1.000 0.000
#&gt; GSM11241     1   0.000      0.985 1.000 0.000
#&gt; GSM28703     1   0.000      0.985 1.000 0.000
#&gt; GSM11227     1   0.000      0.985 1.000 0.000
#&gt; GSM28706     1   0.000      0.985 1.000 0.000
#&gt; GSM11229     1   0.000      0.985 1.000 0.000
#&gt; GSM11235     1   0.000      0.985 1.000 0.000
#&gt; GSM28707     1   0.000      0.985 1.000 0.000
#&gt; GSM11240     2   0.000      1.000 0.000 1.000
#&gt; GSM28714     2   0.000      1.000 0.000 1.000
#&gt; GSM11216     2   0.000      1.000 0.000 1.000
#&gt; GSM28715     2   0.000      1.000 0.000 1.000
#&gt; GSM11234     2   0.000      1.000 0.000 1.000
#&gt; GSM28699     2   0.000      1.000 0.000 1.000
#&gt; GSM11233     2   0.000      1.000 0.000 1.000
#&gt; GSM28718     2   0.000      1.000 0.000 1.000
#&gt; GSM11231     2   0.000      1.000 0.000 1.000
#&gt; GSM11237     2   0.000      1.000 0.000 1.000
#&gt; GSM11228     2   0.000      1.000 0.000 1.000
#&gt; GSM28697     2   0.000      1.000 0.000 1.000
#&gt; GSM28698     2   0.000      1.000 0.000 1.000
#&gt; GSM11238     2   0.000      1.000 0.000 1.000
#&gt; GSM11242     2   0.000      1.000 0.000 1.000
#&gt; GSM28719     2   0.000      1.000 0.000 1.000
#&gt; GSM28708     2   0.000      1.000 0.000 1.000
#&gt; GSM28722     2   0.000      1.000 0.000 1.000
#&gt; GSM11232     2   0.000      1.000 0.000 1.000
#&gt; GSM28709     2   0.000      1.000 0.000 1.000
#&gt; GSM11226     2   0.000      1.000 0.000 1.000
#&gt; GSM11239     2   0.000      1.000 0.000 1.000
#&gt; GSM11225     2   0.000      1.000 0.000 1.000
#&gt; GSM11220     2   0.000      1.000 0.000 1.000
#&gt; GSM28701     2   0.000      1.000 0.000 1.000
#&gt; GSM28721     2   0.000      1.000 0.000 1.000
#&gt; GSM28713     2   0.000      1.000 0.000 1.000
#&gt; GSM28716     1   0.644      0.804 0.836 0.164
#&gt; GSM11221     2   0.000      1.000 0.000 1.000
#&gt; GSM28717     2   0.000      1.000 0.000 1.000
#&gt; GSM11223     1   0.000      0.985 1.000 0.000
#&gt; GSM11218     2   0.000      1.000 0.000 1.000
#&gt; GSM11219     2   0.000      1.000 0.000 1.000
#&gt; GSM11236     2   0.000      1.000 0.000 1.000
#&gt; GSM28702     2   0.000      1.000 0.000 1.000
#&gt; GSM28705     2   0.000      1.000 0.000 1.000
#&gt; GSM11230     2   0.000      1.000 0.000 1.000
#&gt; GSM28704     2   0.000      1.000 0.000 1.000
#&gt; GSM28700     2   0.000      1.000 0.000 1.000
#&gt; GSM11224     2   0.000      1.000 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-1-a').click(function(){
  $('#tab-ATC-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-2'>
<p><a id='tab-ATC-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2 p3
#&gt; GSM28710     2   0.000      1.000 0.000 1.000  0
#&gt; GSM28711     2   0.000      1.000 0.000 1.000  0
#&gt; GSM28712     2   0.000      1.000 0.000 1.000  0
#&gt; GSM11222     3   0.000      1.000 0.000 0.000  1
#&gt; GSM28720     1   0.000      0.978 1.000 0.000  0
#&gt; GSM11217     1   0.000      0.978 1.000 0.000  0
#&gt; GSM28723     1   0.000      0.978 1.000 0.000  0
#&gt; GSM11241     1   0.000      0.978 1.000 0.000  0
#&gt; GSM28703     1   0.000      0.978 1.000 0.000  0
#&gt; GSM11227     1   0.000      0.978 1.000 0.000  0
#&gt; GSM28706     1   0.000      0.978 1.000 0.000  0
#&gt; GSM11229     1   0.000      0.978 1.000 0.000  0
#&gt; GSM11235     1   0.000      0.978 1.000 0.000  0
#&gt; GSM28707     1   0.000      0.978 1.000 0.000  0
#&gt; GSM11240     2   0.000      1.000 0.000 1.000  0
#&gt; GSM28714     2   0.000      1.000 0.000 1.000  0
#&gt; GSM11216     3   0.000      1.000 0.000 0.000  1
#&gt; GSM28715     2   0.000      1.000 0.000 1.000  0
#&gt; GSM11234     2   0.000      1.000 0.000 1.000  0
#&gt; GSM28699     2   0.000      1.000 0.000 1.000  0
#&gt; GSM11233     2   0.000      1.000 0.000 1.000  0
#&gt; GSM28718     2   0.000      1.000 0.000 1.000  0
#&gt; GSM11231     2   0.000      1.000 0.000 1.000  0
#&gt; GSM11237     2   0.000      1.000 0.000 1.000  0
#&gt; GSM11228     2   0.000      1.000 0.000 1.000  0
#&gt; GSM28697     2   0.000      1.000 0.000 1.000  0
#&gt; GSM28698     3   0.000      1.000 0.000 0.000  1
#&gt; GSM11238     3   0.000      1.000 0.000 0.000  1
#&gt; GSM11242     3   0.000      1.000 0.000 0.000  1
#&gt; GSM28719     2   0.000      1.000 0.000 1.000  0
#&gt; GSM28708     2   0.000      1.000 0.000 1.000  0
#&gt; GSM28722     2   0.000      1.000 0.000 1.000  0
#&gt; GSM11232     2   0.000      1.000 0.000 1.000  0
#&gt; GSM28709     3   0.000      1.000 0.000 0.000  1
#&gt; GSM11226     2   0.000      1.000 0.000 1.000  0
#&gt; GSM11239     3   0.000      1.000 0.000 0.000  1
#&gt; GSM11225     3   0.000      1.000 0.000 0.000  1
#&gt; GSM11220     3   0.000      1.000 0.000 0.000  1
#&gt; GSM28701     2   0.000      1.000 0.000 1.000  0
#&gt; GSM28721     2   0.000      1.000 0.000 1.000  0
#&gt; GSM28713     2   0.000      1.000 0.000 1.000  0
#&gt; GSM28716     1   0.412      0.748 0.832 0.168  0
#&gt; GSM11221     2   0.000      1.000 0.000 1.000  0
#&gt; GSM28717     2   0.000      1.000 0.000 1.000  0
#&gt; GSM11223     1   0.000      0.978 1.000 0.000  0
#&gt; GSM11218     2   0.000      1.000 0.000 1.000  0
#&gt; GSM11219     2   0.000      1.000 0.000 1.000  0
#&gt; GSM11236     2   0.000      1.000 0.000 1.000  0
#&gt; GSM28702     3   0.000      1.000 0.000 0.000  1
#&gt; GSM28705     2   0.000      1.000 0.000 1.000  0
#&gt; GSM11230     2   0.000      1.000 0.000 1.000  0
#&gt; GSM28704     2   0.000      1.000 0.000 1.000  0
#&gt; GSM28700     2   0.000      1.000 0.000 1.000  0
#&gt; GSM11224     2   0.000      1.000 0.000 1.000  0
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-2-a').click(function(){
  $('#tab-ATC-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-3'>
<p><a id='tab-ATC-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2 p3    p4
#&gt; GSM28710     2  0.0000      0.972 0.000 1.000  0 0.000
#&gt; GSM28711     2  0.0000      0.972 0.000 1.000  0 0.000
#&gt; GSM28712     2  0.0000      0.972 0.000 1.000  0 0.000
#&gt; GSM11222     4  0.0000      0.825 0.000 0.000  0 1.000
#&gt; GSM28720     1  0.0000      1.000 1.000 0.000  0 0.000
#&gt; GSM11217     1  0.0000      1.000 1.000 0.000  0 0.000
#&gt; GSM28723     1  0.0000      1.000 1.000 0.000  0 0.000
#&gt; GSM11241     1  0.0000      1.000 1.000 0.000  0 0.000
#&gt; GSM28703     1  0.0000      1.000 1.000 0.000  0 0.000
#&gt; GSM11227     1  0.0000      1.000 1.000 0.000  0 0.000
#&gt; GSM28706     1  0.0000      1.000 1.000 0.000  0 0.000
#&gt; GSM11229     1  0.0000      1.000 1.000 0.000  0 0.000
#&gt; GSM11235     1  0.0000      1.000 1.000 0.000  0 0.000
#&gt; GSM28707     1  0.0000      1.000 1.000 0.000  0 0.000
#&gt; GSM11240     2  0.0000      0.972 0.000 1.000  0 0.000
#&gt; GSM28714     2  0.0000      0.972 0.000 1.000  0 0.000
#&gt; GSM11216     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; GSM28715     2  0.0000      0.972 0.000 1.000  0 0.000
#&gt; GSM11234     2  0.0000      0.972 0.000 1.000  0 0.000
#&gt; GSM28699     2  0.0000      0.972 0.000 1.000  0 0.000
#&gt; GSM11233     2  0.0000      0.972 0.000 1.000  0 0.000
#&gt; GSM28718     2  0.0000      0.972 0.000 1.000  0 0.000
#&gt; GSM11231     2  0.0000      0.972 0.000 1.000  0 0.000
#&gt; GSM11237     2  0.0000      0.972 0.000 1.000  0 0.000
#&gt; GSM11228     2  0.2704      0.812 0.000 0.876  0 0.124
#&gt; GSM28697     4  0.0188      0.824 0.000 0.004  0 0.996
#&gt; GSM28698     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; GSM11238     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; GSM11242     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; GSM28719     4  0.4382      0.669 0.000 0.296  0 0.704
#&gt; GSM28708     4  0.3726      0.744 0.000 0.212  0 0.788
#&gt; GSM28722     2  0.0000      0.972 0.000 1.000  0 0.000
#&gt; GSM11232     2  0.0000      0.972 0.000 1.000  0 0.000
#&gt; GSM28709     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; GSM11226     4  0.0000      0.825 0.000 0.000  0 1.000
#&gt; GSM11239     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; GSM11225     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; GSM11220     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; GSM28701     4  0.2589      0.792 0.000 0.116  0 0.884
#&gt; GSM28721     4  0.0000      0.825 0.000 0.000  0 1.000
#&gt; GSM28713     2  0.0000      0.972 0.000 1.000  0 0.000
#&gt; GSM28716     2  0.4776      0.382 0.376 0.624  0 0.000
#&gt; GSM11221     2  0.0000      0.972 0.000 1.000  0 0.000
#&gt; GSM28717     2  0.0000      0.972 0.000 1.000  0 0.000
#&gt; GSM11223     1  0.0000      1.000 1.000 0.000  0 0.000
#&gt; GSM11218     4  0.0000      0.825 0.000 0.000  0 1.000
#&gt; GSM11219     2  0.0000      0.972 0.000 1.000  0 0.000
#&gt; GSM11236     4  0.4877      0.479 0.000 0.408  0 0.592
#&gt; GSM28702     4  0.0000      0.825 0.000 0.000  0 1.000
#&gt; GSM28705     4  0.0000      0.825 0.000 0.000  0 1.000
#&gt; GSM11230     4  0.4855      0.497 0.000 0.400  0 0.600
#&gt; GSM28704     2  0.0000      0.972 0.000 1.000  0 0.000
#&gt; GSM28700     2  0.0000      0.972 0.000 1.000  0 0.000
#&gt; GSM11224     2  0.0000      0.972 0.000 1.000  0 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-3-a').click(function(){
  $('#tab-ATC-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-4'>
<p><a id='tab-ATC-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28710     2  0.1792      0.794 0.000 0.916 0.000 0.084 0.000
#&gt; GSM28711     2  0.2648      0.769 0.000 0.848 0.000 0.152 0.000
#&gt; GSM28712     2  0.0000      0.807 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11222     4  0.3586      0.615 0.000 0.000 0.000 0.736 0.264
#&gt; GSM28720     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.0000      0.807 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28714     2  0.2270      0.755 0.000 0.904 0.000 0.020 0.076
#&gt; GSM11216     3  0.1792      0.918 0.000 0.000 0.916 0.000 0.084
#&gt; GSM28715     2  0.0000      0.807 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11234     2  0.0000      0.807 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28699     5  0.4126      0.877 0.000 0.380 0.000 0.000 0.620
#&gt; GSM11233     5  0.3612      0.850 0.000 0.268 0.000 0.000 0.732
#&gt; GSM28718     2  0.2377      0.670 0.000 0.872 0.000 0.000 0.128
#&gt; GSM11231     2  0.0162      0.807 0.000 0.996 0.000 0.004 0.000
#&gt; GSM11237     5  0.4088      0.819 0.000 0.368 0.000 0.000 0.632
#&gt; GSM11228     2  0.3636      0.677 0.000 0.728 0.000 0.272 0.000
#&gt; GSM28697     4  0.0000      0.769 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28698     3  0.0000      0.963 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11238     3  0.0000      0.963 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11242     3  0.0000      0.963 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28719     4  0.4613      0.336 0.000 0.360 0.000 0.620 0.020
#&gt; GSM28708     4  0.3143      0.635 0.000 0.204 0.000 0.796 0.000
#&gt; GSM28722     2  0.3395      0.723 0.000 0.764 0.000 0.236 0.000
#&gt; GSM11232     2  0.3424      0.718 0.000 0.760 0.000 0.240 0.000
#&gt; GSM28709     3  0.0162      0.961 0.000 0.000 0.996 0.000 0.004
#&gt; GSM11226     4  0.1270      0.766 0.000 0.000 0.000 0.948 0.052
#&gt; GSM11239     3  0.0000      0.963 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11225     3  0.0000      0.963 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11220     3  0.3305      0.807 0.000 0.000 0.776 0.000 0.224
#&gt; GSM28701     4  0.2761      0.730 0.000 0.104 0.000 0.872 0.024
#&gt; GSM28721     4  0.0510      0.770 0.000 0.000 0.000 0.984 0.016
#&gt; GSM28713     2  0.0000      0.807 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28716     5  0.5461      0.833 0.096 0.284 0.000 0.000 0.620
#&gt; GSM11221     2  0.3143      0.742 0.000 0.796 0.000 0.204 0.000
#&gt; GSM28717     5  0.4126      0.877 0.000 0.380 0.000 0.000 0.620
#&gt; GSM11223     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     4  0.1478      0.762 0.000 0.000 0.000 0.936 0.064
#&gt; GSM11219     2  0.0000      0.807 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11236     2  0.4302      0.169 0.000 0.520 0.000 0.480 0.000
#&gt; GSM28702     4  0.3586      0.615 0.000 0.000 0.000 0.736 0.264
#&gt; GSM28705     4  0.0000      0.769 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11230     4  0.5115     -0.103 0.000 0.480 0.000 0.484 0.036
#&gt; GSM28704     2  0.3395      0.723 0.000 0.764 0.000 0.236 0.000
#&gt; GSM28700     2  0.0000      0.807 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11224     2  0.0000      0.807 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-4-a').click(function(){
  $('#tab-ATC-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-5'>
<p><a id='tab-ATC-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28710     2   0.324      0.651 0.000 0.732 0.000 0.268 0.000 0.000
#&gt; GSM28711     2   0.327      0.681 0.000 0.760 0.000 0.232 0.008 0.000
#&gt; GSM28712     2   0.000      0.821 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11222     6   0.238      0.748 0.000 0.000 0.000 0.152 0.000 0.848
#&gt; GSM28720     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2   0.000      0.821 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28714     2   0.209      0.753 0.000 0.876 0.000 0.000 0.124 0.000
#&gt; GSM11216     3   0.348      0.642 0.000 0.000 0.684 0.000 0.000 0.316
#&gt; GSM28715     2   0.000      0.821 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11234     2   0.000      0.821 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28699     5   0.249      0.810 0.000 0.164 0.000 0.000 0.836 0.000
#&gt; GSM11233     5   0.000      0.697 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28718     2   0.263      0.691 0.000 0.820 0.000 0.000 0.180 0.000
#&gt; GSM11231     2   0.079      0.815 0.000 0.968 0.000 0.032 0.000 0.000
#&gt; GSM11237     5   0.341      0.542 0.000 0.300 0.000 0.000 0.700 0.000
#&gt; GSM11228     4   0.238      0.743 0.000 0.152 0.000 0.848 0.000 0.000
#&gt; GSM28697     4   0.000      0.716 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28698     3   0.000      0.925 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11238     3   0.000      0.925 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11242     3   0.000      0.925 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28719     4   0.420      0.542 0.000 0.316 0.000 0.652 0.000 0.032
#&gt; GSM28708     4   0.214      0.758 0.000 0.128 0.000 0.872 0.000 0.000
#&gt; GSM28722     2   0.368      0.517 0.000 0.628 0.000 0.372 0.000 0.000
#&gt; GSM11232     2   0.371      0.502 0.000 0.620 0.000 0.380 0.000 0.000
#&gt; GSM28709     3   0.234      0.837 0.000 0.000 0.852 0.000 0.000 0.148
#&gt; GSM11226     4   0.385     -0.161 0.000 0.000 0.000 0.540 0.000 0.460
#&gt; GSM11239     3   0.000      0.925 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11225     3   0.000      0.925 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11220     6   0.294      0.368 0.000 0.000 0.220 0.000 0.000 0.780
#&gt; GSM28701     4   0.370      0.724 0.000 0.112 0.000 0.788 0.000 0.100
#&gt; GSM28721     4   0.171      0.657 0.000 0.000 0.000 0.908 0.000 0.092
#&gt; GSM28713     2   0.000      0.821 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28716     5   0.297      0.793 0.036 0.128 0.000 0.000 0.836 0.000
#&gt; GSM11221     2   0.196      0.784 0.000 0.888 0.000 0.112 0.000 0.000
#&gt; GSM28717     5   0.249      0.810 0.000 0.164 0.000 0.000 0.836 0.000
#&gt; GSM11223     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     6   0.362      0.447 0.000 0.000 0.000 0.352 0.000 0.648
#&gt; GSM11219     2   0.000      0.821 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11236     4   0.226      0.757 0.000 0.140 0.000 0.860 0.000 0.000
#&gt; GSM28702     6   0.238      0.748 0.000 0.000 0.000 0.152 0.000 0.848
#&gt; GSM28705     4   0.000      0.716 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11230     2   0.514      0.546 0.000 0.640 0.000 0.260 0.024 0.076
#&gt; GSM28704     2   0.358      0.564 0.000 0.660 0.000 0.340 0.000 0.000
#&gt; GSM28700     2   0.000      0.821 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11224     2   0.000      0.821 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-5-a').click(function(){
  $('#tab-ATC-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-pam-signature_compare](figure_cola/ATC-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-pam-collect-classes](figure_cola/ATC-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n tissue(p) k
#> ATC:pam 54     0.398 2
#> ATC:pam 54     0.374 3
#> ATC:pam 51     0.350 4
#> ATC:pam 51     0.457 5
#> ATC:pam 51     0.426 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "mclust"]
# you can also extract it by
# res = res_list["ATC:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21452 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-mclust-collect-plots](figure_cola/ATC-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-mclust-select-partition-number](figure_cola/ATC-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.788           0.856       0.935         0.4322 0.535   0.535
#> 3 3 1.000           0.989       0.994         0.3661 0.835   0.703
#> 4 4 0.752           0.808       0.876         0.1404 0.901   0.769
#> 5 5 0.758           0.819       0.881         0.1521 0.824   0.530
#> 6 6 0.785           0.737       0.877         0.0628 0.889   0.560
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-classes'>
<ul>
<li><a href='#tab-ATC-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-mclust-get-classes-1'>
<p><a id='tab-ATC-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28710     2  0.0000      0.969 0.000 1.000
#&gt; GSM28711     2  0.0000      0.969 0.000 1.000
#&gt; GSM28712     2  0.0000      0.969 0.000 1.000
#&gt; GSM11222     2  0.0000      0.969 0.000 1.000
#&gt; GSM28720     1  0.0000      0.836 1.000 0.000
#&gt; GSM11217     1  0.0000      0.836 1.000 0.000
#&gt; GSM28723     1  0.0000      0.836 1.000 0.000
#&gt; GSM11241     1  0.0000      0.836 1.000 0.000
#&gt; GSM28703     1  0.0000      0.836 1.000 0.000
#&gt; GSM11227     1  0.0000      0.836 1.000 0.000
#&gt; GSM28706     1  0.0000      0.836 1.000 0.000
#&gt; GSM11229     1  0.0000      0.836 1.000 0.000
#&gt; GSM11235     1  0.0000      0.836 1.000 0.000
#&gt; GSM28707     1  0.0000      0.836 1.000 0.000
#&gt; GSM11240     2  0.0000      0.969 0.000 1.000
#&gt; GSM28714     2  0.1843      0.941 0.028 0.972
#&gt; GSM11216     2  0.9996     -0.253 0.488 0.512
#&gt; GSM28715     2  0.0000      0.969 0.000 1.000
#&gt; GSM11234     2  0.0000      0.969 0.000 1.000
#&gt; GSM28699     2  0.2603      0.923 0.044 0.956
#&gt; GSM11233     2  0.1184      0.954 0.016 0.984
#&gt; GSM28718     2  0.0938      0.958 0.012 0.988
#&gt; GSM11231     2  0.0000      0.969 0.000 1.000
#&gt; GSM11237     2  0.0000      0.969 0.000 1.000
#&gt; GSM11228     2  0.0000      0.969 0.000 1.000
#&gt; GSM28697     2  0.0000      0.969 0.000 1.000
#&gt; GSM28698     1  0.9522      0.588 0.628 0.372
#&gt; GSM11238     1  0.9522      0.588 0.628 0.372
#&gt; GSM11242     1  0.9522      0.588 0.628 0.372
#&gt; GSM28719     2  0.0000      0.969 0.000 1.000
#&gt; GSM28708     2  0.0000      0.969 0.000 1.000
#&gt; GSM28722     2  0.0000      0.969 0.000 1.000
#&gt; GSM11232     2  0.0000      0.969 0.000 1.000
#&gt; GSM28709     1  0.9522      0.588 0.628 0.372
#&gt; GSM11226     2  0.0000      0.969 0.000 1.000
#&gt; GSM11239     1  0.9522      0.588 0.628 0.372
#&gt; GSM11225     1  0.9522      0.588 0.628 0.372
#&gt; GSM11220     1  0.9608      0.564 0.616 0.384
#&gt; GSM28701     2  0.0000      0.969 0.000 1.000
#&gt; GSM28721     2  0.0000      0.969 0.000 1.000
#&gt; GSM28713     2  0.0000      0.969 0.000 1.000
#&gt; GSM28716     1  0.2778      0.811 0.952 0.048
#&gt; GSM11221     2  0.0000      0.969 0.000 1.000
#&gt; GSM28717     2  0.0000      0.969 0.000 1.000
#&gt; GSM11223     1  0.0000      0.836 1.000 0.000
#&gt; GSM11218     2  0.0000      0.969 0.000 1.000
#&gt; GSM11219     2  0.0000      0.969 0.000 1.000
#&gt; GSM11236     2  0.8386      0.520 0.268 0.732
#&gt; GSM28702     2  0.0000      0.969 0.000 1.000
#&gt; GSM28705     2  0.0000      0.969 0.000 1.000
#&gt; GSM11230     2  0.0000      0.969 0.000 1.000
#&gt; GSM28704     2  0.0000      0.969 0.000 1.000
#&gt; GSM28700     2  0.0000      0.969 0.000 1.000
#&gt; GSM11224     2  0.0000      0.969 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-1-a').click(function(){
  $('#tab-ATC-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-2'>
<p><a id='tab-ATC-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28710     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM28711     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM28712     2  0.0237      0.995 0.004 0.996 0.000
#&gt; GSM11222     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM28720     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM11217     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM28723     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM11241     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM28703     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM11227     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM28706     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM11229     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM11235     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM28707     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM11240     2  0.0237      0.995 0.004 0.996 0.000
#&gt; GSM28714     2  0.0237      0.995 0.004 0.996 0.000
#&gt; GSM11216     3  0.0000      0.989 0.000 0.000 1.000
#&gt; GSM28715     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM11234     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM28699     3  0.1620      0.966 0.012 0.024 0.964
#&gt; GSM11233     2  0.2446      0.934 0.012 0.936 0.052
#&gt; GSM28718     2  0.0237      0.995 0.004 0.996 0.000
#&gt; GSM11231     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM11237     2  0.0237      0.995 0.004 0.996 0.000
#&gt; GSM11228     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM28697     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM28698     3  0.0000      0.989 0.000 0.000 1.000
#&gt; GSM11238     3  0.0000      0.989 0.000 0.000 1.000
#&gt; GSM11242     3  0.0000      0.989 0.000 0.000 1.000
#&gt; GSM28719     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM28708     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM28722     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM11232     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM28709     3  0.0000      0.989 0.000 0.000 1.000
#&gt; GSM11226     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM11239     3  0.0000      0.989 0.000 0.000 1.000
#&gt; GSM11225     3  0.0000      0.989 0.000 0.000 1.000
#&gt; GSM11220     3  0.0000      0.989 0.000 0.000 1.000
#&gt; GSM28701     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM28721     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM28713     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM28716     1  0.3846      0.862 0.876 0.016 0.108
#&gt; GSM11221     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM28717     3  0.1267      0.970 0.004 0.024 0.972
#&gt; GSM11223     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM11218     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM11219     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM11236     3  0.1267      0.970 0.004 0.024 0.972
#&gt; GSM28702     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM28705     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM11230     2  0.0237      0.995 0.004 0.996 0.000
#&gt; GSM28704     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM28700     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM11224     2  0.0000      0.997 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-2-a').click(function(){
  $('#tab-ATC-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-3'>
<p><a id='tab-ATC-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28710     2  0.2868      0.813 0.000 0.864 0.000 0.136
#&gt; GSM28711     2  0.3801      0.847 0.000 0.780 0.000 0.220
#&gt; GSM28712     2  0.4134      0.854 0.000 0.740 0.000 0.260
#&gt; GSM11222     3  0.5764      0.318 0.000 0.452 0.520 0.028
#&gt; GSM28720     1  0.0000      0.905 1.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      0.905 1.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0336      0.902 0.992 0.000 0.000 0.008
#&gt; GSM11241     1  0.0000      0.905 1.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      0.905 1.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      0.905 1.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0336      0.902 0.992 0.000 0.000 0.008
#&gt; GSM11229     1  0.0000      0.905 1.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      0.905 1.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      0.905 1.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.4072      0.857 0.000 0.748 0.000 0.252
#&gt; GSM28714     2  0.4382      0.822 0.000 0.704 0.000 0.296
#&gt; GSM11216     3  0.0188      0.854 0.000 0.000 0.996 0.004
#&gt; GSM28715     2  0.3942      0.863 0.000 0.764 0.000 0.236
#&gt; GSM11234     2  0.3907      0.863 0.000 0.768 0.000 0.232
#&gt; GSM28699     4  0.0592      0.973 0.000 0.000 0.016 0.984
#&gt; GSM11233     4  0.0188      0.982 0.000 0.004 0.000 0.996
#&gt; GSM28718     2  0.4304      0.834 0.000 0.716 0.000 0.284
#&gt; GSM11231     2  0.3942      0.863 0.000 0.764 0.000 0.236
#&gt; GSM11237     2  0.4304      0.834 0.000 0.716 0.000 0.284
#&gt; GSM11228     2  0.1302      0.762 0.000 0.956 0.000 0.044
#&gt; GSM28697     2  0.1302      0.762 0.000 0.956 0.000 0.044
#&gt; GSM28698     3  0.0000      0.856 0.000 0.000 1.000 0.000
#&gt; GSM11238     3  0.0000      0.856 0.000 0.000 1.000 0.000
#&gt; GSM11242     3  0.0000      0.856 0.000 0.000 1.000 0.000
#&gt; GSM28719     2  0.0188      0.785 0.000 0.996 0.000 0.004
#&gt; GSM28708     2  0.0707      0.777 0.000 0.980 0.000 0.020
#&gt; GSM28722     2  0.3942      0.863 0.000 0.764 0.000 0.236
#&gt; GSM11232     2  0.3528      0.861 0.000 0.808 0.000 0.192
#&gt; GSM28709     3  0.0000      0.856 0.000 0.000 1.000 0.000
#&gt; GSM11226     2  0.0188      0.785 0.000 0.996 0.000 0.004
#&gt; GSM11239     3  0.0000      0.856 0.000 0.000 1.000 0.000
#&gt; GSM11225     3  0.0000      0.856 0.000 0.000 1.000 0.000
#&gt; GSM11220     3  0.0188      0.854 0.000 0.000 0.996 0.004
#&gt; GSM28701     2  0.1302      0.762 0.000 0.956 0.000 0.044
#&gt; GSM28721     2  0.0707      0.776 0.000 0.980 0.000 0.020
#&gt; GSM28713     2  0.3942      0.863 0.000 0.764 0.000 0.236
#&gt; GSM28716     1  0.5392      0.189 0.528 0.012 0.000 0.460
#&gt; GSM11221     2  0.3837      0.850 0.000 0.776 0.000 0.224
#&gt; GSM28717     4  0.0000      0.984 0.000 0.000 0.000 1.000
#&gt; GSM11223     1  0.0336      0.902 0.992 0.000 0.000 0.008
#&gt; GSM11218     2  0.0188      0.785 0.000 0.996 0.000 0.004
#&gt; GSM11219     2  0.3975      0.862 0.000 0.760 0.000 0.240
#&gt; GSM11236     1  0.9425     -0.118 0.360 0.116 0.204 0.320
#&gt; GSM28702     3  0.5764      0.318 0.000 0.452 0.520 0.028
#&gt; GSM28705     2  0.0592      0.778 0.000 0.984 0.000 0.016
#&gt; GSM11230     2  0.4008      0.861 0.000 0.756 0.000 0.244
#&gt; GSM28704     2  0.3837      0.864 0.000 0.776 0.000 0.224
#&gt; GSM28700     2  0.4008      0.860 0.000 0.756 0.000 0.244
#&gt; GSM11224     2  0.3942      0.863 0.000 0.764 0.000 0.236
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-3-a').click(function(){
  $('#tab-ATC-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-4'>
<p><a id='tab-ATC-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28710     4  0.3508      0.672 0.000 0.252 0.000 0.748 0.000
#&gt; GSM28711     4  0.3452      0.684 0.000 0.244 0.000 0.756 0.000
#&gt; GSM28712     2  0.3980      0.678 0.000 0.796 0.000 0.076 0.128
#&gt; GSM11222     4  0.4308      0.697 0.000 0.012 0.072 0.788 0.128
#&gt; GSM28720     1  0.0000      0.965 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      0.965 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      0.965 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      0.965 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      0.965 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      0.965 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0162      0.962 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11229     1  0.0000      0.965 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      0.965 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      0.965 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.2732      0.666 0.000 0.840 0.000 0.000 0.160
#&gt; GSM28714     2  0.2773      0.662 0.000 0.836 0.000 0.000 0.164
#&gt; GSM11216     3  0.0609      0.924 0.000 0.000 0.980 0.000 0.020
#&gt; GSM28715     2  0.2516      0.795 0.000 0.860 0.000 0.140 0.000
#&gt; GSM11234     2  0.3508      0.732 0.000 0.748 0.000 0.252 0.000
#&gt; GSM28699     5  0.3691      0.905 0.000 0.156 0.000 0.040 0.804
#&gt; GSM11233     5  0.4000      0.861 0.000 0.228 0.000 0.024 0.748
#&gt; GSM28718     2  0.2732      0.666 0.000 0.840 0.000 0.000 0.160
#&gt; GSM11231     2  0.3242      0.786 0.000 0.784 0.000 0.216 0.000
#&gt; GSM11237     2  0.2677      0.696 0.000 0.872 0.000 0.016 0.112
#&gt; GSM11228     4  0.1410      0.834 0.000 0.060 0.000 0.940 0.000
#&gt; GSM28697     4  0.1410      0.834 0.000 0.060 0.000 0.940 0.000
#&gt; GSM28698     3  0.0000      0.932 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11238     3  0.0290      0.929 0.000 0.000 0.992 0.000 0.008
#&gt; GSM11242     3  0.0000      0.932 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28719     4  0.1671      0.843 0.000 0.076 0.000 0.924 0.000
#&gt; GSM28708     4  0.1671      0.843 0.000 0.076 0.000 0.924 0.000
#&gt; GSM28722     2  0.2690      0.793 0.000 0.844 0.000 0.156 0.000
#&gt; GSM11232     2  0.2966      0.783 0.000 0.816 0.000 0.184 0.000
#&gt; GSM28709     3  0.0000      0.932 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11226     4  0.1671      0.843 0.000 0.076 0.000 0.924 0.000
#&gt; GSM11239     3  0.0000      0.932 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11225     3  0.0000      0.932 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11220     3  0.0609      0.924 0.000 0.000 0.980 0.000 0.020
#&gt; GSM28701     4  0.1410      0.834 0.000 0.060 0.000 0.940 0.000
#&gt; GSM28721     4  0.1732      0.843 0.000 0.080 0.000 0.920 0.000
#&gt; GSM28713     2  0.3796      0.729 0.000 0.700 0.000 0.300 0.000
#&gt; GSM28716     1  0.5996      0.475 0.656 0.040 0.000 0.108 0.196
#&gt; GSM11221     4  0.3508      0.672 0.000 0.252 0.000 0.748 0.000
#&gt; GSM28717     5  0.2377      0.880 0.000 0.128 0.000 0.000 0.872
#&gt; GSM11223     1  0.0290      0.959 0.992 0.000 0.000 0.000 0.008
#&gt; GSM11218     4  0.1671      0.843 0.000 0.076 0.000 0.924 0.000
#&gt; GSM11219     2  0.3289      0.791 0.000 0.844 0.000 0.108 0.048
#&gt; GSM11236     3  0.7172      0.284 0.024 0.020 0.536 0.236 0.184
#&gt; GSM28702     4  0.4308      0.697 0.000 0.012 0.072 0.788 0.128
#&gt; GSM28705     4  0.1851      0.845 0.000 0.088 0.000 0.912 0.000
#&gt; GSM11230     2  0.3093      0.655 0.000 0.824 0.000 0.008 0.168
#&gt; GSM28704     2  0.3143      0.780 0.000 0.796 0.000 0.204 0.000
#&gt; GSM28700     2  0.4752      0.768 0.000 0.724 0.000 0.184 0.092
#&gt; GSM11224     2  0.2732      0.791 0.000 0.840 0.000 0.160 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-4-a').click(function(){
  $('#tab-ATC-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-5'>
<p><a id='tab-ATC-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28710     2  0.0291     0.8972 0.000 0.992 0.000 0.004 0.000 0.004
#&gt; GSM28711     2  0.0508     0.8938 0.000 0.984 0.000 0.012 0.000 0.004
#&gt; GSM28712     5  0.2402     0.5438 0.000 0.140 0.000 0.000 0.856 0.004
#&gt; GSM11222     4  0.3050     0.6348 0.000 0.000 0.000 0.764 0.000 0.236
#&gt; GSM28720     1  0.0000     0.9716 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0260     0.9722 0.992 0.000 0.000 0.000 0.000 0.008
#&gt; GSM28723     1  0.1141     0.9475 0.948 0.000 0.000 0.000 0.000 0.052
#&gt; GSM11241     1  0.0000     0.9716 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000     0.9716 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0260     0.9722 0.992 0.000 0.000 0.000 0.000 0.008
#&gt; GSM28706     1  0.1204     0.9451 0.944 0.000 0.000 0.000 0.000 0.056
#&gt; GSM11229     1  0.0260     0.9722 0.992 0.000 0.000 0.000 0.000 0.008
#&gt; GSM11235     1  0.0260     0.9722 0.992 0.000 0.000 0.000 0.000 0.008
#&gt; GSM28707     1  0.0260     0.9722 0.992 0.000 0.000 0.000 0.000 0.008
#&gt; GSM11240     5  0.0458     0.6316 0.000 0.016 0.000 0.000 0.984 0.000
#&gt; GSM28714     5  0.0458     0.6316 0.000 0.016 0.000 0.000 0.984 0.000
#&gt; GSM11216     3  0.2941     0.7934 0.000 0.000 0.780 0.000 0.000 0.220
#&gt; GSM28715     2  0.0858     0.8949 0.000 0.968 0.000 0.004 0.028 0.000
#&gt; GSM11234     2  0.0146     0.8982 0.000 0.996 0.000 0.004 0.000 0.000
#&gt; GSM28699     5  0.3838    -0.1417 0.000 0.000 0.000 0.000 0.552 0.448
#&gt; GSM11233     5  0.4319     0.0269 0.000 0.024 0.000 0.000 0.576 0.400
#&gt; GSM28718     5  0.0458     0.6316 0.000 0.016 0.000 0.000 0.984 0.000
#&gt; GSM11231     2  0.2473     0.7942 0.000 0.856 0.000 0.136 0.008 0.000
#&gt; GSM11237     5  0.2595     0.5101 0.000 0.160 0.000 0.000 0.836 0.004
#&gt; GSM11228     4  0.3584     0.6678 0.000 0.308 0.000 0.688 0.000 0.004
#&gt; GSM28697     4  0.3584     0.6678 0.000 0.308 0.000 0.688 0.000 0.004
#&gt; GSM28698     3  0.0000     0.9351 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11238     3  0.0000     0.9351 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11242     3  0.0000     0.9351 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28719     4  0.0260     0.7680 0.000 0.008 0.000 0.992 0.000 0.000
#&gt; GSM28708     4  0.2088     0.7296 0.000 0.000 0.000 0.904 0.028 0.068
#&gt; GSM28722     2  0.0777     0.8960 0.000 0.972 0.000 0.004 0.024 0.000
#&gt; GSM11232     2  0.0713     0.8943 0.000 0.972 0.000 0.028 0.000 0.000
#&gt; GSM28709     3  0.0260     0.9327 0.000 0.000 0.992 0.000 0.000 0.008
#&gt; GSM11226     4  0.0363     0.7653 0.000 0.000 0.000 0.988 0.012 0.000
#&gt; GSM11239     3  0.0000     0.9351 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11225     3  0.0146     0.9333 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM11220     3  0.2941     0.7934 0.000 0.000 0.780 0.000 0.000 0.220
#&gt; GSM28701     4  0.3584     0.6678 0.000 0.308 0.000 0.688 0.000 0.004
#&gt; GSM28721     4  0.2048     0.7591 0.000 0.120 0.000 0.880 0.000 0.000
#&gt; GSM28713     2  0.0937     0.8849 0.000 0.960 0.000 0.040 0.000 0.000
#&gt; GSM28716     6  0.5616     0.2651 0.008 0.148 0.000 0.000 0.288 0.556
#&gt; GSM11221     2  0.0291     0.8972 0.000 0.992 0.000 0.004 0.000 0.004
#&gt; GSM28717     6  0.3862    -0.1066 0.000 0.000 0.000 0.000 0.476 0.524
#&gt; GSM11223     1  0.2562     0.8365 0.828 0.000 0.000 0.000 0.000 0.172
#&gt; GSM11218     4  0.0000     0.7648 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11219     2  0.2100     0.8328 0.000 0.884 0.000 0.004 0.112 0.000
#&gt; GSM11236     6  0.6475     0.1515 0.000 0.056 0.004 0.132 0.308 0.500
#&gt; GSM28702     4  0.3050     0.6348 0.000 0.000 0.000 0.764 0.000 0.236
#&gt; GSM28705     4  0.2730     0.7347 0.000 0.192 0.000 0.808 0.000 0.000
#&gt; GSM11230     5  0.3608     0.3247 0.000 0.012 0.000 0.004 0.736 0.248
#&gt; GSM28704     2  0.3215     0.6734 0.000 0.756 0.000 0.240 0.004 0.000
#&gt; GSM28700     2  0.3482     0.4622 0.000 0.684 0.000 0.000 0.316 0.000
#&gt; GSM11224     2  0.0790     0.8926 0.000 0.968 0.000 0.000 0.032 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-5-a').click(function(){
  $('#tab-ATC-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-mclust-signature_compare](figure_cola/ATC-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-mclust-collect-classes](figure_cola/ATC-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> ATC:mclust 53     0.397 2
#> ATC:mclust 54     0.374 3
#> ATC:mclust 50     0.349 4
#> ATC:mclust 52     0.334 5
#> ATC:mclust 47     0.458 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:NMF*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "NMF"]
# you can also extract it by
# res = res_list["ATC:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21452 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-NMF-collect-plots](figure_cola/ATC-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-NMF-select-partition-number](figure_cola/ATC-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.859           0.941       0.971         0.4890 0.516   0.516
#> 3 3 0.941           0.937       0.974         0.2831 0.653   0.438
#> 4 4 0.854           0.865       0.934         0.1965 0.837   0.582
#> 5 5 0.746           0.692       0.836         0.0455 0.952   0.811
#> 6 6 0.758           0.627       0.779         0.0312 0.922   0.683
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-classes'>
<ul>
<li><a href='#tab-ATC-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-NMF-get-classes-1'>
<p><a id='tab-ATC-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28710     2  0.2043      0.942 0.032 0.968
#&gt; GSM28711     2  0.7815      0.740 0.232 0.768
#&gt; GSM28712     2  0.0000      0.955 0.000 1.000
#&gt; GSM11222     1  0.0000      0.992 1.000 0.000
#&gt; GSM28720     2  0.0000      0.955 0.000 1.000
#&gt; GSM11217     2  0.0000      0.955 0.000 1.000
#&gt; GSM28723     2  0.0000      0.955 0.000 1.000
#&gt; GSM11241     2  0.0000      0.955 0.000 1.000
#&gt; GSM28703     2  0.0000      0.955 0.000 1.000
#&gt; GSM11227     2  0.0000      0.955 0.000 1.000
#&gt; GSM28706     2  0.0000      0.955 0.000 1.000
#&gt; GSM11229     2  0.0000      0.955 0.000 1.000
#&gt; GSM11235     2  0.0000      0.955 0.000 1.000
#&gt; GSM28707     2  0.0000      0.955 0.000 1.000
#&gt; GSM11240     2  0.3431      0.922 0.064 0.936
#&gt; GSM28714     2  0.6343      0.835 0.160 0.840
#&gt; GSM11216     1  0.0000      0.992 1.000 0.000
#&gt; GSM28715     2  0.5519      0.869 0.128 0.872
#&gt; GSM11234     2  0.0672      0.953 0.008 0.992
#&gt; GSM28699     2  0.0000      0.955 0.000 1.000
#&gt; GSM11233     2  0.0000      0.955 0.000 1.000
#&gt; GSM28718     2  0.0672      0.953 0.008 0.992
#&gt; GSM11231     2  0.2043      0.942 0.032 0.968
#&gt; GSM11237     2  0.0000      0.955 0.000 1.000
#&gt; GSM11228     1  0.4939      0.872 0.892 0.108
#&gt; GSM28697     1  0.2778      0.944 0.952 0.048
#&gt; GSM28698     1  0.0000      0.992 1.000 0.000
#&gt; GSM11238     1  0.0000      0.992 1.000 0.000
#&gt; GSM11242     1  0.0000      0.992 1.000 0.000
#&gt; GSM28719     1  0.0000      0.992 1.000 0.000
#&gt; GSM28708     1  0.0000      0.992 1.000 0.000
#&gt; GSM28722     2  0.5629      0.866 0.132 0.868
#&gt; GSM11232     2  0.9896      0.292 0.440 0.560
#&gt; GSM28709     1  0.0000      0.992 1.000 0.000
#&gt; GSM11226     1  0.0000      0.992 1.000 0.000
#&gt; GSM11239     1  0.0000      0.992 1.000 0.000
#&gt; GSM11225     1  0.0000      0.992 1.000 0.000
#&gt; GSM11220     1  0.0000      0.992 1.000 0.000
#&gt; GSM28701     1  0.0000      0.992 1.000 0.000
#&gt; GSM28721     1  0.0000      0.992 1.000 0.000
#&gt; GSM28713     2  0.0000      0.955 0.000 1.000
#&gt; GSM28716     2  0.0000      0.955 0.000 1.000
#&gt; GSM11221     2  0.2043      0.942 0.032 0.968
#&gt; GSM28717     2  0.0000      0.955 0.000 1.000
#&gt; GSM11223     2  0.0000      0.955 0.000 1.000
#&gt; GSM11218     1  0.0000      0.992 1.000 0.000
#&gt; GSM11219     2  0.5059      0.884 0.112 0.888
#&gt; GSM11236     1  0.0000      0.992 1.000 0.000
#&gt; GSM28702     1  0.0000      0.992 1.000 0.000
#&gt; GSM28705     1  0.0000      0.992 1.000 0.000
#&gt; GSM11230     1  0.0000      0.992 1.000 0.000
#&gt; GSM28704     2  0.3114      0.928 0.056 0.944
#&gt; GSM28700     2  0.0000      0.955 0.000 1.000
#&gt; GSM11224     2  0.0000      0.955 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-1-a').click(function(){
  $('#tab-ATC-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-2'>
<p><a id='tab-ATC-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28710     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM28711     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM28712     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM11222     3  0.0000      0.952 0.000 0.000 1.000
#&gt; GSM28720     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM11217     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM28723     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM11241     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM28703     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM11227     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM28706     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM11229     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM11235     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM28707     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM11240     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM28714     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM11216     3  0.0000      0.952 0.000 0.000 1.000
#&gt; GSM28715     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM11234     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM28699     2  0.0237      0.963 0.004 0.996 0.000
#&gt; GSM11233     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM28718     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM11231     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM11237     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM11228     2  0.1163      0.946 0.000 0.972 0.028
#&gt; GSM28697     2  0.4399      0.765 0.000 0.812 0.188
#&gt; GSM28698     3  0.0000      0.952 0.000 0.000 1.000
#&gt; GSM11238     3  0.0000      0.952 0.000 0.000 1.000
#&gt; GSM11242     3  0.0000      0.952 0.000 0.000 1.000
#&gt; GSM28719     2  0.0747      0.955 0.000 0.984 0.016
#&gt; GSM28708     3  0.4605      0.742 0.000 0.204 0.796
#&gt; GSM28722     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM11232     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM28709     3  0.0000      0.952 0.000 0.000 1.000
#&gt; GSM11226     3  0.5216      0.649 0.000 0.260 0.740
#&gt; GSM11239     3  0.0000      0.952 0.000 0.000 1.000
#&gt; GSM11225     3  0.0000      0.952 0.000 0.000 1.000
#&gt; GSM11220     3  0.0000      0.952 0.000 0.000 1.000
#&gt; GSM28701     2  0.6095      0.349 0.000 0.608 0.392
#&gt; GSM28721     2  0.4750      0.726 0.000 0.784 0.216
#&gt; GSM28713     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM28716     1  0.1753      0.938 0.952 0.048 0.000
#&gt; GSM11221     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM28717     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM11223     1  0.0000      0.995 1.000 0.000 0.000
#&gt; GSM11218     3  0.1163      0.929 0.000 0.028 0.972
#&gt; GSM11219     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM11236     3  0.0000      0.952 0.000 0.000 1.000
#&gt; GSM28702     3  0.0000      0.952 0.000 0.000 1.000
#&gt; GSM28705     2  0.1031      0.949 0.000 0.976 0.024
#&gt; GSM11230     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM28704     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM28700     2  0.0000      0.966 0.000 1.000 0.000
#&gt; GSM11224     2  0.0000      0.966 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-2-a').click(function(){
  $('#tab-ATC-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-3'>
<p><a id='tab-ATC-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1    p2    p3    p4
#&gt; GSM28710     4  0.3486      0.754  0 0.188 0.000 0.812
#&gt; GSM28711     2  0.4250      0.694  0 0.724 0.000 0.276
#&gt; GSM28712     2  0.0336      0.863  0 0.992 0.000 0.008
#&gt; GSM11222     3  0.0592      0.953  0 0.000 0.984 0.016
#&gt; GSM28720     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11240     2  0.0469      0.863  0 0.988 0.000 0.012
#&gt; GSM28714     2  0.0469      0.861  0 0.988 0.000 0.012
#&gt; GSM11216     3  0.0336      0.956  0 0.000 0.992 0.008
#&gt; GSM28715     2  0.4643      0.578  0 0.656 0.000 0.344
#&gt; GSM11234     4  0.3266      0.767  0 0.168 0.000 0.832
#&gt; GSM28699     2  0.0336      0.856  0 0.992 0.000 0.008
#&gt; GSM11233     2  0.0469      0.853  0 0.988 0.000 0.012
#&gt; GSM28718     2  0.0336      0.863  0 0.992 0.000 0.008
#&gt; GSM11231     2  0.4406      0.662  0 0.700 0.000 0.300
#&gt; GSM11237     2  0.0469      0.863  0 0.988 0.000 0.012
#&gt; GSM11228     4  0.0707      0.902  0 0.020 0.000 0.980
#&gt; GSM28697     4  0.0469      0.899  0 0.000 0.012 0.988
#&gt; GSM28698     3  0.0336      0.957  0 0.000 0.992 0.008
#&gt; GSM11238     3  0.0188      0.957  0 0.000 0.996 0.004
#&gt; GSM11242     3  0.0336      0.957  0 0.000 0.992 0.008
#&gt; GSM28719     4  0.0707      0.902  0 0.020 0.000 0.980
#&gt; GSM28708     3  0.4103      0.678  0 0.000 0.744 0.256
#&gt; GSM28722     2  0.4164      0.707  0 0.736 0.000 0.264
#&gt; GSM11232     4  0.1389      0.891  0 0.048 0.000 0.952
#&gt; GSM28709     3  0.0188      0.957  0 0.000 0.996 0.004
#&gt; GSM11226     4  0.0817      0.894  0 0.000 0.024 0.976
#&gt; GSM11239     3  0.0188      0.956  0 0.000 0.996 0.004
#&gt; GSM11225     3  0.0188      0.956  0 0.000 0.996 0.004
#&gt; GSM11220     3  0.0336      0.956  0 0.000 0.992 0.008
#&gt; GSM28701     4  0.0469      0.899  0 0.000 0.012 0.988
#&gt; GSM28721     4  0.0657      0.900  0 0.004 0.012 0.984
#&gt; GSM28713     4  0.4992     -0.120  0 0.476 0.000 0.524
#&gt; GSM28716     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11221     2  0.4746      0.478  0 0.632 0.000 0.368
#&gt; GSM28717     2  0.0779      0.849  0 0.980 0.004 0.016
#&gt; GSM11223     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; GSM11218     4  0.0817      0.894  0 0.000 0.024 0.976
#&gt; GSM11219     2  0.1637      0.852  0 0.940 0.000 0.060
#&gt; GSM11236     3  0.0000      0.955  0 0.000 1.000 0.000
#&gt; GSM28702     3  0.3444      0.796  0 0.000 0.816 0.184
#&gt; GSM28705     4  0.0707      0.902  0 0.020 0.000 0.980
#&gt; GSM11230     2  0.0336      0.863  0 0.992 0.000 0.008
#&gt; GSM28704     4  0.1389      0.891  0 0.048 0.000 0.952
#&gt; GSM28700     2  0.1792      0.848  0 0.932 0.000 0.068
#&gt; GSM11224     2  0.3688      0.760  0 0.792 0.000 0.208
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-3-a').click(function(){
  $('#tab-ATC-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-4'>
<p><a id='tab-ATC-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28710     4  0.5309      0.454 0.000 0.264 0.000 0.644 0.092
#&gt; GSM28711     5  0.5491      0.239 0.000 0.312 0.000 0.088 0.600
#&gt; GSM28712     2  0.1502      0.728 0.000 0.940 0.000 0.004 0.056
#&gt; GSM11222     3  0.3835      0.653 0.000 0.000 0.732 0.008 0.260
#&gt; GSM28720     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11229     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.0162      0.739 0.000 0.996 0.000 0.004 0.000
#&gt; GSM28714     2  0.0510      0.741 0.000 0.984 0.000 0.000 0.016
#&gt; GSM11216     3  0.1270      0.891 0.000 0.000 0.948 0.000 0.052
#&gt; GSM28715     2  0.5117      0.568 0.000 0.652 0.000 0.276 0.072
#&gt; GSM11234     4  0.3745      0.500 0.000 0.196 0.000 0.780 0.024
#&gt; GSM28699     2  0.3177      0.637 0.000 0.792 0.000 0.000 0.208
#&gt; GSM11233     2  0.0880      0.730 0.000 0.968 0.000 0.000 0.032
#&gt; GSM28718     2  0.3662      0.648 0.000 0.744 0.000 0.004 0.252
#&gt; GSM11231     2  0.5348      0.555 0.000 0.656 0.000 0.112 0.232
#&gt; GSM11237     2  0.3430      0.678 0.000 0.776 0.000 0.004 0.220
#&gt; GSM11228     4  0.2796      0.625 0.000 0.008 0.008 0.868 0.116
#&gt; GSM28697     4  0.0771      0.592 0.000 0.004 0.000 0.976 0.020
#&gt; GSM28698     3  0.0162      0.907 0.000 0.000 0.996 0.000 0.004
#&gt; GSM11238     3  0.0290      0.907 0.000 0.000 0.992 0.000 0.008
#&gt; GSM11242     3  0.0290      0.907 0.000 0.000 0.992 0.000 0.008
#&gt; GSM28719     4  0.4863      0.361 0.000 0.016 0.008 0.592 0.384
#&gt; GSM28708     5  0.5819      0.205 0.000 0.048 0.336 0.032 0.584
#&gt; GSM28722     2  0.5760      0.247 0.000 0.536 0.000 0.096 0.368
#&gt; GSM11232     4  0.6002      0.373 0.000 0.140 0.000 0.552 0.308
#&gt; GSM28709     3  0.0880      0.904 0.000 0.000 0.968 0.000 0.032
#&gt; GSM11226     4  0.5385      0.238 0.000 0.028 0.016 0.528 0.428
#&gt; GSM11239     3  0.0162      0.907 0.000 0.000 0.996 0.000 0.004
#&gt; GSM11225     3  0.0162      0.907 0.000 0.000 0.996 0.000 0.004
#&gt; GSM11220     3  0.2712      0.842 0.000 0.000 0.880 0.032 0.088
#&gt; GSM28701     4  0.1697      0.553 0.000 0.000 0.008 0.932 0.060
#&gt; GSM28721     4  0.4402      0.460 0.000 0.012 0.000 0.636 0.352
#&gt; GSM28713     2  0.4639      0.478 0.000 0.612 0.000 0.368 0.020
#&gt; GSM28716     1  0.0451      0.986 0.988 0.008 0.000 0.000 0.004
#&gt; GSM11221     2  0.5681      0.534 0.000 0.604 0.000 0.276 0.120
#&gt; GSM28717     2  0.3661      0.577 0.000 0.724 0.000 0.000 0.276
#&gt; GSM11223     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11218     5  0.4883     -0.356 0.000 0.016 0.004 0.464 0.516
#&gt; GSM11219     2  0.1981      0.743 0.000 0.924 0.000 0.028 0.048
#&gt; GSM11236     3  0.0865      0.903 0.000 0.004 0.972 0.000 0.024
#&gt; GSM28702     3  0.4874      0.458 0.000 0.000 0.632 0.040 0.328
#&gt; GSM28705     4  0.3203      0.624 0.000 0.012 0.000 0.820 0.168
#&gt; GSM11230     2  0.3488      0.693 0.000 0.808 0.024 0.000 0.168
#&gt; GSM28704     4  0.3888      0.629 0.000 0.076 0.000 0.804 0.120
#&gt; GSM28700     2  0.2359      0.744 0.000 0.904 0.000 0.036 0.060
#&gt; GSM11224     2  0.4255      0.686 0.000 0.776 0.000 0.128 0.096
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-4-a').click(function(){
  $('#tab-ATC-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-5'>
<p><a id='tab-ATC-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28710     6  0.6155    -0.0413 0.000 0.224 0.000 0.336 0.008 0.432
#&gt; GSM28711     6  0.6221     0.1761 0.000 0.316 0.000 0.060 0.108 0.516
#&gt; GSM28712     2  0.1866     0.7048 0.000 0.908 0.000 0.008 0.084 0.000
#&gt; GSM11222     3  0.5585     0.1314 0.000 0.000 0.456 0.028 0.068 0.448
#&gt; GSM28720     1  0.0000     0.9954 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11217     1  0.0000     0.9954 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28723     1  0.0000     0.9954 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11241     1  0.0000     0.9954 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28703     1  0.0000     0.9954 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11227     1  0.0000     0.9954 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28706     1  0.0260     0.9906 0.992 0.000 0.000 0.000 0.008 0.000
#&gt; GSM11229     1  0.0000     0.9954 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11235     1  0.0000     0.9954 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28707     1  0.0000     0.9954 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11240     2  0.1010     0.7382 0.000 0.960 0.000 0.004 0.036 0.000
#&gt; GSM28714     2  0.1429     0.7451 0.000 0.940 0.000 0.004 0.052 0.004
#&gt; GSM11216     3  0.2633     0.8332 0.000 0.000 0.864 0.104 0.032 0.000
#&gt; GSM28715     2  0.4148     0.6443 0.000 0.748 0.000 0.184 0.012 0.056
#&gt; GSM11234     4  0.5683     0.3280 0.000 0.336 0.000 0.532 0.016 0.116
#&gt; GSM28699     5  0.3684     0.6692 0.000 0.332 0.000 0.004 0.664 0.000
#&gt; GSM11233     2  0.3714     0.0486 0.000 0.656 0.000 0.004 0.340 0.000
#&gt; GSM28718     2  0.2978     0.7324 0.000 0.856 0.000 0.008 0.084 0.052
#&gt; GSM11231     2  0.3920     0.7070 0.000 0.800 0.000 0.036 0.060 0.104
#&gt; GSM11237     2  0.3065     0.7305 0.000 0.848 0.000 0.008 0.096 0.048
#&gt; GSM11228     6  0.4712     0.0314 0.000 0.016 0.000 0.440 0.020 0.524
#&gt; GSM28697     4  0.3769     0.5856 0.000 0.040 0.000 0.780 0.012 0.168
#&gt; GSM28698     3  0.0551     0.8698 0.000 0.000 0.984 0.008 0.004 0.004
#&gt; GSM11238     3  0.0692     0.8679 0.000 0.000 0.976 0.000 0.004 0.020
#&gt; GSM11242     3  0.0508     0.8695 0.000 0.000 0.984 0.004 0.000 0.012
#&gt; GSM28719     6  0.3147     0.3721 0.000 0.008 0.000 0.160 0.016 0.816
#&gt; GSM28708     6  0.6825     0.2559 0.000 0.044 0.260 0.064 0.096 0.536
#&gt; GSM28722     2  0.4378     0.6050 0.000 0.724 0.000 0.020 0.048 0.208
#&gt; GSM11232     6  0.6514     0.0255 0.000 0.308 0.000 0.220 0.032 0.440
#&gt; GSM28709     3  0.1720     0.8581 0.000 0.000 0.928 0.032 0.040 0.000
#&gt; GSM11226     6  0.2842     0.4127 0.000 0.020 0.016 0.048 0.032 0.884
#&gt; GSM11239     3  0.0603     0.8688 0.000 0.000 0.980 0.000 0.004 0.016
#&gt; GSM11225     3  0.1167     0.8655 0.000 0.000 0.960 0.008 0.012 0.020
#&gt; GSM11220     3  0.4038     0.6982 0.000 0.000 0.712 0.244 0.044 0.000
#&gt; GSM28701     4  0.3533     0.5424 0.000 0.004 0.004 0.780 0.020 0.192
#&gt; GSM28721     6  0.4559     0.3516 0.000 0.044 0.000 0.192 0.040 0.724
#&gt; GSM28713     2  0.4286     0.5963 0.000 0.744 0.000 0.156 0.008 0.092
#&gt; GSM28716     1  0.1003     0.9584 0.964 0.020 0.000 0.016 0.000 0.000
#&gt; GSM11221     5  0.7244     0.2699 0.000 0.344 0.000 0.120 0.356 0.180
#&gt; GSM28717     5  0.3767     0.6798 0.000 0.276 0.004 0.012 0.708 0.000
#&gt; GSM11223     1  0.0146     0.9932 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; GSM11218     6  0.1007     0.4125 0.000 0.004 0.004 0.008 0.016 0.968
#&gt; GSM11219     2  0.1275     0.7530 0.000 0.956 0.000 0.016 0.016 0.012
#&gt; GSM11236     3  0.3210     0.8347 0.000 0.016 0.856 0.064 0.056 0.008
#&gt; GSM28702     6  0.4962     0.1849 0.000 0.000 0.292 0.020 0.056 0.632
#&gt; GSM28705     6  0.4872     0.0802 0.000 0.028 0.000 0.400 0.020 0.552
#&gt; GSM11230     2  0.4553     0.6257 0.000 0.776 0.056 0.016 0.072 0.080
#&gt; GSM28704     6  0.6122    -0.1845 0.000 0.308 0.000 0.336 0.000 0.356
#&gt; GSM28700     2  0.2528     0.7319 0.000 0.892 0.000 0.028 0.056 0.024
#&gt; GSM11224     2  0.2865     0.7346 0.000 0.868 0.000 0.056 0.012 0.064
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-5-a').click(function(){
  $('#tab-ATC-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-NMF-signature_compare](figure_cola/ATC-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-NMF-collect-classes](figure_cola/ATC-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n tissue(p) k
#> ATC:NMF 53     0.397 2
#> ATC:NMF 53     0.373 3
#> ATC:NMF 52     0.352 4
#> ATC:NMF 43     0.338 5
#> ATC:NMF 38     0.308 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

## Session info


```r
sessionInfo()
```

```
#> R version 3.6.0 (2019-04-26)
#> Platform: x86_64-pc-linux-gnu (64-bit)
#> Running under: CentOS Linux 7 (Core)
#> 
#> Matrix products: default
#> BLAS:   /usr/lib64/libblas.so.3.4.2
#> LAPACK: /usr/lib64/liblapack.so.3.4.2
#> 
#> locale:
#>  [1] LC_CTYPE=en_GB.UTF-8       LC_NUMERIC=C               LC_TIME=en_GB.UTF-8       
#>  [4] LC_COLLATE=en_GB.UTF-8     LC_MONETARY=en_GB.UTF-8    LC_MESSAGES=en_GB.UTF-8   
#>  [7] LC_PAPER=en_GB.UTF-8       LC_NAME=C                  LC_ADDRESS=C              
#> [10] LC_TELEPHONE=C             LC_MEASUREMENT=en_GB.UTF-8 LC_IDENTIFICATION=C       
#> 
#> attached base packages:
#> [1] grid      stats     graphics  grDevices utils     datasets  methods   base     
#> 
#> other attached packages:
#> [1] genefilter_1.66.0    ComplexHeatmap_2.3.1 markdown_1.1         knitr_1.26          
#> [5] GetoptLong_0.1.7     cola_1.3.2          
#> 
#> loaded via a namespace (and not attached):
#>  [1] circlize_0.4.8       shape_1.4.4          xfun_0.11            slam_0.1-46         
#>  [5] lattice_0.20-38      splines_3.6.0        colorspace_1.4-1     vctrs_0.2.0         
#>  [9] stats4_3.6.0         blob_1.2.0           XML_3.98-1.20        survival_2.44-1.1   
#> [13] rlang_0.4.2          pillar_1.4.2         DBI_1.0.0            BiocGenerics_0.30.0 
#> [17] bit64_0.9-7          RColorBrewer_1.1-2   matrixStats_0.55.0   stringr_1.4.0       
#> [21] GlobalOptions_0.1.1  evaluate_0.14        memoise_1.1.0        Biobase_2.44.0      
#> [25] IRanges_2.18.3       parallel_3.6.0       AnnotationDbi_1.46.1 highr_0.8           
#> [29] Rcpp_1.0.3           xtable_1.8-4         backports_1.1.5      S4Vectors_0.22.1    
#> [33] annotate_1.62.0      skmeans_0.2-11       bit_1.1-14           microbenchmark_1.4-7
#> [37] brew_1.0-6           impute_1.58.0        rjson_0.2.20         png_0.1-7           
#> [41] digest_0.6.23        stringi_1.4.3        polyclip_1.10-0      clue_0.3-57         
#> [45] tools_3.6.0          bitops_1.0-6         magrittr_1.5         eulerr_6.0.0        
#> [49] RCurl_1.95-4.12      RSQLite_2.1.4        tibble_2.1.3         cluster_2.1.0       
#> [53] crayon_1.3.4         pkgconfig_2.0.3      zeallot_0.1.0        Matrix_1.2-17       
#> [57] xml2_1.2.2           httr_1.4.1           R6_2.4.1             mclust_5.4.5        
#> [61] compiler_3.6.0
```


