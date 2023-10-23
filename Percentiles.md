Averages can be unhelpful when it comes to monitoring. If you’re merely looking at averages, you’re potentially missing the outliers, which might matter a lot more. There are two issues with averages in the presence of outliers:

1. Averages hide the outliers, so you can’t see them.
2. Outliers skew averages, so in a system with outliers, the average doesn’t represent typical behavior.

So ***when you average the metrics from a system with erratic behavior, you get the worst of both worlds: you see neither the typical behavior, nor the unusual behavior***. Most systems have tons of outlying data points, by the way.

*Looking at the extremes in the "[long tail](https://en.wikipedia.org/wiki/Long_tail)" is important because it shows you how bad things can sometimes get, and you’ll miss this if you rely on averages.*

%%“While the average might be easy to understand it’s also extremely misleading. Why? Because looking at your average response time is like measuring the average temperature of a hospital. What you really care about is a patient’s temperature, and in particular, the patients who need the most help.”%%

Percentiles (more broadly, quantiles) are often praised as a potential way to bypass this fundamental issue with averages. The idea of the 99th percentile is to take a population of data (say, a collection of measurements from a system) and sort them, then discard the worst 1% and look at the largest remaining value. The resulting value has two important properties:

1. It’s the largest value that occurs 99% of the time. If it’s a webpage load time, for example, it represents the worst experience 99% of your visitors have.
2. It's robust in the face of truly extreme outliers, which come from all sorts of causes including measurement errors.

Regardless of their exact values and how wrong they are, percentile metrics tend to a) show outlying behavior and b) get bigger when outlying behavior gets badder. Super useful.

### **How Time Series Databases Store and Transform Metrics**

Time series databases are almost always storing _aggregate_ metrics over time ranges, not the _full population_ of events originally measured.

_Percentiles are computed from a population of data, and have to be recalculated every time the population (time interval) changes. Time series databases with traditional metrics don’t have the original population._

### **Alternative Ways To Compute Percentiles**

- Histograms, which partition the population into ranges or bins, and then count how many fall into various ranges.
- Approximate streaming data structures and algorithms (sketches).
- Databases sampling from populations to give fast approximate answers.
- Solutions bounded in time, space, or both.

![[Pasted image 20230813141510.png]]
_Source: Catchpoint.com, data from Oct. 15, 2013 to Nov. 25, 2013 for 30KB

There are tons of ways to compute and store approximate distributions, but histograms are popular because of their relative simplicity. Some monitoring solutions actually support histograms. [[Circonus]] is one, for example.

## **Percentiles Done Better in Time Series Databases**

A better way to compute percentiles with a time series database is to collect banded metrics. I mention the assumption because lots of time series databases are just ordered, timestamped collections of named values, without the capability of storing histograms.

*Banded metrics provide a way to get the same effect as a series of histograms over time. What you’d do is select limits that divide the space of values up into ranges or bands, and then compute and store metrics about each band over time. The metric will be just as it is in histograms: the count of observations that fall into the range.*

[How to choose the ranges](https://www.brendangregg.com/FrequencyTrails/modes.html )

## **Beyond Percentiles: Heatmaps**

[[Circonus]] provides an excellent example of [heatmap](https://www.circonus.com/understanding-data-with-histograms/) visualizations

![[Pasted image 20230813142423.png]]


## References:

1. https://orangematter.solarwinds.com/2016/11/18/why-percentiles-dont-work-the-way-you-think/ 