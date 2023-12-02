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

One of the biggest shopping mall of the world located in United Arab Emirates (i.e. arid climate) with Floor area is about 500,000 m2
and number of visitors every year is around 90 million. Building with tremendously high cooling energy demand.

We will begin with visualizing some EnergyPlus simulation model data.

**Monthly Energy Visulization**

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/project3/monthly.png" title="monthly" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Energy Consumption in Total GWh 
</div>

**Different Design Options Scenerio**

First let's take a look at one of the design options - the use of optimized schedules. Instead of the building operating 24/7, the building would have the cooling system on only during operating hours.

<div class="row">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/project3/E2.png" title="Aircon" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/project3/E3.png" title="Difference between baseline and improved schedule" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Baseline vs Improved Schedule
</div>

**Comparing all of the options**

- Glazing
- Rooftop gardens
- Thermal Comfort
- Cool paiting on the roof

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/project3/E4.png" title="Comparing Options" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Comparing Options
</div>

**Meter Data Analysis**

For this section, we will be using the Building Data [Genome Project](https://github.com/buds-lab/the-building-data-genome-project)

<!-- ![Genome](https://raw.githubusercontent.com/buds-lab/the-building-data-genome-project/master/figures/buildingdatagenome1.png) -->

This project is a set of 500+ time-series meter data from buildings -- check out the website which includes an overview of the data set, sources, and publications that use the data set.

**Plot Simple Charts of time-series data**

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/project3/E6.png" title="office_amelia" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Meter data for office Amelia
</div>

**Resample the data and plot the data**

Two types of resampling are:

- `Upsampling`: Where you increase the frequency of the samples, such as from minutes to seconds.
- `Downsampling`: Where you decrease the frequency of the samples, such as from days to months.

<div class="row">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/project3/E7.png" title="Daily" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/project3/E8.png" title="Monthly" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Energy Consumption by amelia office daily(left) and monthly(right). 
</div>

**Analysis on multiple buildings**

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/project3/E11.png" title="Unnormalized" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/project3/E12.png" title="normalized" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Unnormalized vs normalized data plot.
</div>

- From the left plot we see from the annual totals of energy consumption, the Lab named Ciel uses a lot of energy as compared to the other buildings but, when normalized using floor area, `Ciel` is not the highest consumer.

**Analyze the weather influence on energy consumption**

<div class="row justify-content-sm-center">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/project3/E13.png" title="weather influence" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
   Weather influence on energy consumption.
</div>

**Indoor Environment Quality Analysis**

For this section we will use the ASHRAE Thermal Comfort Database II.

<div class="row justify-content-sm-center">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/project3/E14.png" title="By Country" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Air temperature (C) and Relative humidity (%) by Country
</div>

**Conditions comfortable for people ?**

what conditions influence the comfort of occupants in the various contexts that were tested in the ASHRAE Thermal Comfort Database II studies. lets investigate how temperature, humidity, air movement, and other factors influence comfort.

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/project3/E15.png" title="Air-Temperature" class="img-fluid rounded z-depth-1" %}
    </div>
     <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/project3/E16.png" title="Relative Humidity" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Thermal Sensation with respect to Air temperature (C) and Relative humidity respectively.
</div>

**How does building type or its systems influence people's comfort?**

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/project3/E17.png" title="Building type" class="img-fluid rounded z-depth-1" %}
    </div>
     <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/project3/E18.png" title="Cooling strategy" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Influence of Building type and cooling strategy.
</div>


### **Jupyter Notebook**

{::nomarkdown}
{% assign jupyter_path = "assets/jupyter/building.ipynb" | relative_url %}
{% capture notebook_exists %}{% file_exists assets/jupyter/building.ipynb %}{% endcapture %}
{% if notebook_exists == "true" %}
    {% jupyter_notebook jupyter_path %}
{% else %}
    <p>Sorry, the notebook you are looking for does not exist.</p>
{% endif %}
{:/nomarkdown}