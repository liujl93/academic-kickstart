+++
title = "Krylov Subspace Method for Large Spatial Datasets"
# date = 2017-01-01T00:00:00  # Schedule page publish date.
draft = false

# Talk start and end times.
#   End time can optionally be hidden by prefixing the line with `#`.
#time_start = 2030-06-01T13:00:00
#time_end = 2030-06-01T15:00:00

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = ["Jialuo Liu","Tingjin Chu","Haonan Wang","Jun Zhu"]


# Abstract and optional shortened version.
abstract = "Maximum likelihood method for irregularly spaced spatial datasets is computationally intensive, as it involves the manipulation of sizable dense covariance matrices. Finding the exact likelihood is clearly impractical, especially for large datasets. We present an approximation to the Gaussian log-likelihood function using Krylov subspace methods. This method reduces the computational complexity from $O(n^3)$ operations to $O(n^2)$ for dense matrices and further to linear if matrices are sparse. Specifically, this algorithm comprises two parts: 1) an implementation of conjugate gradient method; and 2) a stochastic estimator of the log-determinant based on Monte Carlo method and Lanczos quadrature rule. We give conditions to ensure consistency of the estimators. Simulation studies have been conducted to explore the computational efficiency as well as small sample performance. We also apply our proposed method to estimate the spatial structure of the yearly anomalies data of total precipitation for the continental US. "

# Name of event and optional event URL.
# event = "Academic Theme Conference"
# event_url = "https://example.org"

# Location of event.
location = "Colorado State University"

# Is this a featured talk? (true/false)
featured = false

# Projects (optional).
#   Associate this talk with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["deep-learning"]` references
#   `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
# projects = ["internal-project"]

# Tags (optional).
#   Set `tags = []` for no tags, or use the form `tags = ["A Tag", "Another Tag"]` for one or more tags.
tags = []

# Slides (optional).
#   Associate this talk with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references
#   `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
# slides = "example-slides"

# Links (optional).
url_pdf = ""
url_slides = ""
url_video = ""
url_code = ""

# Does the content use math formatting?
math = true

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
[image]
  # Caption (optional)
  # caption = "Image credit: [**Unsplash**](https://unsplash.com/photos/bzdhc5b3Bxs)"

  # Focal point (optional)
  # Options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
  focal_point = "Smart"
  preview_only = true
+++
