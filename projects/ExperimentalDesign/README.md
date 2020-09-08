[< Back to the portfolio](https://s-bishnoi.github.io/shubham-bishnoi/)

# Experimental Design

**Work in Progress**

The project is on experimenting with different features of the online streaming service to reduce the average browsing time. They generates revenue when users stream something online by minimal browsing. Due to so many options, users often get overwhelmed and end up watching nothing. Therefore, the purpose of this experiment is to find the combination of optimum values of preview length, preview size and tile size to minimize the average browsing time, thereby resulting in quicker user selection.

Three factors will be explored in this project; Tile Size, Preview Size, and Preview length. Tile size is the ratio of a tile’s height to the overall screen height, preview size is the ratio of the preview window’s height to the overall screen height and preview length is the duration (in seconds) of a show or movie’s preview.

The optimum value turns out to be

| Average Browsing Time | Preview.Length | Preview.Size | Tile.Size |
| :---: | :---: | :---: | :---: |
| 15 minutes and 45 seconds | 90 | 0.65 | 0.2 |

### Phase 1

The objective of this phase of the experiment is to figure out if any of the factors Tile.Size, Prev.Size, and Prev.Length are significantly important for *Average Browsing Time* response. The 2^K = 2^3 factorial experiment was used instead of a $2^{3-1}$ because the design generator AB = C leads to defining relation of I = ABC with resolution III. In resolution III, every main effect would have been confounded with a two-factor interaction. It is difficult to differentiate the significance achieved through main effects or the interactions in the linear model in lower resolution.

```json
## Load helpful packages and functions
library(plot3D) # needed for the mesh() function
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
# Data
netflix.ph1 <- read.csv(file = "./RESULTS_20636835_2020-08-08_factor_screen.csv", header = T)

ph0 <- data.frame(y = netflix.ph1$Browse.Time,
                  x1 = convert.N.to.C(U = netflix.ph1$Prev.Length, UH = 90, UL = 30),
                  x2 = convert.N.to.C(U = netflix.ph1$Prev.Size, UH = 0.5, UL = 0.3),
                  x3 = convert.N.to.C(U = netflix.ph1$Tile.Size, UH = 0.3, UL = 0.1))
```