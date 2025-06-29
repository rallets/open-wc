# Change Log

## 3.0.6

### Patch Changes

- Updated dependencies [b4b836dc]
  - @open-wc/dedupe-mixin@2.0.0

## 3.0.5

### Patch Changes

- 479a956d: Marked `@open-wc/scoped-elements` as side-effect free

## 3.0.4

### Patch Changes

- fec89f1d: Backport registry creation check from v2

## 3.0.3

### Patch Changes

- 3e6fff71: fix types for angular

## 3.0.2

### Patch Changes

- c386fcf9: fix: types again

## 3.0.1

### Patch Changes

- 12011875: Fix types for scoped elements

## 3.0.0

### Major changes

- See the `MIGRATION.md` for more information

## 2.2.4

### Patch Changes

- 84e38ab1: Use split versions for all lit dependencies

## 2.2.3

### Patch Changes

- c6cdea6f: fix: Backport passing the scoped custom element registry via the registry option in addition to the customElement option to account for the change in the specification

## 2.2.2

### Patch Changes

- 0402c80f: fix: add scopedElementsVersion

## 2.2.1

### Patch Changes

- 9d86f689: Add "type": "module" to package.json so build tools won't assume this is a CommonJS module.

## 2.2.0

### Minor Changes

- 935c8ffe: Drop support for Node@14

### Patch Changes

- 80c6ae66: Use latest @web/\* dependencies.
  Upgrade Rollup to `v3.x`
- Updated dependencies [935c8ffe]
  - @open-wc/dedupe-mixin@1.4.0

## 2.1.5

### Patch Changes

- 52f3fa6d: fix: getScopedTagName bug

## 2.1.4

### Patch Changes

- 89fb5601: Add type entrypoint which enables you to get more advanced type information like

  ```ts
  import { ScopedElementsHost } from "@open-wc/scoped-elements/types.js";
  ```

## 2.1.3

### Patch Changes

- 61e2668f: update eslint, eslint-config-airbnb-base and eslint-plugin-import

## 2.1.2

### Patch Changes

- bf99826e: Check if `__registry` actually exists on the current class, not a parent.
- 12b9a736: Fixed compatibility with Typescript 4.7 nodeResolution: "node16".

## 2.1.1

### Patch Changes

- dbc2b9e2: Update `ScopedElementsMixin` type info to include `createScopedElement`

## 2.1.0

### Minor Changes

- 3bb2f979: BREAKING CHANGE: Work without polyfill if possible (and do not auto load polyfill)

  ScopedElementsMixin 2.x tried to be as convenient as possible by automatically loading the scoped custom elements registry polyfill.
  This however led to a fatal error whenever you registered any component before ScopedElementsMixin was used.

  The error especially happened when you would import a component applying ScopedElementsMixin into an existing application, fundamentally going against the "import and use" nature of web components.

  Therefore, we decided to not load the polyfill inside the mixin, but let the developer (optionally) load it on top of his/her app.
  This means that, depending on the app of the developer, the change can be breaking. After updating to this latest version, two scenarios will be possible:

  1. **everything is fixed and less js is loaded than before**: the app doesn't need scoping (in this case the app either continues to work as is, or the fatal error mentioned above will disappear)
  2. **the fatal error is replaced by a console error from ScopedElementsMixin**: the app needs scoping, because different versions of the same component are used (in this case, the fatal error mentioned above will disappear and a console error asking you to load the polyfill on top of your app will appear)

  Only the second case requires an action. The remaining error can be resolved via a small migration path, a breaking bugfix if you will, which should be treated as if it were a security breaking change: consumers do the breaking bug fix on their end, because releasing a new major version would not be beneficial, neither to us nor to our consumers.

  Not loading the polyfill by default will also allow sites to opt out of it altogether. This means until the browser ships scoped registries, the developer can choose to fall back to the global registry, by not loading the polyfill. This will save bandwidth & complexity since it doesn't need to be loaded by the client in that case.

  All previous 2.x versions will be deprecated and scoped element will behave as follows:

  1. If polyfill is not loaded it will use the global registry as a fallback
  2. Log error if actually scoping is needed and polyfill is not loaded
  3. If you manually create elements you will need to handle polyfilled and not polyfilled cases now

  ```diff
  -  const myButton = this.shadowRoot.createElement('my-button');
  +  const myButton = this.createScopedElement('my-button');
  ```

  This also removes `@webcomponents/scoped-custom-element-registry` as a production dependency.

  If you need scoping be sure to load the polyfill before any other web component gets registered.

  It may look something like this in your HTML

  ```html
  <script src="/node_modules/@webcomponents/scoped-custom-element-registry/scoped-custom-element-registry.min.js"></script>
  ```

  or if you have an SPA you can load it at the top of your app shell code

  ```js
  import "@webcomponents/scoped-custom-element-registry";
  ```

  You need scoping if you want to

  - use 2 major versions of a web component (e.g. in an SPA pageA uses 1.x and pageB uses 2.x of color-picker)
  - or you want to use the same tag name with different implementations (use tag color-picker from foo here and from bar here)

## 2.0.1

### Patch Changes

- 1649ba46: Release bump version as major versions have already been used and unpublished in an accidental publish about a year ago.

## 2.0.0

### Major Changes

- edca5a82: Adds compatibility for [lit](https://lit.dev/) with `lit-html v2` and `lit-element v3`.

  - This version does NOT work with lit-element v2 - please use Scoped Elements v1 for it
  - Uses a `CustomElementsRegistry` instance for each component class instead of for each component instance. In case you need to have a registry for each component instance, you must override the registry `get` and `set` methods to bind the registry to the component instance

    ```js
    /** @override */
    get registry() {
      return this.__registry;
    }

    /** @override */
    set registry(registry) {
      this.__registry = registry;
    }
    ```

  - `getScopedTagName` became deprecated - use the native `el.tagName` instead

### Minor Changes

- 1e54d297: Use the webcomponents polyfill instead of the forked one

### Patch Changes

- 4b9ea6f6: Use lit@2.0 stable based dependencies across the project.
- c05d92fb: Mark `loadPolyfill.js` as a side effect
- 0513917c: Keep deprecated static getScopedTagName function
- ff17798f: Adjust the renderBefore node so that any styles in Lit content render before adoptedStyleSheets.
- a0b5e360: fix getScopedTagName returning the tag that was passed in

## 2.0.0-next.6

### Patch Changes

- 4b9ea6f6: Use lit@2.0 stable based dependencies across the project.

## 2.0.0-next.5

### Patch Changes

- ff17798f: Adjust the renderBefore node so that any styles in Lit content render before adoptedStyleSheets.

## 2.0.0-next.4

### Minor Changes

- 1e54d297: Use the webcomponents polyfill instead of the forked one

## 2.0.0-next.3

### Patch Changes

- 0513917c: Keep deprecated static getScopedTagName function

## 2.0.0-next.2

### Patch Changes

- a0b5e360: fix getScopedTagName returning the tag that was passed in

## 2.0.0-next.1

### Patch Changes

- c05d92fb: Mark `loadPolyfill.js` as a side effect

## 2.0.0-next.0

### Major Changes

- edca5a82: Adds compatibility for [lit](https://lit.dev/) with `lit-html v2` and `lit-element v3`.

  - This version does NOT work with lit-element v2 - please use Scoped Elements v1 for it
  - Uses a `CustomElementsRegistry` instance for each component class instead of for each component instance. In case you need to have a registry for each component instance, you must override the registry `get` and `set` methods to bind the registry to the component instance

    ```js
    /** @override */
    get registry() {
      return this.__registry;
    }

    /** @override */
    set registry(registry) {
      this.__registry = registry;
    }
    ```

  - `getScopedTagName` became deprecated - use the native `el.tagName` instead

## 1.3.3

### Patch Changes

- 31ff454a: Add constructor to ScopedElementsHost type so that base constructors have the same return type as extensions. See [TypeScript issue](https://github.com/microsoft/TypeScript/issues/40110).

## 1.3.2

### Patch Changes

- 9fb1c131: refactor to remove optional chaining syntax in Cache file for better tooling compatibility

## 1.3.1

### Patch Changes

- 04666893: Fix types

## 1.3.0

### Minor Changes

- 95d055dc: Define lazy components per Scoped Element instance instead of by Scoped Element class. This forces `getScopedTagName` method to stop being a static method.

## 1.2.4

### Patch Changes

- 4a81d791: Add types folder to npm artifacts

## 1.2.3

### Patch Changes

- 17e9e7dc: Change type distribution workflow

All notable changes to this project will be documented in this file.
See [Conventional Commits](https://conventionalcommits.org) for commit guidelines.

## [1.2.2](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@1.2.1...@open-wc/scoped-elements@1.2.2) (2020-08-19)

### Bug Fixes

- **scoped-elements:** add host to the mixin type for static props ([88ffd99](https://github.com/open-wc/open-wc/commit/88ffd995efeb94d79ee686f665d18e8ca405e3bc))

## [1.2.1](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@1.2.0...@open-wc/scoped-elements@1.2.1) (2020-08-14)

### Bug Fixes

- **scoped-elements:** make code ie11 compatible ([08483df](https://github.com/open-wc/open-wc/commit/08483df864276f7d1ac5a72ca0fc1b37ad6367df))

# [1.2.0](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@1.1.2...@open-wc/scoped-elements@1.2.0) (2020-08-05)

### Features

- **scoped-elements:** add data-tag-name with the original tagName ([d97454e](https://github.com/open-wc/open-wc/commit/d97454ea7e39457b754ee907c6d686aba5d5078b))

## [1.1.2](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@1.1.1...@open-wc/scoped-elements@1.1.2) (2020-07-08)

**Note:** Version bump only for package @open-wc/scoped-elements

## [1.1.1](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@1.1.0...@open-wc/scoped-elements@1.1.1) (2020-04-26)

### Bug Fixes

- **scoped-elements:** duplicate definition ([9aecafb](https://github.com/open-wc/open-wc/commit/9aecafbd68b8eb6a77f9d6fd8420e7693c3e8eef))

# [1.1.0](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@1.0.9...@open-wc/scoped-elements@1.1.0) (2020-04-25)

### Features

- **scoped-elements:** self-registering components compatibility ([d4806e4](https://github.com/open-wc/open-wc/commit/d4806e43972c6cf2b895998a521ff89d5f16583d))

## [1.0.9](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@1.0.8...@open-wc/scoped-elements@1.0.9) (2020-04-20)

**Note:** Version bump only for package @open-wc/scoped-elements

## [1.0.8](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@1.0.7...@open-wc/scoped-elements@1.0.8) (2020-04-12)

**Note:** Version bump only for package @open-wc/scoped-elements

## [1.0.7](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@1.0.6...@open-wc/scoped-elements@1.0.7) (2020-04-06)

**Note:** Version bump only for package @open-wc/scoped-elements

## [1.0.6](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@1.0.5...@open-wc/scoped-elements@1.0.6) (2020-04-05)

### Bug Fixes

- **scoped-elements:** define unused lazy scoped elements ([5b863a2](https://github.com/open-wc/open-wc/commit/5b863a2bbaa8647b29cc6818ffb6dadc7297caae))

## [1.0.5](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@1.0.4...@open-wc/scoped-elements@1.0.5) (2020-03-28)

### Bug Fixes

- **scoped-elements:** elements not scoped by directives ([71f7438](https://github.com/open-wc/open-wc/commit/71f7438308d010148f80abdafd7e7dcb828e529c))

## [1.0.4](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@1.0.3...@open-wc/scoped-elements@1.0.4) (2020-03-26)

**Note:** Version bump only for package @open-wc/scoped-elements

## [1.0.3](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@1.0.2...@open-wc/scoped-elements@1.0.3) (2020-03-25)

### Bug Fixes

- **scoped-elements:** getScopedTagName returns undefined ([a96c675](https://github.com/open-wc/open-wc/commit/a96c675d36b2c04ba84c11c706400cd8ecaaf584))

## [1.0.2](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@1.0.1...@open-wc/scoped-elements@1.0.2) (2020-03-24)

**Note:** Version bump only for package @open-wc/scoped-elements

## [1.0.1](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@1.0.0...@open-wc/scoped-elements@1.0.1) (2020-03-20)

**Note:** Version bump only for package @open-wc/scoped-elements

# [1.0.0](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.7.1...@open-wc/scoped-elements@1.0.0) (2020-03-19)

### Bug Fixes

- **scoped-elements:** fix some jsDocs ([b4d4fc1](https://github.com/open-wc/open-wc/commit/b4d4fc14a01c35e9b50ea7c87110853e0b18b1a3))

### BREAKING CHANGES

- **scoped-elements:** getScopedTagName is not available anymore as a function but
            a ScopedElementsMixin method.

## [0.7.1](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.7.0...@open-wc/scoped-elements@0.7.1) (2020-03-19)

**Note:** Version bump only for package @open-wc/scoped-elements

# [0.7.0](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.6.9...@open-wc/scoped-elements@0.7.0) (2020-03-19)

### Features

- **scoped-elements:** add support for lazy elements ([0d67b9f](https://github.com/open-wc/open-wc/commit/0d67b9f8851e73a1a2dc48fe66717a62822fc4b7))

## [0.6.9](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.6.8...@open-wc/scoped-elements@0.6.9) (2020-03-19)

**Note:** Version bump only for package @open-wc/scoped-elements

## [0.6.8](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.6.7...@open-wc/scoped-elements@0.6.8) (2020-03-15)

**Note:** Version bump only for package @open-wc/scoped-elements

## [0.6.7](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.6.6...@open-wc/scoped-elements@0.6.7) (2020-03-15)

**Note:** Version bump only for package @open-wc/scoped-elements

## [0.6.6](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.6.5...@open-wc/scoped-elements@0.6.6) (2020-03-11)

**Note:** Version bump only for package @open-wc/scoped-elements

## [0.6.5](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.6.4...@open-wc/scoped-elements@0.6.5) (2020-03-10)

**Note:** Version bump only for package @open-wc/scoped-elements

## [0.6.4](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.6.3...@open-wc/scoped-elements@0.6.4) (2020-03-10)

**Note:** Version bump only for package @open-wc/scoped-elements

## [0.6.3](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.6.2...@open-wc/scoped-elements@0.6.3) (2020-03-08)

**Note:** Version bump only for package @open-wc/scoped-elements

## [0.6.2](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.6.1...@open-wc/scoped-elements@0.6.2) (2020-03-06)

**Note:** Version bump only for package @open-wc/scoped-elements

## [0.6.1](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.6.0...@open-wc/scoped-elements@0.6.1) (2020-03-02)

**Note:** Version bump only for package @open-wc/scoped-elements

# [0.6.0](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.5.0...@open-wc/scoped-elements@0.6.0) (2020-02-29)

### Features

- **rollup-plugin-html:** first release ([9acb29a](https://github.com/open-wc/open-wc/commit/9acb29ac84b0ef7e2b06c57043c9d2c76d5a29c0))

# [0.5.0](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.4.1...@open-wc/scoped-elements@0.5.0) (2020-02-24)

### Features

- **scoped-elements:** add getScopedTagName function ([21132e0](https://github.com/open-wc/open-wc/commit/21132e00f72111dbaaedddf659a84014359a4232))

## [0.4.1](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.4.0...@open-wc/scoped-elements@0.4.1) (2020-02-23)

**Note:** Version bump only for package @open-wc/scoped-elements

# [0.4.0](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.3.1...@open-wc/scoped-elements@0.4.0) (2020-02-19)

### Features

- **scoped-elements:** implement as ScopedElementsMixin ([b4f6483](https://github.com/open-wc/open-wc/commit/b4f648362234949572e1215e0b65df415e63d65c))

## [0.3.1](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.3.0...@open-wc/scoped-elements@0.3.1) (2020-02-13)

### Bug Fixes

- **scoped-elements:** support extending elements ([#1272](https://github.com/open-wc/open-wc/issues/1272)) ([9868bc7](https://github.com/open-wc/open-wc/commit/9868bc7a1bc94d4e54651c92d458f3b413a1ebda))

# [0.3.0](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.2.15...@open-wc/scoped-elements@0.3.0) (2020-02-13)

### Bug Fixes

- **scoped-elements:** keep descriptive tag name when building ([64b8a44](https://github.com/open-wc/open-wc/commit/64b8a44d23b51a7843185462f8b2204e522a07c8))

### Features

- **scoped-elements:** add '-se' suffix to all scoped tag names ([28c7011](https://github.com/open-wc/open-wc/commit/28c7011bb834638a6448b420ea4a4d844b151fe7))

## [0.2.15](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.2.14...@open-wc/scoped-elements@0.2.15) (2020-02-12)

### Bug Fixes

- add setting so webpack can apply tree shaking on it ([#1337](https://github.com/open-wc/open-wc/issues/1337)) ([b5fdf5c](https://github.com/open-wc/open-wc/commit/b5fdf5c2f124913ffd07b97dbbb666661e4ef480))

## [0.2.14](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.2.13...@open-wc/scoped-elements@0.2.14) (2020-02-10)

**Note:** Version bump only for package @open-wc/scoped-elements

## [0.2.13](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.2.12...@open-wc/scoped-elements@0.2.13) (2020-02-10)

**Note:** Version bump only for package @open-wc/scoped-elements

## [0.2.12](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.2.11...@open-wc/scoped-elements@0.2.12) (2020-02-09)

**Note:** Version bump only for package @open-wc/scoped-elements

## [0.2.11](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.2.10...@open-wc/scoped-elements@0.2.11) (2020-02-09)

**Note:** Version bump only for package @open-wc/scoped-elements

## [0.2.10](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.2.9...@open-wc/scoped-elements@0.2.10) (2020-02-06)

**Note:** Version bump only for package @open-wc/scoped-elements

## [0.2.9](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.2.8...@open-wc/scoped-elements@0.2.9) (2020-02-06)

**Note:** Version bump only for package @open-wc/scoped-elements

## [0.2.8](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.2.7...@open-wc/scoped-elements@0.2.8) (2020-02-03)

**Note:** Version bump only for package @open-wc/scoped-elements

## [0.2.7](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.2.6...@open-wc/scoped-elements@0.2.7) (2020-02-03)

**Note:** Version bump only for package @open-wc/scoped-elements

## [0.2.6](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.2.5...@open-wc/scoped-elements@0.2.6) (2020-02-02)

**Note:** Version bump only for package @open-wc/scoped-elements

## [0.2.5](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.2.3...@open-wc/scoped-elements@0.2.5) (2020-01-31)

### Bug Fixes

- skip brooken published versions ([25d21de](https://github.com/open-wc/open-wc/commit/25d21def522f22f98fc8c71b4c055617089c0e23))

## [0.2.3](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.2.2...@open-wc/scoped-elements@0.2.3) (2020-01-27)

**Note:** Version bump only for package @open-wc/scoped-elements

## [0.2.2](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.2.1...@open-wc/scoped-elements@0.2.2) (2020-01-27)

**Note:** Version bump only for package @open-wc/scoped-elements

## [0.2.1](https://github.com/open-wc/open-wc/compare/@open-wc/scoped-elements@0.2.0...@open-wc/scoped-elements@0.2.1) (2020-01-19)

**Note:** Version bump only for package @open-wc/scoped-elements

# 0.2.0 (2020-01-19)

### Features

- **scoped-elements:** add scoped elements feature ([a7e195b](https://github.com/open-wc/open-wc/commit/a7e195b893deaed8041b2952f51a5229e33134a1))
