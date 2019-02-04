+++
title = "Movie Recommendation Engine using Spark and Elasticsearch"
subtitle = ""

date = 2019-01-27T00:00:00
lastmod = 2018-01-27T00:00:00
draft = false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = ["Jialuo Liu"]

tags = ["MovieLens", "Spark","python", "MLlib","collaborative filtering"]
summary = ""

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

The results are based on Python 3.7 with Spark 2.4.

## MLlib : Machine Learning Library in Spark

- Consists of common ML algorithms, including classification, regression, clustering, and collaborative filtering.
- two packages in this library:
    * ***spark.mllib***: handles data in RDDs
    * ***spark.ml***: handles data in DataFrames

## Datasets: from Movielens

- MovieLens uses "collaborative filtering" technology to make recommendations of movies.
- Full data set (24 million ratings, 40,000 movies, 250,000 users).
- A smaller data set (100836 ratings, 3683 tag application across 9742 movies, 610 users, 1996/03-2018/09)
- Each user rated at least 20 movies, unique user-id.

## Road-map
![This is an image](/img/movierec1.png)

### Step 1: Data Preparation


Load necessary libraries and check if PySpark is running.
```{python}
from IPython.display import Image, HTML, display
spark
```
Load data in the folder ` ml-latest-small` and look at first several rows.

#### `ratings.csv`
```{python}
ratings = spark.read.csv("ratings.csv", header=True, inferSchema=True)
ratings.cache()
ratings.count() # 100836
ratings.show(5)
```

```
+------+-------+------+---------+
|userId|movieId|rating|timestamp|
+------+-------+------+---------+
|     1|      1|   4.0|964982703|
|     1|      3|   4.0|964981247|
|     1|      6|   4.0|964982224|
|     1|     47|   5.0|964983815|
|     1|     50|   5.0|964982931|
+------+-------+------+---------+
```

```{python}
ratings = ratings.select(ratings.userId, ratings.movieId, ratings.rating, (ratings.timestamp.cast("long") * 1000).alias("timestamp"))

ratings.show(5)
```

```
+------+-------+------+------------+
|userId|movieId|rating|   timestamp|
+------+-------+------+------------+
|     1|      1|   4.0|964982703000|
|     1|      3|   4.0|964981247000|
|     1|      6|   4.0|964982224000|
|     1|     47|   5.0|964983815000|
|     1|     50|   5.0|964982931000|
+------+-------+------+------------+
```

#### `movies.csv`

```{python}
raw_movies = spark.read.csv("movies.csv", header=True, inferSchema = True)
raw
```

```
+-------+----------------------------------+-------------------------------------------+
|movieId|title                             |genres                                     |
+-------+----------------------------------+-------------------------------------------+
|1      |Toy Story (1995)                  |Adventure|Animation|Children|Comedy|Fantasy|
|2      |Jumanji (1995)                    |Adventure|Children|Fantasy                 |
|3      |Grumpier Old Men (1995)           |Comedy|Romance                             |
|4      |Waiting to Exhale (1995)          |Comedy|Drama|Romance                       |
|5      |Father of the Bride Part II (1995)|Comedy                                     |
+-------+----------------------------------+-------------------------------------------+
```


- A useful `SQL` functions in PySpark: UDF (user-defined function)
- Convert a delimited string to a list

```{python}
from pyspark.sql.functions import udf
from pyspark.sql.types import *

extract_genres = udf(lambda x: x.lower().split("|"), ArrayType(StringType()))
```

```{python}
raw_movies.select("movieId","title",extract_genres("genres").alias("genres")).show(5,False)
```

```
+-------+----------------------------------+-------------------------------------------------+
|movieId|title                             |genres                                           |
+-------+----------------------------------+-------------------------------------------------+
|1      |Toy Story (1995)                  |[adventure, animation, children, comedy, fantasy]|
|2      |Jumanji (1995)                    |[adventure, children, fantasy]                   |
|3      |Grumpier Old Men (1995)           |[comedy, romance]                                |
|4      |Waiting to Exhale (1995)          |[comedy, drama, romance]                         |
|5      |Father of the Bride Part II (1995)|[comedy]                                         |
+-------+----------------------------------+-------------------------------------------------+
```

- Extract the release year from the title

```{python}
import re

def extract_year_fn(title):
  result = re.search("\(\d{4}\)", title)
  try:
  if result:
    group = result.group()
    year = group[1:-1]
    start_pos = result.start()
    title = title[:start_pos-1]
    return (title, year)
  else:
    return (title, 1970)
  except:
    print(title)

extract_year = udf(extract_year_fn,\
                   StructType([StructField("title", StringType(), True),\
                               StructField("release_date", StringType(), True)]))
```

```{python}
s = "Jumanji (1995)"
extract_year_fn(s)
```

```
('Jumanji', '1995')
```

`movie`: cleaned versions

```
movies = raw_movies.select("movieId",extract_year("title").title.alias("title"),\
                            extract_year("title").release_date.alias("release_date"),\
                            extract_genres("genres").alias("genres"))

movies.show(5,truncated=False)
```

```
+-------+---------------------------+------------+-------------------------------------------------+
|movieId|title                      |release_date|genres                                           |
+-------+---------------------------+------------+-------------------------------------------------+
|1      |Toy Story                  |1995        |[adventure, animation, children, comedy, fantasy]|
|2      |Jumanji                    |1995        |[adventure, children, fantasy]                   |
|3      |Grumpier Old Men           |1995        |[comedy, romance]                                |
|4      |Waiting to Exhale          |1995        |[comedy, drama, romance]                         |
|5      |Father of the Bride Part II|1995        |[comedy]                                         |
+-------+---------------------------+------------+-------------------------------------------------+
```
## References

- F. Maxwell Harper and Joseph A. Konstan. 2015. The MovieLens Datasets: History and Context. ACM Transactions on Interactive Intelligent Systems.
