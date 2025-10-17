---
layout: post
title:  "Highcharts Histograms are Weird"
date:   2025-10-15 9:00:00 -0500
categories: jekyll update
---

You can create histograms in highcharts. These are different from column charts because they do the binning for you and the bars touch each other if they are next to each other.

The json to submit to the highcharts server for a histogram looks like this.

```json
{
  "chart": {
    "type": "histogram"
  },
  "series": [
    {
      "type": "histogram",
      "baseSeries": "s1",
      "binsNumber": 5,
      "zIndex": -1
    },
    {
      "id": "s1",
      "name": "Data",
      "type": "scatter",
      "data": [20, 30, 80, 90],
      "visible": false 
    }
  ]
}
```

This causes highcharts to automatically bin the data in the second series, `s1`, and show it as a histogram. You can only create histograms from an existing series so if you don't want the behavior you should turn the visibility of the scatter series to false. The other important thing to know is that highcharts won't add bins between your data even if they are far apart so the bar containing the `30` and the bar containing the `80` will touch each other unless you set the `binsNumber`
