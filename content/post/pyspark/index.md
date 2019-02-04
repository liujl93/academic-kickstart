+++
title = "Install PySpark on MacOS"
subtitle = "A quick installation guide"

date = 2019-01-27T00:00:00
lastmod = 2018-01-27T00:00:00
draft = false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = ["Jialuo Liu"]

tags = ["Spark","python"]
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

## Prerequisite

- Install `Jupyter` notebook

```{bash}

pip install jupyter

```

- Install `PySpark`
  * Prerequisite: Java 8 or higher
  * From Apache Spark [downloads page](https://spark.apache.org/downloads.html), choose an appropriate repository.
  * unzip the downloaded file and move it to the directory: /opt/ (you might need sudo mv)
  * create a symbolic link: ln -s (you might need sudo ln)
  * in your bash(~/.bashrc or ~/.zshrc), configure the environment variables SPARK_HOME, PATH and PYSPARK_DRIVER_PYTHON.
  * source bash(~/.bashrc or ~/.zshrc) or restart terminal.
  * run `pyspark` from your intended directory.

```{bash}

$ wget http://mirrors.ocf.berkeley.edu/apache/spark/spark-2.4.0/spark-2.4.0-bin-hadoop2.7.tgz
$ tar xzf spark-2.4.0-bin-hadoop2.7.tgz
$ mv spark-2.4.0-bin-hadoop2.7 /opt/spark-2.4.0
$ ln -s /opt/spark-2.4.0 /opt/spark̀

```

```{bash}

$ vim ~/.zshrc

```

Press i and copy the following code to your bash file:

```{bash}

export SPARK_HOME=/opt/spark
export PATH=$SPARK_HOME/bin:$PATH
export PYSPARK_DRIVER_PYTHON=jupyter
export PYSPARK_DRIVER_PYTHON_OPTS='notebook'
#For python 3, You have to add the line below or you will get an error
# export PYSPARK_PYTHON=python3

```

Then, run

```{bash}

source ~/.zshrc
pystark

```

## Test your installation

* In your jupyter notebook, show SparkContext.

```
sc
```

You will see something like

```{python}
SparkContext

Spark UI

Version
v2.4.0
Master
local[*]
AppName
PySparkShell
```

Try the following example, from Michael Galarnyk's [post](https://medium.com/@GalarnykMichael/install-spark-on-mac-pyspark-453f395f240b):

```{python}
sc = SparkContext.getOrCreate()
import numpy as np
TOTAL = 1000000
dots = sc.parallelize([2.0 * np.random.random(2) - 1.0 for i in range(TOTAL)]).cache()
print("Number of random points:", dots.count())
stats = dots.stats()
print('Mean:', stats.mean())
print('stdev:', stats.stdev())
```


Additional [examples](http://spark.apache.org/examples.html) are provided on the Spark official site.
