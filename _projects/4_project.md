---
layout: page
title: Home Credit Default Risk
project_title: Home Credit Default Risk
description: Kaggle Competetion by Home Credit Group. 
img: assets/img/homecredit.png
importance: 2
# category: Projects
giscus_comments: false
---

{::nomarkdown}
{% assign jupyter_path = "assets/jupyter/homecredit.ipynb" | relative_url %}
{% capture notebook_exists %}{% file_exists assets/jupyter/homecredit.ipynb %}{% endcapture %}
{% if notebook_exists == "true" %}
    {% jupyter_notebook jupyter_path %}
{% else %}
    <p>Sorry, the notebook you are looking for does not exist.</p>
{% endif %}
{:/nomarkdown}