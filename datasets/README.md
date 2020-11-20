# Datasets

**Data can be used in HASH to instantiate agents in a simulation, to set the properties of agents, or to influence any part of the simulation.**

HASH allows you to incorporate your own data into simulations, or download datasets from [hIndex](https://hash.ai/index) to use in a simulation.

Example Simulations that use datasets:

* [City Infection Model](https://core.hash.ai/@hash/city-infection-model/main)
* [Local Competition](https://hash.ai/@hash/local-competition)
* [Wholesale Warehouse](https://hash.ai/@hash/wholesale-warehouse1)

## Importing your own data in HASH

To import your own datasets into HASH, first navigate to your profile, click the  'New Project' button and select new Dataset. You can then set the display name and the project path, a  unique path that can be referenced from any simulation you create.

{% hint style="info" %}
**Coming soon:** data syncing from remote sources is currently only achievable through [hEngine](https://hash.ai/platform/engine), but remains on our roadmap for [hCore](https://hash.ai/platform/core).
{% endhint %}

## Integrating data into a simulation

To integrate with a dataset from within hCore, navigate to the "Add to Simulation" interface in the bottom-left hand corner of the UI to select datasets from either hIndex or hDrive to add. Once selected the dataset will be listed in your simulation's file list.

HASH parses imported datasets and generates a new field in `context.data()`with the file name.  This contains the content of datasets associated in the simulation. At this time HASH supports datasets imported in CSV or JSON formats.

* If the dataset is a JSON document, it gets parsed for you directly.
* If a dataset is a CSV file, we parse it into an array of JSON objects with values keyed under the headers in the first row.

To access a dataset:

```javascript
//For a dataset from hIndex (a dataset you've published or imported)
context.data()["@[user-handle or org-handle]/[short-name]/[dataset].[csv or json]")
```

{% hint style="info" %}
**Coming soon:** we will be streamlining this process shortly, providing more optionality around parsing treatment, and expanding support for the types of datasets ingestible by HASH.
{% endhint %}

If you wish to explore the universe of data available in HASH outside of hCore, you can do so directly [within hIndex](https://hash.ai/index/search?contentType=Dataset&sort=popularity). As with behaviors, we encourage you to tag data in hIndex with the type of '[Thing](https://hash.ai/index/schemas/Thing)' it represents. This ensures that the data can subsequently be easily discovered and reused.

**Example: Using a Dataset to initialize agents**

In the [city infection model](https://core.hash.ai/@hash/city-infection-model/main) you can see an example of using data to create agents with heterogenous values. 

The simulation contains a file, sf900homes100offices.csv, that appropriately contains listings of 900 homes and 100 offices. Each row contains a different building with a different lat, lng location

```javascript
  use_def	                 neighborhood	       lat	           long
0	Single Family Residential	Sunset/Parkside	-122.502183895904	37.763653457648
1	Single Family Residential	Bernal Heights	-122.4170591352	37.747528129366
```

An accompanying behavior, gis\_data\_upload.js, imports the data, performs transformations to it \(ex. cleaning the data, parsing it into floats\), and then pushes the data as objects into an array.

```javascript
let gis_data = context.data()["@b/property_data/sf900homes100offices.csv"]
...
let json_data = selected.map(row => ({
    "use_def": row[0],
    "neighborhood": row[1],
    "lat": parseFloat(row[2]),
    "lon": parseFloat(row[3]),
    "type": transform_type(row[0])
    })
  )
...
json_data.forEach(e => agents.push(e))

```

Then another behavior, create\_agents.js, iterates through the agents array and [initializes the agents](../tutorials/building-the-local-competition-model/phase-1-building-a-simple-hotelling-model-in-2d/initialization.md).

Now the simulation has a collection of agents with unique positions derived from real world data.

