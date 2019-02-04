+++
title = "Movie Recommendation Engine using Spark"
subtitle = "A Spark project"

date = 2019-01-27T00:00:00
lastmod = 2018-01-27T00:00:00
draft = false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = ["Jialuo Liu"]

tags = ["MovieLens", "Spark","SQL"]
summary = "A Spark project"

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

In this post, I will use Apache Spark and MLlib (Spark machine learning library) to build a movie recommender system.

Prerequisite: [Install PySpark on MacOS](https://liujl93.github.io/post/pyspark/)

The results are based on Python 3.7 with Spark 2.4.

## Datasets: from [Movielens](https://grouplens.org/datasets/movielens/latest/)

[comment]: <> ( MovieLens uses "collaborative filtering" technology to make recommendations of movies.)
- Full data set (24 million ratings, 40,000 movies, 250,000 users).
- A smaller data set (100836 ratings, 3683 tag application across 9742 movies, 610 users, 1996/03-2018/09)
- Each user rated at least 20 movies, unique user-id.

## MLlib : Machine Learning Library in Spark

- Consists of common ML algorithms, including classification, regression, clustering, and collaborative filtering.
- two packages in this library:
    * ***spark.mllib***: handles data in RDDs
    * ***spark.ml***: handles data in DataFrames

## Recommendation System Strategies
![Strategies](/img/post/movierec/movierec4.png)

This graph is summarized from the paper "[Matrix factorization techniques for recommender systems](https://datajobs.com/data-science-repo/Recommender-Systems-[Netflix].pdf)." Refer to it for more insights on collaborative filtering and two illuminating cartoons.

In this post, we will be using the alternating least squares algorithm, from `pyspark.ml.recommendation`. To use it, do

```{python}

from pyspark.ml.recommendation import ALS

```


## Some useful sparksql commands
To query and explore the data, we will use some spark SQL commands. Here are a brief list of some useful sparksql commands.

![Sparksql](/img/post/movierec/movierec2.jpeg)


## Run the notebook
Armed with the knowledge of collaborative filtering, let's get some hands-on [experience](https://github.com/liujl93/Movie-Recommendation-Engine-using-Spark/blob/master/MovieLens-Recommendation-System-PySpark-ALS.ipynb).

## References

- Koren, Yehuda, Robert Bell, and Chris Volinsky. "Matrix factorization techniques for recommender systems." Computer 8 (2009): 30-37.
- F. Maxwell Harper and Joseph A. Konstan. 2015. The MovieLens Datasets: History and Context. ACM Transactions on Interactive Intelligent Systems.
- [Movie recommender system with Spark machine learning](Movie recommender system with Spark machine learning) by IBM.
