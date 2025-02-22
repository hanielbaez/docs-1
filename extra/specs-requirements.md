# System Requirements

Certain HASH features rely on cutting-edge technology that may not be supported in every browser and on every platform. For whatever browser you choose to use, we recommend the latest and greatest version to ensure support. We have found the best performance and user experience on the latest Chrome release.

## hCore

### Browser Compatibility

|  | Chrome | Firefox | Safari |
| :--- | :--- | :--- | :--- |
| Local JS Behaviors | ✔ | ✔ | ✔ |
| Local Python Behaviors | ✔ | ✔ | 𝙓 |
| Local Rust Behaviors | 𝙓 | 𝙓 | 𝙓 |
| Cloud JS Behaviors | ✔ | ✔ | ✔ |
| Cloud Python Behaviors | ✔ | ✔ | ✔ |
| Cloud Rust behaviors | ✔ | ✔ | ✔ |

{% hint style="info" %}
Simulations can be run on [HASH Cloud](../h.cloud.md) with results streamed back to any browser.
{% endhint %}

### Hardware Requirements

We recommend ensuring your device has at least 8GB of ram and a decent graphics card to create, run and explore most normal-sized simulations. If you stumble into performance issues, try using the "Run in Cloud" button in hCore to offload the heavy-lifting and computation to [hCloud](../h.cloud.md).

Local simulations run in hCore typically scale easily to ~2,000 agents, but if your simulation is much larger than that, or the number of agents grows exponentially, executing on HASH Cloud may be a better fit.

