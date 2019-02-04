+++
title = "Calling C/C++ from R and multithreading with OpenMP"
subtitle = "Integrating R and C/C++"

date = 2019-01-27T00:00:00
lastmod = 2018-01-27T00:00:00
draft = false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = ["Jialuo Liu"]

tags = ["Computing", "R"]
summary = "Integrating R and C/C++"

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["deep-learning"]` references
#   `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
# projects = ["internal-project"]

# Featured image
# To use, add an image named `featured.jpg/png` to your project's folder.
# [image]
  # Caption (optional)
  # caption = "Image credit: [**Unsplash**](https://unsplash.com/photos/CpkOjOcXdUY)"

  # Focal point (optional)
  # Options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, # BottomRight
  # focal_point = ""

  # Show image only in page previews?
  # preview_only = false

# Set captions for image gallery.

[[gallery_item]]
album = "gallery"
image = "theme-default.png"
caption = "Default"

[[gallery_item]]
album = "gallery"
image = "theme-ocean.png"
caption = "Ocean"

[[gallery_item]]
album = "gallery"
image = "theme-forest.png"
caption = "Forest"

[[gallery_item]]
album = "gallery"
image = "theme-dark.png"
caption = "Dark"

[[gallery_item]]
album = "gallery"
image = "theme-apogee.png"
caption = "Apogee"

[[gallery_item]]
album = "gallery"
image = "theme-1950s.png"
caption = "1950s"

[[gallery_item]]
album = "gallery"
image = "theme-coffee-playfair.png"
caption = "Coffee theme with Playfair font"

[[gallery_item]]
album = "gallery"
image = "theme-cupcake.png"
caption = "Cupcake"
+++

This post is related to a short course [High Performance Computing for Spatial Data](http://blue.for.msu.edu/envr18/) taught by [Dr. Andrew Finley](http://blue.for.msu.edu/) and Dr. Dorit Hammerling at [2018 ENVR Workshop](https://community.amstat.org/envr/events/envr2018workshop). An useful slide can be found [here](http://blue.for.msu.edu/envr18/slides/R-api.pdf).

There are three functions for integrating R and C/C++:

- .Call()
- External()
- .C()

## .Call()
In this short course, a detailed explanation of the function .Call() is given. In brief, you will need

- A C/C++ function (filename.cpp) and an R wrapper function (filename.R)
- Compile the C/C++ code (in R):

  ```{r}
            system("R CMD SHLIB filename.cpp")
  ```

- Load the compiled object (filename.so in MacOS or Linux or filename.dll in Windows) in R:

```{r}
            dyn.load("filename.so")
```

## .C()
A good implementation of the function .C() can be found in the R package [bdmiso](https://www.stat.colostate.edu/~wanghn/Rpackages/bdmiso_0.9.tar.gz), which is developed by Dr. Ela Sienkiewicz from CSU.

## Rcpp()
An alternative is the [Rcpp](http://www.rcpp.org/) package in R. The Rcpp package offers a seamless integration of R and C++. For detailed implementation, you can refer to my [STplm](https://github.com/liujl93/STplm) R package.
