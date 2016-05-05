# ggcyto : Visualize `Cytometry` data with `ggplot`




```r
library(ggcyto)
dataDir <- system.file("extdata",package="flowWorkspaceData")
```

## Feature 1: 3-tier plot constructors `ggplot`, `ggcyto`, `autoplot`
* represent different levels of complexity and flexibility
* meet the needs of various plot applications
* suitable for users at different levels of coding skills.

### tier 1: `ggplot`

The overloaded `fority` methods empower `ggplot`  to work with all the major Cytometry data structures right away, which allows users to do all kinds of highly customized and versitled plots.

#### `GatingSet`

```r
gs <- load_gs(list.files(dataDir, pattern = "gs_manual",full = TRUE))
attr(gs, "subset") <- "CD3+"
ggplot(gs, aes(x = `<B710-A>`, y = `<R780-A>`)) + geom_hex(bins = 128) + scale_fill_gradientn(colours = gray.colors(9))
```

![](Top_features_of_ggcyto_files/figure-html/unnamed-chunk-3-1.png)

#### `flowSet/ncdfFlowSet/flowFrame` 

```r
fs <- getData(gs, "CD3+")
ggplot(fs, aes(x = `<B710-A>`)) + geom_density(fill = "blue", alpha= 0.5)
```

![](Top_features_of_ggcyto_files/figure-html/unnamed-chunk-4-1.png)

#### `gates`

```r
gates <- filterList(getGate(gs, "CD8"))
ggplot(gs, aes(x = `<B710-A>`, y = `<R780-A>`)) + geom_hex(bins = 128) + geom_polygon(data = gates, fill = "transparent", col = "purple")
```

![](Top_features_of_ggcyto_files/figure-html/unnamed-chunk-5-1.png)

### tier 2: `ggcyto`

`ggcyto` constructor along with overloaded `+` operator encapsulate lots of details that might be tedious and intimidating for many users.


```r
ggcyto(gs, aes(x = CD4, y = CD8)) + geom_hex(bins = 128) + geom_gate("CD8")
```

![](Top_features_of_ggcyto_files/figure-html/unnamed-chunk-6-1.png)

It simplies the plotting by:
* add a default scale_fill_gradientn for you
* fuzzy-matching in `aes` by either detector or fluorochromes names
* determine the `parent` popoulation automatically
* exact and plot the gate object by simply referring to the `child` population name

### tier 3: `autoplot`
Inheriting the spirit from ggplot's `Quick plot`, it further simply the plotting job by hiding more details from users and taking more assumptions for the plot.

* when plotting `flowSet`, it determines `geom` type automatically by the number of `dim` supplied 
* for `GatingSet`, it further skip the need of `dim` by guessing it from the `children` gate


```r
#1d
autoplot(fs, "CD4")
```

![](Top_features_of_ggcyto_files/figure-html/unnamed-chunk-7-1.png)

```r
#2d
autoplot(fs, "CD4", "CD8", bins = 64)
```

![](Top_features_of_ggcyto_files/figure-html/unnamed-chunk-7-2.png)

```r
autoplot(gs, c("CD4", "CD8"), bins = 64)
```

![](Top_features_of_ggcyto_files/figure-html/unnamed-chunk-7-3.png)

## Feature 2: in-line transformation by different `scales` layers dedicated fom `cytometry`

## Feature 3: generic `geom_gate` layer hides the details for plotting different geometric shapes

## Feature 4: `axis_inverse_trans` can display the `log` scaled data in the original value

## Feature 5: `ggcyto_par_set` separates and aggregates the different settings that are unqiue in `Cytometry` plotting

## Feature 6: `as.ggplot` coerce method allows user to convert `ggcyto` objects to pure `ggplot` objects for further the manipulating jobs that can not be done within `ggcyto` framework.

## Feature 7: Layout many gate plots on the same page

When plooting a `GatingHierarchy`, multiple cell populations with their asssociated gates can be plotted in different panels of the same plot.

```r
gh <- gs[[1]]
nodes <- getNodes(gh, path = "auto")[c(3:9, 14)]
nodes
```

```
## [1] "singlets"    "CD3+"        "CD4"         "CD4/38- DR+" "CD4/38+ DR+"
## [6] "CD4/38+ DR-" "CD4/38- DR-" "CD8"
```

```r
autoplot(gh, nodes, bins = 64)
```

![](Top_features_of_ggcyto_files/figure-html/unnamed-chunk-8-1.png)

## Feature 8: Optionally set limits by `instrument` or `data` range

More examples:

* [ggplot + flowSet1d](vignettes/advanced/ggplot.flowSet.1d.md)
* [ggplot + flowSet2d](vignettes/advanced/ggplot.flowSet.2d.md)
* [ggplot + flowSet + gate](vignettes/advanced/ggplot.flowSet.gate.md)
* [ggplot + flowSet + overlay](vignettes/advanced/ggplot.flowSet.overlay.md)