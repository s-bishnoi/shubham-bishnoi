[< Back to the portfolio](https://s-bishnoi.github.io/shubham-bishnoi/)

# Experimental Design

**Work in Progress**

The project is on experimenting with different features of the online streaming service to reduce the average browsing time. They generates revenue when users stream something online by minimal browsing. Due to so many options, users often get overwhelmed and end up watching nothing. Therefore, the purpose of this experiment is to find the combination of optimum values of preview length, preview size and tile size to minimize the average browsing time, thereby resulting in quicker user selection.

Three factors will be explored in this project; Tile Size, Preview Size, and Preview length. Tile size is the ratio of a tile’s height to the overall screen height, preview size is the ratio of the preview window’s height to the overall screen height and preview length is the duration (in seconds) of a show or movie’s preview.

The optimum value turns out to be

| Average Browsing Time | Preview.Length | Preview.Size | Tile.Size |
| :---: | :---: | :---: | :---: |
| 15 minutes and 45 seconds | 90 | 0.65 | 0.2 |


#### Load helpful packages and functions

```
library(plot3D)
library(gplots)

# Function to create blues
blue_palette <- colorRampPalette(c(rgb(247,251,255,maxColorValue = 255), rgb(8,48,107,maxColorValue = 255)))

# Function for converting from natural units to coded units
convert.N.to.C <- function(U,UH,UL){
  x <- (U - (UH+UL)/2) / ((UH-UL)/2)
  return(x)
}

# Function for converting from coded units to natural units
convert.C.to.N <- function(x,UH,UL){
  U <- x*((UH-UL)/2) + (UH+UL)/2
  return(U)
}
```

### Phase 1

The objective of this phase of the experiment is to figure out if any of the factors Tile.Size, Prev.Size, and Prev.Length are significantly important for *Average Browsing Time* response. The 2^K = 2^3 factorial experiment was used because 2^(3-1) will lead to resolution III. In resolution III, every main effect would have been confounded with a two-factor interaction. It is difficult to differentiate the significance achieved through main effects or the interactions in the linear model in lower resolution.

```
netflix.ph1 <- read.csv(file = "./factor_screening.csv", header = T)

ph0 <- data.frame(y = netflix.ph1$Browse.Time,
                  x1 = convert.N.to.C(U = netflix.ph1$Prev.Length, UH = 90, UL = 30),
                  x2 = convert.N.to.C(U = netflix.ph1$Prev.Size, UH = 0.5, UL = 0.3),
                  x3 = convert.N.to.C(U = netflix.ph1$Tile.Size, UH = 0.3, UL = 0.1))

par(mfrow=c(2,3)) 
plotmeans(formula = y~x3, ylab = "Average Browsing Time", xlab = "Tile.Size", 
          data = ph0, xaxt = "n", pch = 16,ylim = c(16,21))
axis(side = 1, at = c(1,2), labels = c("0.1", "0.3"))
plotmeans(formula = y~x2, ylab = "Average Browsing Time", xlab = "Prev.Size", 
          data = ph0, xaxt = "n", pch = 16,ylim = c(16,21))
axis(side = 1, at = c(1,2), labels = c("0.3", "0.5"))
plotmeans(formula = y~x1, ylab = "Average Browsing Time", xlab = "Prev.Length", 
          data = ph0, xaxt = "n", pch = 16,ylim = c(16,21))
axis(side = 1, at = c(1,2), labels = c("30", "90"))
```

[<img src="./p11.png" width="900"/>](./p11.png)

The plots of preview size and preview length shows that the average browsing time decreases as we go from their respective lows to highs. On the other hand, in the plot for title size, we can see that going from 0.1 to 0.3 doesn't much effect the average browsing time. This leads us to believe that the title size might be insignificant. We will test the null hypotesis if title size is relevant or not.

```
## Fit a full model with all main effects and interaction terms
model <- lm(y ~ x1 * x2 * x3, data = ph0)

## Fit a reduced model with just the main effects and interactions that appear to be significant
model_red <- lm(y ~ x1 + x2 + x1:x2, data = ph0)

anova(model_red,model)
```

[<img src="./p12.png" width="500"/>](./p12.png)

We fail to reject the null hypothesis that both the models fits the data equally well. Therefore, we will be dropping the tile size for the rest of the experiment.

### Phase 2