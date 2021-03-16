# Libraries

## HASH Standard Library

HASH provides a set of useful functions to help simplify simulation construction. These are provided through a standard library, `hash_stdlib`

Currently the Standard Library is only accessible in JavaScript; however, we're expanding this to include equivalent functions for Python and Rust. The Python language environment [also provides access to](https://docs.hash.ai/core/libraries/python-packages) a wide array of scientific Python packages_._

### Types of functions in the HASH Standard Library

The HASH Standard Library contains the following types of functions:

| Category | Description |
| :--- | :--- |
| [Spatial](hash/spatial.md) | Functions describing and modifying the location of agents in x,y,z space. |
| [Neighbors](hash/neighbors.md) | Functions related to neighbors and neighbor calculations. |
| [Statistical](hash/javascript-libraries.md) | Functions for performing complex statistical modeling or analysis. |
| [Random](hash/random.md) | Functions related to random number generation |
| Agent | Functions for helping build and use agents. |

### Using the HASH Standard Library

To call a standard library function, use the `hash_stblib` object followed by the function name, for example: 

```javascript
function behavior(state, context) {
    let distance = hash_stdlib.distanceBetween(agentA, agentB)
}
```

## Scientific Python

HASH provides access to a number of scientific Python packages which can be utilized in simulations. [Read more &gt;](https://docs.hash.ai/core/libraries/python-packages)

