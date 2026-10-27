# SoilHealth_Yield

Code and data for the systematic analysis of how soil properties affect crop yield
changes under conservation agriculture.

## Reference

> Ren, X., Zou, W., Jiao, J., Stewart, R., & Jian, J. (2023). Soil properties affect crop
> yield changes under conservation agriculture: A systematic analysis.
> *European Journal of Soil Science*, 74(5), e13413. https://doi.org/10.1111/ejss.13413

## Contents

| Path | Description |
| :--- | :--- |
| `SoilHealth_Yield_revision.Rmd` | Main analysis script (R Markdown). Generates the results and figures reported in the paper. |
| `SoilHealth_Yield.Rproj` | RStudio project file. |
| `data/SoilHealthDB_V2.csv` | Main dataset: soil health and crop yield observations extracted from the literature. |
| `data/Koeppen-Geiger-ASCII.txt` | Koppen-Geiger climate classification grid, used to assign a climate class to each site. |
| `data/SoilHealthDB_V2.xlsx` | Spreadsheet version of the database. |
| `data/SoilHealthDB_V2(1).xlsx` | Alternate copy of the spreadsheet version. |

## What the analysis does

The script is organised as a sequence of R Markdown chunks:

1. **Load and curate the database** (`load data`). Reads `data/SoilHealthDB_V2.csv`, merges
   each study site with the Koppen-Geiger climate grid on rounded latitude/longitude, and
   derives the grouping variables used throughout: `Top_Climate`, `TextureGroup`,
   `ConservationGroup`, `CoverCropGroup` and `GrainCropGroup`.
2. **Bootstrap the overall and subgroup yield effects** (`bootstrap`). The `boosting_yield`
   function resamples the effect sizes 10,000 times (`set.seed(1234567)`) and returns
   confidence intervals for the percentage change in yield. It is applied to the full
   dataset and then to each subgroup: climate, soil texture, conservation practice
   (no-tillage, cover crop, agroforestry, other, mixed), cover crop type and cash crop type.
3. **Relate yield change to soil properties** (`regression`). For every soil property the
   log ratio between the conservation and control treatment is used as a moderator in
   `lm(y ~ log_x)` / `lm(y ~ x)`, yielding slope, p-value and R² per property.
4. **Figures and comparisons.** Site maps, distribution checks, the effect-size figure
   (Figure 2), radar plots comparing climates and soil-texture groups, and targeted
   comparisons such as corn yield change under conservation agriculture and yield under
   agroforestry.

Effect sizes are log response ratios (`yi`) of crop yield under conservation agriculture
relative to the conventional control.

## Reproducing

Requirements: R and RStudio, plus the packages loaded in the `preliminaries` and
library chunks of `SoilHealth_Yield_revision.Rmd`: `data.table`, `dplyr`, `ggplot2`,
`lubridate`, `kableExtra`, `knitr`, `ggpubr`, `ggmap`, `maps`, `mapdata`, `tidyr`, `car`,
`agricolae`, `multcompView`, `bootstrap`, `metafor`, `e1071`, `stringr`, `rgdal`,
`raster`, `patchwork`, `tidyverse` and `RColorBrewer`.

```r
setwd("SoilHealth_Yield")   # the folder containing the .Rmd and data/
rmarkdown::render("SoilHealth_Yield_revision.Rmd")
```

The script reads its inputs from `data/` relative to the working directory and writes
rendered output to `outputs/`, so open the project from this folder (or `setwd()` into it)
before rendering.

## Citation

If you use this code or the accompanying data, please cite the paper above.