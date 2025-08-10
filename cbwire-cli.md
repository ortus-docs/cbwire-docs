# CBWIRE CLI

[Michael Risby](https://github.com/mrigsby/) has created a [CommandBox](https://www.ortussolutions.com/products/commandbox) CLI for CBWIRE that you can use to quickly scaffold out CBWIRE components. You can find the CBWIRE CLI on [ForgeBox](https://www.forgebox.io/view/cbwire-cli).

{% embed url="https://www.forgebox.io/view/cbwire-cli" %}
CBWIRE CLI on ForgeBox
{% endembed %}

## Installation

Install via CommandBox like so:

```bash
box install cbwire-cli
```

## Command Line Arguments

| Argument             | Type    | Description                                                                                                                          |
| -------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| `name`               | String  | Name of the wire to create without extensions. Use `@module` to place in a module's wires directory.                                 |
| `dataProps`          | String  | A comma-delimited list of data property keys to add.                                                                                 |
| `lockedDataProps`    | String  | A comma-delimited list of data property keys to lock.                                                                                |
| `actions`            | String  | A comma-delimited list of actions to generate.                                                                                       |
| `outerElement`       | String  | The outer element type to use for the wire. Defaults to `"div"`.                                                                     |
| `jsWireRef`          | Boolean | If `true`, includes `livewire:init` & `component.init` hooks and assigns a reference as `window.wirename = $wire`.                   |
| `lifeCycleEvents`    | String  | A comma-delimited list of lifecycle event names to generate. If none provided, only `onMount()` will be generated but commented out. |
| `onHydrateProps`     | String  | A comma-delimited list of properties to create `onHydrate()` property methods for in the wire.                                       |
| `onUpdateProps`      | String  | A comma-delimited list of properties to create `onUpdate()` property methods for in the wire.                                        |
| `wiresDirectory`     | String  | The directory where your wires are stored. Defaults to the standard wires directory.                                                 |
| `appMapping`         | String  | The root location of the application in the web root (e.g., `MyApp/`) or leave blank if in the root.                                 |
| `description`        | String  | The wire component's hint description.                                                                                               |
| `open`               | Boolean | If `true`, opens the wire component & template once generated.                                                                       |
| `force`              | Boolean | If `true`, forces overwrite of existing wires.                                                                                       |
| `singleFileWire`     | Boolean | If `true`, creates a single file wire.                                                                                               |
| `includePlaceholder` | Boolean | If `true`, inserts a placeholder action in the wire component for lazy loading wires.                                                |



## Examples

### **Super Basic Example**

```bash
cbwire create wire myWireName
```

### **Basic Example**

{% code overflow="wrap" %}
```bash
cbwire create wire name="myWireName" dataProps="counter1,counter2,counter3" actions="saveSomething,doSomething,GetSomething" --jsWireRef --open
```
{% endcode %}

### **Basic Example with module name using myWireName@MyModuleName**

{% code overflow="wrap" %}
```bash
cbwire create wire name="myWireName@MyModuleName" dataProps="counter1,counter2,counter3" actions="saveSomething,doSomething,GetSomething" --jsWireRef --open
```
{% endcode %}

### **Many options (WITHOUT singleFileWire)**

{% code overflow="wrap" %}
```bash
cbwire create wire name="myWireName" dataProps="counter1,counter2,counter3" lockedDataProps="counter2,counter3" actions="saveSomething,doSomething,GetSomething" outerElement="p" lifeCycleEvents="onRender,onHydrate,onMount,onUpdate" onHydrateProps="counter2,counter3" onUpdateProps="counter1,counter2" description="This is my wire description" --jsWireRef --open --force
```
{% endcode %}

### **Many options (WITH singleFileWire)**

{% code overflow="wrap" %}
```bash
cbwire create wire name="myWireName" dataProps="counter1,counter2,counter3" lockedDataProps="counter2,counter3" actions="saveSomething,doSomething,GetSomething" outerElement="p" lifeCycleEvents="onRender,onHydrate,onMount,onUpdate" onHydrateProps="counter2,counter3" onUpdateProps="counter1,counter2" description="This is my wire description" --jsWireRef --open --force --singleFileWire
```
{% endcode %}
