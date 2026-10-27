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
| `Yield_Loop/Yield_Loop.Rmd` | Main analysis script (R Markdown). Bootstrap confidence intervals for yield change, plus soil-property meta-regressions. |
| `Yield_Loop/subdata.csv` | Study-level dataset used by the analysis. |
| `Yield_Loop/Yield_Loop.Rproj` | RStudio project file. |
| `Yield_Loop.rar` | Original compressed archive of the `Yield_Loop` folder, preserved as submitted. |

`Yield_Loop.rar` and the `Yield_Loop/` folder contain identical files (verified by SHA-256);
the folder is provided separately so the code and data can be browsed and diffed directly
on GitHub.

## What the analysis does

1. **Effect size.** `yi` is the log response ratio of crop yield under conservation
   agriculture relative to the conventional control. Effect sizes are aggregated to
   `StudyID` / `ExperimentID` level before modelling.
2. **Yield change by subgroup.** A bootstrap routine (`boosting_yield`, 10,000
   resamples, `set.seed(1234567)`) converts `yi` into the percentage change in yield and
   returns 95% confidence intervals from the bootstrap quantiles. Subgroups: climate,
   soil texture, conservation practice group, cover crop type, and cash crop type.
3. **Soil-property effects.** For each soil property the log ratio between the
   conservation and control treatment (e.g. `x_OC = log(OC_T) - log(OC_C)`) is used as a
   moderator in a linear meta-regression `lm(yi ~ x_property)`, reporting `n`, number of
   studies, intercept, slope, slope p-value, and R². Properties covered include bulk
   density, organic carbon, N, P, K, pH, CEC, base saturation, aggregation, porosity,
   penetration resistance, infiltration, saturated hydraulic conductivity, erosion,
   runoff, leaching, soil temperature, soil water content, plant-available water
   capacity, weeds, diseases, pests, soil fauna, fungi, other microbes, enzymes, C and N
   mineralisation, N2O, CO2, CH4, microbial biomass C and N.

## Reproducing

Requirements: R (tested with the versions recorded in the session info of the Rmd),
RStudio, and the packages listed in the setup chunk of `Yield_Loop.Yield_Loop.Rmd`:
`data.table`, `dplyr`, `ggplot2`, `lubridate`, `kableExtra`, `knitr`, `ggpubr`,
`ggmap`, `maps`, `mapdata`, `tidyr`, `car`, `agricolae`, `multcompView`, `bootstrap`,
`metafor`, `e1071`, `stringr`, `rgdal`, `raster`, `patchwork`, `ggradar`, `fmsb`.

```r
setwd("Yield_Loop")
rmarkdown::render("Yield_Loop.Rmd")
```

`subdata.csv` is read from the working directory, so open the project from the
`Yield_Loop` folder (or `setwd()` into it) before rendering.

## Citation

If you use this code or the accompanying data, please cite the paper above.