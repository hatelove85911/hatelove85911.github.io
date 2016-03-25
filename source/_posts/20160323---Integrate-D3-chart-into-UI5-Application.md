---
title: Integrate D3 chart into UI5 Application
tags:
  - ui5
  - javascript
  - d3
date: 2016-03-23 18:17:42
---


# You don't need to create custom control for charts
In my last project, I encoutered a situation that I need to incorporate [d3](https://d3js.org/) chart into ui5 appliation. I googled around and found some posts about this topic:
1. [Custom SAPUI5 Visualization Controls with D3.js](http://scn.sap.com/community/developer-center/front-end/blog/2014/07/17/custom-sapui5-visualization-controls-with-d3js)
2. [How to Use D3 elements inside SAP UI5 , when using JS views?](http://stackoverflow.com/questions/25612362/how-to-use-d3-elements-inside-sap-ui5-when-using-js-views)
3. I even asked a question on SCN myself, [What's the best way to incorporate a d3 chart into UI5 application?](https://scn.sap.com/thread/3594973)

In above posts, the solution is to create a custom control for the chart, but IMO, creating a custom control in UI5 is a complicated and unpleasant task.
After some research, I found a much easier solution, you do __NOT__ need to create custom control at all! Let me show you how.

# How

For the example, I'll embed a simple pie chart into a UI5 application. Here's a snapshot of the final result
{% asset_img final_result.png "Final Result"%}

#### Implement Reusable Chart Module

Implement a reusable chart module. I highly recommend you to read the article [Towards Reusable Charts](https://bost.ocks.org/mike/chart/) in which the author of d3 library Mike Bostoc proposed a convention for building reusable chart. Below is the code of an example chart module compliant with the reusable convention, you don't need to go through the code, you only need to know it will generate a pie chart.
~~~javascript
sap.ui.define([], function () {
  return function () {
    // avabled configuration options
    var _innerRadius = 5
    var _outerRadius = 30
    var _width = 60
    var _height = 60

    // Chart
    var my = function (selection) {
      selection.each(function (d, i) {
        var arc = d3.svg.arc().outerRadius(_outerRadius).innerRadius(_innerRadius)
        var pie = d3.layout.pie().value(function (d) {
          return d.score
        })

        var g = d3.select(this).append('g')
          .attr('transform', 'translate(' + _width / 2 + ',' + _height / 2 + ')')

        g.selectAll('path')
          .data(pie(d))
          .enter()
          .append('path')
          .attr('d', arc)
          .attr('fill', function (d) {
            return d.data.color
          })
      })
    }

    // getter/setter for outerRadius
    my.outerRadius = function (outerRadius) {
      if (!arguments.length) return _outerRadius
      _outerRadius = outerRadius
      return my
    }

    // getter/setter for innerRadius
    my.innerRadius = function (innerRadius) {
      if (!arguments.length) return _innerRadius
      _innerRadius = innerRadius
      return my
    }

    // getter/setter for width
    my.width = function (width) {
      if (!arguments.length) return _width
      _width = width
      return my
    }

    // getter/setter for height
    my.height = function (height) {
      if (!arguments.length) return _height
      _height = height
      return my
    }
    return my
  }
})
~~~
A side note about "resubale", for a chart to be reusable, it meets the following requirements:
1. configurable
2. environment-agnostic

environment-agnostic means the chart should be implemented in the way that it should be told in which container and what data it should draw. In a simpler word, the container element and data should be passed in as parameters. 

#### Put HTML Placeholder in the Page

In the xml view, insert a `sap.ui.core.HTML` element as the root container for our chart.
~~~xml
<mvc:View controllerName="d3inui5.view.Main"
          xmlns:mvc="sap.ui.core.mvc"
          xmlns:core="sap.ui.core"
          displayBlock="true"
          xmlns="sap.m">
  <Page>
    <content>
      <core:HTML content="<svg id='pieChart' width='100%' height='200px'></svg>" afterRendering='draw'></core:HTML>
    </content>
  </Page>
</mvc:View>
~~~

#### Use the Chart in Controller

In the controller, use the chart like below:
~~~javascript
sap.ui.define([
  'd3inui5/view/BaseController',
  'd3inui5/chart/pieChart'
], function (Controller, pieChart) {
  'use strict'

  return Controller.extend('d3inui5.view.Main', {
    onInit: function () {
      // initialize a pie chart instance
      this.chart = pieChart()
      // configure the inner and outer radius for the chart
      this.chart.innerRadius(40)
      this.chart.outerRadius(80)
    },
    draw: function () {
      // sample data
      var data = [
        {
          'name': 'jun shen',
          'score': 30,
          'color': 'red'
        },
        {
          'name': 'aaron',
          'score': 60,
          'color': 'yellow'
        },
        {
          'name': 'atige',
          'score': 137,
          'color': 'purple'
        },
        {
          'name': 'linus',
          'score': 46,
          'color': 'black'
        },
        {
          'name': 'grass',
          'score': 8,
          'color': 'green'
        }
      ]

      // configure the width and height for the chart
      this.chart.width($('#pieChart').width())
      this.chart.height($('#pieChart').height())

      // draw the chart
      d3.select('#pieChart').data([data])
        .call(this.chart)
    }
  })
})
~~~
That's it, our chart has been integrated into our UI5 application. It's super simple, there's no overhead for creating a UI5 custom control. And out chart is highly reusable and customiziable, you can use it in other UI5 applications. you can even reuse it in non-UI5 projects. Isn't that great? 

#### The End
Happy coding !!!
Thanks for reading !!!
Leave comment if you have any question or comments.
