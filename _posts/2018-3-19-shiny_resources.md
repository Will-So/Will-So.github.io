---
layout: post
title: Shiny by Example
draft: false
tags: [R, Dashboarding, Resources]
---
I've found the best way to learn Shiny is by example. When I started working with Shiny, I could look at previous projects on my team. Since this is no longer an option, I've found some instructive examples to learn from below.

## Example 1: Solid Shiny Dashboard
![](https://s3.us-east-2.amazonaws.com/screens12/OL178.png)
[This](https://gallery.shinyapps.io/086-bus-dashboard/) example ([source](https://github.com/rstudio/shiny-examples/tree/master/086-bus-dashboard)) uses Shiny Dashboard and efficiently makes an elegant dashboard.

It's also a good demonstration of Leaflet, which I always recommend for anything mapping related. 

## Example 2: Shiny S&P 500 Dashboard
![](https://s3.us-east-2.amazonaws.com/screens12/OL179.png)
This dashboard was made by [Zhe Yang](https://www.linkedin.com/in/zhe-yang-993634ba/). Clone [this](https://github.com/Zheeee/Shiny_SP500_Dashboard) repository and try to run it. Installing all the dependencies can take awhile but it's worth it. There are a number of things to like about this dashboard:

- It's a good example of a standard Shiny Dashboard App.
- The Time Series Tab is especially insightful. 
- The organization of all the different features is great. The main functionality is all in the sidebar and more specific feature sets are tabs within the main pane.
- Once you install all the dependencies, it just works. 


# Important Concepts
In addition to the examples above, here are some resources I've found useful.

- **Shiny Components:**[the official documentation](https://shiny.rstudio.com/gallery/widget-gallery.html) on shiny components is very useful. 

- **Shiny Dashboard:**`shinydashboard` makes it easy to structure your Shiny App. [This Page](https://rstudio.github.io/shinydashboard/structure.html) has everything that you need to know to get started. 

- **Reactive Programming:** The hardest thing about shiny programming is properly understanding reactive programming. [Read](https://shiny.rstudio.com/articles/reactivity-overview.html) this to understand how reactivity in Shiny works. This is also useful for learning about `react.js` or `vue.js` in the future.


# Additional Resources
- [Shiny Material](https://ericrayanderson.github.io/shinymaterial/) for making 
- For calling Python code from R, you can [use Falcon](http://falcon.readthedocs.io/en/stable/user/quickstart.html) to build a REST API in Python. Other popular choices 
- [DT](https://rstudio.github.io/DT/) for interactive tables and pretty tables.
- [Leaflet For Mapping](https://rstudio.github.io/leaflet/shiny.html)
- [Official Showcase of Examples](https://www.rstudio.com/products/shiny/shiny-user-showcase/) has the bux example and many more. 
- [Dashboard Themes](https://nik01010.wordpress.com/) would be fun to experiment with. Note: I haven't tried this one personally. 
- I've used [plotly](https://plotly-book.cpsievert.me/) in the past for any type of visualization that requies more interactivity than ggplot.  
    + [This](https://plot.ly/r/box-plots/) is a good example of Plotly in action. 

