> [!IMPORTANT]
> ## This component has moved
>
> The Branching Component is now developed and released as the **Branching Diagram** in **[mustry-perspective-component-module](https://github.com/Mustry-Solutions/mustry-perspective-component-module)**. This repository is kept for reference and is no longer maintained.
>
> **→ [Go to mustry-perspective-component-module](https://github.com/Mustry-Solutions/mustry-perspective-component-module)** · [What changed in the move](#moving-to-mustry-perspective-component-module)

# Mustry UI Module

An [Ignition Perspective](https://www.inductiveautomation.com/) module providing a **Branching Component**: a horizontal tree/flow renderer for track-and-trace, routing and decision paths.

![The branching component rendering a tree of coloured nodes with connecting paths](https://github.com/user-attachments/assets/41f57f43-b12e-4dfa-8aa7-db0f7995ec4e)

| | |
|---|---|
| **Module ID** | `org.mustry.mustryui` |
| **Latest release** | [v2.0.0](https://github.com/Mustry-Solutions/ignition-branching-component-module/releases/latest) — Ignition 8.3 |
| **Requires** | Ignition 8.3+, Java 17 |
| **Status** | Unmaintained — see the notice above |

## Install

1. Download `Mustry_UI.modl` from the [releases page](https://github.com/Mustry-Solutions/ignition-branching-component-module/releases).
2. In the Gateway web interface, open **Config → Modules** and install it.
3. The **Branching** component appears in the Perspective component palette in the Designer.

Releases before v2.0.0 target Ignition 8.1; v2.0.0 is the 8.3 build.

## Quick start

Drop a **Branching** component on a view and bind its `data` property to an array of nodes. Each node needs an `id` and a `category`; `nextId` lists the nodes it draws a path to. The root is detected automatically.

```json5
[
  { "id": 0, "name": "Intake",  "category": 0, "nextId": [1, 2], "color": "#0a66c2" },
  { "id": 1, "name": "QA pass", "category": 0, "nextId": [3],    "color": "#2e7d32" },
  { "id": 2, "name": "QA fail", "category": 1, "nextId": [4],    "color": "#d32f2f" },
  { "id": 3, "name": "Ship",    "category": 0, "nextId": [],     "color": "#2e7d32",
    "icon": { "path": "material/local_shipping", "color": "white" },
    "tooltip": "**Ready to ship.**\nScan the pallet label." },
  { "id": 4, "name": "Scrap",   "category": 1, "nextId": [],     "color": "#d32f2f" }
]
```

Two rules decide whether a tree draws: there must be exactly one root — a node with outgoing paths that no other node points at — and every node must be reachable from it, or it is not displayed. `category` is the row: nodes sharing a category are drawn on the same level, and a higher number sits lower.

Note that a path pointing back to an earlier node (a rework loop, say) is drawn as a stray diagonal by this component; the successor routes those properly.

Full property reference, including per-node styling and tooltips: **[doc/data_structure.md](doc/data_structure.md)**.

## Documentation

| Document | What it covers |
|---|---|
| [Data structure](doc/data_structure.md) | Component properties and the node data format |
| [Ignition component](doc/Ignition_component.md) | How the module is put together (Java scopes, web bundle, registration) |
| [Signing the module](doc/signing_module.md) | Keystore/certificate setup for producing a signed `.modl` |

## Build from source

```bash
./gradlew clean build
```

The module is written to `build/Mustry_UI.modl`. Signing needs a keystore and a `gradle.properties` holding the credentials — see [doc/signing_module.md](doc/signing_module.md). To build unsigned, set `skipModlSigning.set(true)` in `build.gradle.kts`.

`gradle.properties` and `certificates/` are gitignored; never commit them.

The repository also contains `dev_react/`, a standalone React harness used to iterate on the component outside Ignition.

## Moving to mustry-perspective-component-module

The successor is a **superset in capability**, adding vertical orientation, direction arrows, per-edge labels and styling, cleaner loop/back-edge routing, localization, selection events, and validation feedback for datasets that won't fully draw. It is free and open source under Apache-2.0, targets Ignition 8.3, and ships thirteen further components alongside the diagram.

Node data carries across unchanged — `id`, `name`, `color`, `nextId`, `category`, `fill`, `style`, `icon`, `tooltip` and `tooltipStyle` all mean the same thing. Two connector-colour properties were **replaced rather than renamed**, so a view built here needs adjusting:

| Here | In the new module |
|---|---|
| `connectionColor` (component property) | The `--brn-line` CSS variable, set via a style class or project stylesheet |
| `colorOutgoing` (per-node boolean) | Always on: a connector takes its source node's colour. Override an individual edge with `data.edgeLabels[].color` |

The practical difference is that `colorOutgoing` was opt-in per node, whereas the new component always colours a connector from its source node. If a node has no colour, its connectors fall back to `--brn-line`.

## License

No license file was ever added to this repository. The successor is licensed under [Apache-2.0](https://github.com/Mustry-Solutions/mustry-perspective-component-module/blob/main/LICENSE).
