+++
title = "Install xgboost on MacOS python3 from source"
subtitle = "A quick installation guide"

date = 2019-01-27T00:00:00
lastmod = 2018-01-27T00:00:00
draft = true

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = ["Jialuo Liu"]

tags = ["xgboost","python"]
summary = "A quick installation guide"

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


- Install `gcc` notebook: obtain gcc-7 with Homebrew (https://brew.sh/) to enable multi-threading
- Clone the `xgboost` repository:
- invoke CMake to build XGBoost with make
- Install system-wide, which requires root permission, replace `python3` by `python` if you use python2.7

```{bash}

brew install gcc@7

```

```{bash}

git clone --recursive https://github.com/dmlc/xgboost

```

```{bash}

mkdir build
cd build
CC=gcc-7 CXX=g++-7 cmake ..
make -j4

```

```{bash}

cd python-package; sudo python3 setup.py install

```

## References

- https://xgboost.readthedocs.io/en/latest/build.html#python-package-installation
