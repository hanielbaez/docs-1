# Geospatial

The geospatial viewer provides a realtime view of a simulation running inside any geographic area — a neighborhood, a city, a country, or the whole world. It's great for visualizing simulations in which agents occupy a position on a map. Take a look at the [City Infection Model](https://hash.ai/@hash/city-infection-model-with-vaccine) for an example simulation using the geospatial viewer.

![](../.gitbook/assets/geospatial-viewer.png)

## Points

HASH will draw any agent with a `lng_lat` field as a point in the geospatial viewer. This field should be an array with two elements — the longitude and latitude of the agent. Additional attributes, such as `geo_color` and `geo_radius`, may be used to style it.

```text
{
  "lng_lat": [-115.811, 37.242],
  "geo_color": "#ff0000",
  "geo_radius": 15,
  "geo_opacity": 0.8,
  // Remaining agent fields ...
}
```

The full list of fields are:

* `lng_lat`: the longitude and latitude of the agent.
* `geo_color`: \(optional\) the color of the circle.
* `geo_radius`: \(optional\) the radius, in pixels, of the circle.
* `geo_opacity`: \(optional\) the opacity of the circle. A number in the range 0.0-1.0.

You can change these fields at any time during the simulation, and the agent will move, change color, or otherwise update as the simulation runs.

## Areas and other shapes

Currently, HASH only supports drawing points in the geospatial viewer. We plan to add further capabilities soon, including support for drawing polygons.

