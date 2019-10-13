``` r
library(RColorBrewer)
library(data.table)

# color parameter options:  c("greys","blues","reds","greens","oranges","purples")
getSeqColors <- function(color, N) {
  colorRefDat <- data.table(colorSelect = 
                              c("greys","blues","reds","greens","oranges","purples"), minCol = c("grey0","lightskyblue","indianred1","darkseagreen1","goldenrod1","lavender"), maxCol = 
                              c("grey100","blue4","darkred","darkgreen","darkorange3","purple4"))
  
  shades <- colorRampPalette(c(colorRefDat[colorSelect == color]$minCol, 
                               colorRefDat[colorSelect == color]$maxCol))
  returnColors <- shades(N)
  return(returnColors)
}

getQualColors <- function(n) {
qual_col_pals = brewer.pal.info[brewer.pal.info$category == 'qual',]
col_vector = unlist(mapply(brewer.pal, qual_col_pals$maxcolors, 
rownames(qual_col_pals)))
return(col_vector)
}
```

The Problem
-----------

I have a use case of showing many colors on the same graph. Many
existing palettes are constrained to 8 or 12 colors. For example, R
Color Brewer (RColorBrewer::display.brewer.all()) has palettes with only
10-12 colors.

I would like an easy function that provides me with a unique palette of
colors that are either sequential (c(“light blue”,“blue”,“dark blue”))
or categorical or qualitative (c(“red”,“white”,“blue”)).

For more information on types of color palettes see
<a href="http://colorbrewer2.org/learnmore/schemes_full.html" class="uri">http://colorbrewer2.org/learnmore/schemes_full.html</a>

The Solution
------------

Now let’s to do a test for the sequential color palette.

``` r
## Sequential 
colorVec <- getSeqColors("reds",20)
getSeqColors("reds",20)
```

    ##  [1] "#FF6A6A" "#F86464" "#F25E5E" "#EC5959" "#E65353" "#E04E4E" "#DA4848"
    ##  [8] "#D44242" "#CE3D3D" "#C83737" "#C13232" "#BB2C2C" "#B52727" "#AF2121"
    ## [15] "#A91B1B" "#A31616" "#9D1010" "#970B0B" "#910505" "#8B0000"

``` r
mat <- matrix(rep(1:20, each = 20))
image(mat, axes = FALSE, col = colorVec)
```

![](GetColors_files/figure-markdown_github/Sequential-1.png)

Now let’s to do a test for the categorical or qualitative color palette.

``` r
colorVecQual <- getQualColors(n = 20)
colorVecQual
```

    ##  [1] "#7FC97F" "#BEAED4" "#FDC086" "#FFFF99" "#386CB0" "#F0027F" "#BF5B17"
    ##  [8] "#666666" "#1B9E77" "#D95F02" "#7570B3" "#E7298A" "#66A61E" "#E6AB02"
    ## [15] "#A6761D" "#666666" "#A6CEE3" "#1F78B4" "#B2DF8A" "#33A02C" "#FB9A99"
    ## [22] "#E31A1C" "#FDBF6F" "#FF7F00" "#CAB2D6" "#6A3D9A" "#FFFF99" "#B15928"
    ## [29] "#FBB4AE" "#B3CDE3" "#CCEBC5" "#DECBE4" "#FED9A6" "#FFFFCC" "#E5D8BD"
    ## [36] "#FDDAEC" "#F2F2F2" "#B3E2CD" "#FDCDAC" "#CBD5E8" "#F4CAE4" "#E6F5C9"
    ## [43] "#FFF2AE" "#F1E2CC" "#CCCCCC" "#E41A1C" "#377EB8" "#4DAF4A" "#984EA3"
    ## [50] "#FF7F00" "#FFFF33" "#A65628" "#F781BF" "#999999" "#66C2A5" "#FC8D62"
    ## [57] "#8DA0CB" "#E78AC3" "#A6D854" "#FFD92F" "#E5C494" "#B3B3B3" "#8DD3C7"
    ## [64] "#FFFFB3" "#BEBADA" "#FB8072" "#80B1D3" "#FDB462" "#B3DE69" "#FCCDE5"
    ## [71] "#D9D9D9" "#BC80BD" "#CCEBC5" "#FFED6F"

``` r
mat <- matrix(rep(1:20, each = 20))
image(mat, axes = FALSE, col = colorVecQual)
```

![](GetColors_files/figure-markdown_github/Categorical-1.png)
