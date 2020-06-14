# Datasets

**Data can be used in HASH to instantiate agents within a simulation, or to augment and enhance the properties of any number of agents.**

HASH allows you to incorporate your own data into simulations, or download datasets from [HASH Index](https://hash.ai/index).

### Importing your own data in HASH

To import your own datasets into HASH, first navigate to your [HASH Drive](https://hash.ai/drive) and use the 'Add New' button to initiate a data upload.

{% hint style="info" %}
**Coming soon:** data syncing from remote sources is currently only achievable through [Engine](https://hash.ai/engine), but remains on our roadmap for Core.
{% endhint %}

### Integrating data into a simulation

To integrate with a dataset from within HASH Core, navigate to the "Add to Simulation" interface in the bottom-left hand corner of the UI to select datasets from either Index or Drive to add. Once integrated, the dataset is listed in your simulation's file list.

HASH parses imported datasets and generates a new field for you in your simulation properties, `data`. This contains the content of datasets associated by the UI. At this time HASH supports datasets imported in CSV or JSON formats.

* If the dataset is a JSON document, it gets parsed for you directly.
* If a dataset is a CSV file, we parse it into an array of JSON objects with values keyed under the headers in the first row.

{% hint style="info" %}
**Coming soon:** we will be streamlining this process shortly, providing more optionality around parsing treatment, and expanding support for the types of datasets ingestible by HASH.
{% endhint %}

If you wish to explore the universe of data available in HASH outside of Core, you can do so [directly within the Index](https://hash.ai/index/search?contentType=Dataset&sort=popularity). As with behaviors, we attempt to match all data published to the HASH Index to the type of '[Thing](https://hash.ai/index/schemas/Thing)' it represents in a global unified schema which subsequently makes finding data that accurately represents agents at the desired level of granularity easy.

**Current example**

```javascript
[{
"data": {
      "New York Census QuickFacts": [
        {
          "Population estimates base, April 1, 2010,  (V2018)": "19,378,124"
        },
        {
          "Population, percent change - April 1, 2010 (estimates base) to July 1, 2018,  (V2018)": "0.80%"
        },
        ...
      ]
    }
}]
```

