---
layout: post
title: The 8a Dataset, Part 2
---

* TOC
{:toc}

# Where do Climbers Live?

In my [previous post](../The-8a-Dataset-Part-1), I introduced the dataset I collected from [8a.nu](https://www.8a.nu/) and looked at how height, weight, and BMI correlated with climbing achievement. I suggest you read it for more background on the data.

In this post we'll ask a different question: Where do the best climbers live? Cities such as Boulder, Innsbruck, or Barcelona come to mind - I wanted to explore what the data on 8a would show. A picture is worth 1000 words, so I figured decided an interactive data visualization counts for at least that. Here's what I did:
1. [Split up the Climbers](#split-up-the-climbers): Similarly to the previous analysis of body composition and grades, I split the climbers into three populations based on what grades they had climbed.
2. [Heatmaps](#heatmaps): The 8a data lists climbers' hometowns, so I used the [Google Maps Geocoding API](https://developers.google.com/maps/documentation/geocoding/start) to look up latitude and longitude for each city. Then, I created a Google Map with a [heatmap layer](https://developers.google.com/maps/documentation/javascript/heatmaplayer), where the intensity was proportional to the number of climbers in that population at that location.

# Split up the Climbers

This is similar to the split I used for the height and weight analysis, dividing into three groups based on how grade. Note that because location (and height and weight) are optional, these groups are *not* the same as before.
- All the climbers on 8a that listed a valid city (whether it could be geocoded) and climbed fewer than 10 ascents 8a or harder. This group had 8626 climbers. I call this "All Climbers".
- The set of climbers that had at least 10 ascents 8a or harder. This group had 615 climbers. I call this "8a Climbers".
- The set of climbers that had at least 10 ascents 8b or harder. This group had 109 climbers. I call this "8b Climbers"
- Note that these populations are disjoint, i.e. climbers from "All Climbers" are not in "8a Climbers".

# Heatmaps

Heatmaps are a technique for visualizing spacial data. The intensity of the coloring is based on the number of data points at a location, i.e. places where there are more climbers appear more red. They're a quick way to summarize and understand a lot of data, and pretty fun to create and peruse! You can zoom and scroll around the maps to explore the location distribution at different granularities.

<style>
  .map {
    height: 90vh;
  }
</style>

## All Climbers

<div id="all_map" class="map"></div>

## 8a Climbers

<div id="8a_map" class="map"></div>

## 8b Climbers

<div id="8b_map" class="map"></div>

<script src="../images/2017-5-21-The-8a-Dataset-Part-2/maps.js"></script>

<script async defer
src="https://maps.googleapis.com/maps/api/js?key=AIzaSyBvoTAgNb8OKEj6v9NIfBNTQkBl5LywU7o&libraries=visualization&callback=initMap">
</script>

<script async defer
src="https://maps.googleapis.com/maps/api/js?key=AIzaSyBvoTAgNb8OKEj6v9NIfBNTQkBl5LywU7o&libraries=visualization&callback=initMap8a">
</script>

<script async defer
src="https://maps.googleapis.com/maps/api/js?key=AIzaSyBvoTAgNb8OKEj6v9NIfBNTQkBl5LywU7o&libraries=visualization&callback=initMap8b">
</script>

# Appendix: Technical Details

## Geocoding API

## Heatmap API
