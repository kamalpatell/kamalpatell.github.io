---
layout: page
title: Building Power Consumption
project_title: Building Energy Analysis
description: Visualization and analysis of simulated building energy data
img: assets/img/project3/E1.png
importance: 3
# category: Projects
giscus_comments: false
---

## Summary

- Conducted thorough data analysis, cleaning, and pre-processing using pandas library, resulting in improved data quality and reduced errors.
- leveraged Matplotlib and Seaborn's functionalities to create visually compelling and informative charts that enhanced the overall understanding of the underlying data.
- Utilized K-means Clustering on electrical meter data to identify daily load profiles and implemented a k-nearest neighbor regression model to accurately predict energy consumption with a MAPE of 6.59%.
- Implemented Random Forest model on ASHRAE thermal dataset, enabling the analysis of thermal sensation and identify key factors influencing human comfort with feature importance plot.

{::nomarkdown}
{% assign jupyter_path = "assets/jupyter/building.ipynb" | relative_url %}
{% capture notebook_exists %}{% file_exists assets/jupyter/building.ipynb %}{% endcapture %}
{% if notebook_exists == "true" %}
    {% jupyter_notebook jupyter_path %}
{% else %}
    <p>Sorry, the notebook you are looking for does not exist.</p>
{% endif %}
{:/nomarkdown}