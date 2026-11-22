# control-surface

Musical control descriptors, bindings, native DOM controls and GPU-rendered performance surfaces. This package exports TypeScript source: use a TypeScript-aware browser bundler. [package.json](package.json) defines subpaths; [src/index.ts](src/index.ts) lists exports.

## Native controls

```ts
import { createDomGui } from "control-surface/dom"
import type { ControlBinding } from "control-surface/types"
import "control-surface/style.css"

let volume = 0.5
const bindings: ControlBinding[] = [{
  key: "instrument-volume",
  sourceLabel: "Instrument",
  descriptor: { id: "volume", label: "Volume", widget: "slider", min: 0, max: 1, step: 0.01 },
  read: () => volume,
  write: value => { volume = Number(value) }
}]
const view = createDomGui(bindings)
document.body.append(view.element)
// On teardown: view.destroy(); view.element.remove()
```

A serializable `ControlDescriptor` describes a control; a runtime `ControlBinding` supplies `read`, `write` and optional `subscribe`. `ComponentUi` can expose descriptors/accessors or a custom DOM GUI. See [types](src/types.ts) and [DOM renderer](src/dom.ts). ADSR values are `[attackSeconds, decaySeconds, sustain0to1, releaseSeconds]`; music grids, strum and relative keys have separate options.

## Performance surfaces

[ControlSurface](src/surface.ts) combines layouts and bound controls. [GpuSurfaceRenderer](src/renderer.ts) draws GPU widgets with native accessible interaction companions. GPU rendering requires its supported graphics backend; native DOM controls do not. Other exports include musical widgets, layout helpers, themes, visualiser frames, pad lights and envelope bindings. Release held controls and destroy owned surfaces on removal.

In audiobus hosts, register `createDomGui` with [setControlRenderer](../audiobus/io/component-ui.ts). Harmoneasy does this in host setup; surrounding app UI follows its non-React conventions.

## Development

Use `pnpm --filter control-surface type-check` from the workspace. `test:browser` and `test:envelope` use Playwright and need its browser runtime. There is no package build script. Repository: [audiobus-control-surface](https://github.com/designerzen/audiobus-control-surface).
