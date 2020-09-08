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