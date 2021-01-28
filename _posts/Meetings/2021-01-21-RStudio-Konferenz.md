---
layout: post
title: "Meeting 21.1.2021"
excerpt: Wir haben uns gemeinsam Vorträge von der RStudio Konferenz 2021 angeschaut
category: Meetings
---

Da das vom Termin her mit unserem regelmäßigen Treffen zusammen fiel haben wir uns einige Talks der [RStudio Conference 2020](https://blog.rstudio.com/2021/01/20/rstudio-global-tomorrow/) zusammen angeschaut.

Die Talks von den Jahren [2020](https://rstudio.com/resources/rstudioconf-2020/), [2019](https://rstudio.com/resources/rstudioconf-2019), [2018](https://rstudio.com/resources/rstudioconf-2018), [2017](https://rstudio.com/resources/rstudioconf-2017) und [2016](https://rstudio.com/resources/shiny-dev-con-2016) kann man sich im Video-Archiv anschauen. Die Talks von diesem Jahr sollen auch irgendwann online gestellt werden.


Im folgenden ein paar Notizen zu den Vorträgen:

------------------
* Talk: Visualisation of the pandemic in the Financial Times:
by: John Burn-Murdoch 

    > John will discuss the lessons he's learned reporting on and visualising the pandemic, including the world of difference between making charts for a technical audience and making charts for a mass audience. 
    > You'll learn from his experience navigating the highly personal and political context within which people consume and evaluate graphics and data, and how that can help us better design and communicate with visualisations down the pipeline for the future.

    * How to communicate the right data to the broader mass.


    * Eyetracking Experiment:

      - Title -> Description of bars

      - participants remembered titles

      - Titles stick!

    * Coronavirus Trajectory tracker:

      - Put a summary of information in the graphic in the title(What is the takeaway)

      - Descriptive text inside the plot on graphs and axis (annotation)


    * Lesson:

      - assumption: use math and geometry to make perfect plot

      - better: "Charts for non-chart people"

      - Make charts intuitively interpretable 

        - "which country does better or worse"

        - not "how many pixels is 100 cases"

      - Communication is key

        - Stick around for feedback about your work


    * Smoothing of the Graph:

      - Splining vs rolling average

          - Rolling avg better to understand

          - spline more true to real data and also smoother

          - choose splining because it communicates better

    * Use of animation:

      - animations can surprise because not all information is visible at once. It builds up tension and climaxes in the keypoint which you want to communicate.



    * Summary:

        - Text is secret weapon

        - explain the chart to confused people in situ

        - consider emotional and political context

        - Stick around 

        - ease of understanding has to come first


    * Tools:

      - first version: R

      - post production in illustrator (because of fin-times styleguide)

        * pivoted to "d3"

        * using R and ggplot to draft then go to d3


--------------

* Buchempfehlung: Hadley Wickham: The joy of functional programming

* workflowr

* Vortrag Allison Horst
  * https://github.com/allisonhorst/stats-illustrations
  * https://eco-data-science.github.io/
  * https://twitter.com/ecodatasci

* Talk: [Aesthetically automated figure production](https://global.rstudio.com/student/page/40627)
  * gridextra
  * ggtext -> markdown in strings
  * match.fun
  * gtable


* Twitter: @accidental__aRt

* formr -> surveys

* John Burn-Murdochs -> [Reporting on and visualising the pandemic : rstudio::global(2021)](https://global.rstudio.com/student/page/40615)
  - google trends: search for "log scale"
  - "chart and non-chart persons"

* tidymodels


* [A New Paradigm for Multifigure, Coordinate-Based Plotting in R : rstudio::global(2021)](https://global.rstudio.com/student/activity/40633)
  - https://github.com/nekramer
  - https://github.com/PhanstielLab/BentoBox/


* [Using Guided Simulation Exercises to Teach Data Science with R : rstudio::global(2021)](https://global.rstudio.com/student/activity/40602)

* [xaringan](https://slides.yihui.org/xaringan/#1) -> html presentations

* [tmap](https://cran.r-project.org/web/packages/tmap/vignettes/tmap-getstarted.html)

* annotate()


* penguin plot
* ggplot2book.org
* geom_polygon
* coord_map()

* Multi plot
  - patchwork
  - cowplot

* data sonification -> Daten für Blinde

* purrr::walk2

