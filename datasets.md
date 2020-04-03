# Datasets

HASH allows you to incorporate your own data into simulations, or download datasets from HASH Index. Data can be used in HASH to instantiate agents within a simulation, or to augment and enhance the properties of any number of agents.

### Importing your own data in HASH

To import your own datasets into HASH, first navigate to your [HASH Drive](https://hash.ai/drive) and use the 'Add New' button to initiate a data upload or sync.

To integrate with a dataset from within HASH Core, navigate to the "Add to Simulation" interface in the bottom-left hand corner of the UI to select datasets from either Index or Drive to add. Once integrated, the dataset is listed in your simulation's file list.

HASH parses imported datasets and generates a new field for you in your simulation properties, `data`. This contains the content of datasets associated by the UI. If the dataset is a JSON document, it gets parsed for you directly, otherwise it is provided as raw text, and you should parse it on your own.

**Example**

```text
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

