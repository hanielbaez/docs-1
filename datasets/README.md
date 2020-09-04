# Datasets

**Data can be used in HASH to instantiate agents within a simulation, or to augment and enhance the properties of any number of agents.**

HASH allows you to incorporate your own data into simulations, or download datasets from [hIndex](https://hash.ai/index).

### Importing your own data in HASH

To import your own datasets into HASH, first navigate to your [hDrive](https://hash.ai/drive) and use the 'Add New' button to initiate a data upload.

{% hint style="info" %}
**Coming soon:** data syncing from remote sources is currently only achievable through [Engine](https://hash.ai/engine), but remains on our roadmap for hCore.
{% endhint %}

### Integrating data into a simulation

To integrate with a dataset from within hCore, navigate to the "Add to Simulation" interface in the bottom-left hand corner of the UI to select datasets from either hIndex or hDrive to add. Once integrated, the dataset is listed in your simulation's file list.

HASH parses imported datasets and generates a new field for you in `context.data()`. This contains the content of datasets associated in the simulation. At this time HASH supports datasets imported in CSV or JSON formats.

* If the dataset is a JSON document, it gets parsed for you directly.
* If a dataset is a CSV file, we parse it into an array of JSON objects with values keyed under the headers in the first row.

To access a dataset:

```javascript
//For a dataset in hDrive (a dataset you've uploaded)
context.data()["/filename.[csv||json]"]

//For a dataset from hIndex (a dataset you've published or imported")
context.data()["@[user-handle||org-handle]/[short-name]/[dataset].[csv||json]")
```

{% hint style="info" %}
**Coming soon:** we will be streamlining this process shortly, providing more optionality around parsing treatment, and expanding support for the types of datasets ingestible by HASH.
{% endhint %}

If you wish to explore the universe of data available in HASH outside of hCore, you can do so directly [within hIndex](https://hash.ai/index/search?contentType=Dataset&sort=popularity). As with behaviors, we attempt to match all data published to the hIndex to the type of '[Thing](https://hash.ai/index/schemas/Thing)' it represents in a global unified schema which subsequently makes finding data that accurately represents agents at the desired level of granularity easy.

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

