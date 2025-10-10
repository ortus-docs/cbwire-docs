# CBWIRE CLI

[Michael Rigsby](https://github.com/mrigsby/) has created a CommandBox CLI tool for CBWIRE that rapidly scaffolds components, templates, and boilerplate code. The CLI supports both standard components and single-file components with extensive customization options.

Install the CBWIRE CLI from [ForgeBox](https://www.forgebox.io/view/cbwire-cli).

{% embed url="https://www.forgebox.io/view/cbwire-cli" %}
CBWIRE CLI on ForgeBox
{% endembed %}

## Installation

Install via CommandBox like so:

```bash
box install cbwire-cli
```

## Basic Usage

Create a simple component:

```bash
cbwire create wire myComponent
```

Create a component with data properties and actions:

```bash
cbwire create wire name="userProfile" dataProps="name,email,isActive" actions="save,cancel" --open
```

## Configuration Options

### Component Structure

| Option           | Type    | Description                                                                             |
| ---------------- | ------- | --------------------------------------------------------------------------------------- |
| `name`           | String  | Component name without extensions. Use `@module` to place in a module's wires directory |
| `singleFileWire` | Boolean | Creates a single-file component combining template and logic                            |
| `outerElement`   | String  | Template outer element type. Defaults to `"div"`                                        |
| `description`    | String  | Component hint description                                                              |

### Data Properties

| Option            | Type   | Description                                                               |
| ----------------- | ------ | ------------------------------------------------------------------------- |
| `dataProps`       | String | Comma-delimited list of data property keys to generate                    |
| `lockedDataProps` | String | Comma-delimited list of data properties to lock from client modifications |

### Component Behavior

| Option               | Type    | Description                                                                          |
| -------------------- | ------- | ------------------------------------------------------------------------------------ |
| `actions`            | String  | Comma-delimited list of action methods to generate                                   |
| `lifeCycleEvents`    | String  | Lifecycle event methods to generate (`onRender`, `onHydrate`, `onMount`, `onUpdate`) |
| `onHydrateProps`     | String  | Properties to create `onHydrate` property methods for                                |
| `onUpdateProps`      | String  | Properties to create `onUpdate` property methods for                                 |
| `includePlaceholder` | Boolean | Inserts placeholder action for lazy loading components                               |

### JavaScript Integration

| Option      | Type    | Description                                                                                                                          |
| ----------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| `jsWireRef` | Boolean | Includes `livewire:init` & `component.init` hooks with `window._{name} = $wire` reference using the name provided in `name` argument |

### File Management

| Option           | Type    | Description                                                      |
| ---------------- | ------- | ---------------------------------------------------------------- |
| `wiresDirectory` | String  | Custom directory for wires. Defaults to standard wires directory |
| `appMapping`     | String  | Application root location in web root (e.g., `MyApp/`)           |
| `open`           | Boolean | Opens generated component and template files                     |
| `force`          | Boolean | Overwrites existing files                                        |

## Advanced Examples

### Module Component

Create a component in a specific module:

```bash
cbwire create wire name="dashboard@AdminModule" dataProps="stats,alerts" actions="refreshData"
```

### Full-Featured Component

Generate a component with comprehensive features:

```bash
cbwire create wire name="userManager" \
  dataProps="users,selectedUser,searchTerm" \
  lockedDataProps="selectedUser" \
  actions="loadUsers,selectUser,deleteUser" \
  lifeCycleEvents="onMount,onUpdate" \
  onUpdateProps="searchTerm" \
  description="User management component" \
  --jsWireRef --open
```

### Single-File Component

Create a single-file component with all features:

```bash
cbwire create wire name="taskList" \
  dataProps="tasks,newTask,filter" \
  lockedDataProps="tasks" \
  actions="addTask,toggleTask,removeTask" \
  outerElement="section" \
  lifeCycleEvents="onMount,onRender" \
  --singleFileWire --jsWireRef --open
```

### Lazy Loading Component

Generate a component optimized for lazy loading:

```bash
cbwire create wire name="heavyChart" \
  dataProps="chartData,isLoaded" \
  actions="loadChart" \
  --includePlaceholder --open
```
