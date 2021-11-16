# Docs output depends on ordering of exports

## Search terms

default export, named export, reference

## Expected Behavior

If the same thing (e.g. class, function) is exported in an entrypoint through
both the default export and a named export, I'd expect the generated
documentation to look the same regardless of which export has been defined
first.

Preferably, if the same thing is exported in both the default and the named
export, the default export should contain references to the named export, not
the other way around.

## Actual Behavior

TypeDoc will produce different documentation pages based on whether the default
export is placed before or after the named export.

For the following code:

```ts
import { SomeClass } from "./someClass";

export default SomeClass;

export { SomeClass };
```

TypeDoc outputs:

![image](https://user-images.githubusercontent.com/26031740/141985674-27ce9679-0b23-40fc-b772-aa5ef1bc30b6.png)


while for this one:

```ts
import { SomeClass } from "./someClass";

export { SomeClass };

export default SomeClass;
```

it shows:

![image](https://user-images.githubusercontent.com/26031740/141985737-d7fa2072-01b8-4a3b-a8b8-179eb564d3a1.png)


## Steps to reproduce the bug

1. Clone [the reproduction
   repository](https://github.com/ejuda/typedoc-default-export-repro).
2. `npm ci`
3. `npm run docs`
4. This will generate two documentation directories:
    - `docs-default-first`, generated using `defaultFirst.ts` as the sole
      entrypoint,
    - `docs-default-last`, generated using `defaultLast.ts` as the sole
      entrypoint.

## Environment

-   Typedoc version: 0.22.8
-   TypeScript version: 4.4.4
-   Node.js version: 14.15.4
-   OS: Windows 10
