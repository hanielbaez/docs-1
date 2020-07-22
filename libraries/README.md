# Libraries

## HASH Standard Library

HASH provides a set of functions through `hash_stdlib.` It contains common functions to simplify simulation construction. 

`hash_stdlib` __is currently only available in JavaScript behaviors; however, we're expanding it to include equivalent functions for Python and Rust. [The Python language environment also provides pyodide package_s._](python-packages.md)\_\_

To call a standard library function, use the `hash_stblib` object followed by the function name, for example: 

```javascript
function behavior(state, context) {
    let distance = hash_stdlib.distanceBetween(agentA, agentB)
}
```

hash\_stdlib contains the following types of functions:

| Category | Description |
| :--- | :--- |
| [Spatial](spatial.md) | Functions describing and modifying the location of agents in x,y,z space. |
| [Neighbors](neighbors.md) | Functions related to neighbors and neighbor calculations. |
| [Statistical](python-packages.md) | Functions for performing complex statistical modeling or analysis. |





