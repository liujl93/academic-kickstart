+++
# Project title.
title = "Krylov Subspace Method for Large Spatial Datasets"

# Date this page was created.
date = 2016-04-27T00:00:00

# Project summary to display on homepage.
summary = "Efficient Algorithm for Large Spatial Datasets."

# Tags: can be used for filtering projects.
# Example: `tags = ["machine-learning", "deep-learning"]`
tags = ["Big Data"]

# Optional external URL for project (replaces project detail page).
url_pdf = ""
#url_slides = "files/krylov_slides.pdf"
#url_poster = "files/SPL_poster_v4.pdf"
url_code = "https://github.com/liujl93/spKrylov"

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references
#   `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
# slides = "example-slides"

# Links (optional).
# url_pdf = ""
# url_slides = ""
# url_video = ""
# url_code = ""

# Custom links (optional).
#   Uncomment line below to enable. For multiple links, use the form `[{...}, {...}, {...}]`.
#url_custom = [{icon_pack = "fab", icon="twitter", name="Follow", url = "https://twitter.com/georgecushen"}]

# Featured image
# To use, add an image named `featured.jpg/png` to your project's folder.
[image]
  # Caption (optional)
  caption = ""

  # Focal point (optional)
  # Options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
  focal_point = "Left"
+++

Maximum likelihood method for irregularly spaced spatial datasets is computationally intensive, as it involves the manipulation of sizable dense covariance matrices. Finding the exact likelihood is clearly impractical, especially for large datasets. We present an approximation to the Gaussian log-likelihood function using Krylov subspace methods. This method reduces the computational complexity from $O(n^3)$ operations to $O(n^2)$ for dense matrices and further to linear if matrices are sparse. Specifically, this algorithm comprises two parts: 1) an implementation of conjugate gradient method; and 2) a stochastic estimator of the log-determinant based on Monte Carlo method and Lanczos quadrature rule. We give conditions to ensure consistency of the estimators. Simulation studies have been conducted to explore the computational efficiency as well as small sample performance. We also apply our proposed method to estimate the spatial structure of the yearly anomalies data of total precipitation for the continental US.
