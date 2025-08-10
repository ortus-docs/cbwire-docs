# Creating Components

You can scaffold cbwire components quickly using the [commandbox-cbwire](https://github.com/commandbox-modules/commandbox-cbwire) module.

## Installing commandbox-cbwire

To get started open the [CommandBox ](https://commandbox.ortusbooks.com/)binary or enter the shell by typing `box` in your terminal or console. Then let's install our scaffolding module.

```
install commandbox-cbwire
```

## Creating Components

Let's create a **Counter** cbwire component that we will use to list our favorite movies.

```javascript
cbwire create Counter
// Created /wires/Counter.cfc
// Created /views/wires/counter.cfm
// Created /tests/specs/integration/wires/CounterTest.cfc
```

You can provide named arguments as well.

```javascript
cbwire create name=Counter views=false
// Created /wires/Counter.cfc
// Created /tests/specs/integration/wires/CounterTest.cfc
```

Below is the entire signature of arguments you can provide to the `create` command.

```javascript
/**
    * @name Name of the component to create without the .cfc.
    * @actions Comma-delimited list of actions to generate.
    * @views Generate a view for the cbwire component.
    * @viewsDirectory Directory where your views are stored. Only used if views is set to true.
    * @integrationTests Generate the integration test component
    * @testsDirectory Your integration tests directory. Only used if integrationTests is true
    * @directory Base directory to create your handler in and creates the directory if it does not exist. Defaults to 'handlers'.
    * @description Component hint description.
    * @open Opens the component (and test(s) if applicable) once generated.
    **/
    function run(
    required string name,
    string actions = "",
    boolean views = true,
    string viewsDirectory = "views/wires",
    boolean integrationTests = true,
    string testsDirectory = "tests/specs/integration/wires",
    string directory = "wires",
    string description = "I am a new cbwire component.",
    boolean open = false
)
```
