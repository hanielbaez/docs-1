# Behavior Keys

Behavior Keys define the **data** **type** of the fields that a behavior accesses on an agent's state. HASH uses behavior keys to improve the runtime performance of simulations.

![Adding behavior keys](../.gitbook/assets/image%20%2828%29.png)

{% hint style="info" %}
Behavior Keys are **optional** for in-browser simulation runs, but are **required** for cloud runs.
{% endhint %}

### Accessing Behavior Keys

To view the behavior keys associated with a file, click the button containing the _brain_ icon, located beneath the help button, to toggle the key panel's visibility.

### Assigning Behavior Keys

From the behavior key panel you can define the field the behavior will need to access by putting in the name of the field - the same name as its field name on the agents state object - and the type of the field.

![Click the brain icon to toggle the behavior key panel&apos;s visibility](../.gitbook/assets/screen-shot-2020-11-24-at-5.34.20-pm.png)

### How do I know what fields I need to assign?

Any fields your behavior is getting from state, or setting in state, should have an entry in your behavior keys. For example, if your behavior calls `state.set("name", "Bob")`, you should have a behavior key called `name` with type `string`.

### Types

If you've used a statically defined language before - like Rust, Go, or Clojure then you'll already be familiar with strong type systems. You select the type - `string`, `number`, `list`, etc. - of data that a field will store. This improves memory allocation and access speed, as well as making sure that if you assign an incorrect type to the field an error is thrown.  **If you are unsure about typing a field, assign it an 'any' type - or ask us for help!**

Types:

| Type | Example |
| :--- | :--- |
| Strings | "Hello" |
| Booleans | True |
| Numbers \(floats\) | 4.00 |
| Structs \(objects with typed fields\) | {"foo": "bar"} |
| Arrays \(ordered collections containing the same type\) | \[1,2,3\] |
| Fixed-size Arrays | \[1,2,3\] \(max 3 elements\)  |
| Any | JS objects, arrays, Python objects, etc |

The `any` type designation can apply, appropriately enough, to any data type - it tells HASH to store the value as JSON and deserialize it at runtime. The generic `any` type can be used to simplify behavior key representations, but will result in slower simulation execution speeds than other type designations, so should be used sparingly.

All fields are nullable \(which means they can be assigned a null value, or may exist with no value assigned\) by default. You can turn off nullability, making a field non-nullable, which will improve simulation performance and memory utilization further.

For complex data types - lists, fixed sized lists, and structs, **you must click the tree icon** to assign the data types of members of the list or struct.

{% hint style="info" %}
Dynamically populated structs should be assigned the `any` type.
{% endhint %}

![Click the tree icon on the right to assign the next level of data types](../.gitbook/assets/screen-shot-2020-11-24-at-5.36.17-pm.png)

Data type fields must be the same across behaviors. For instance if field **foo** in behavior A has type: number, field **foo** \(assuming its the same field\) in behavior B must have type: number.

{% hint style="info" %}
Field Names at the top level of your keys cannot match built-in fields \(e.g. `agent_id`, `position`\) and cannot start with double-underscore \(e.g. `__age`\), which are reserved for engine specific information. Fields below the top level (i.e. as a child of a top-level field) may match those names.
{% endhint %}
