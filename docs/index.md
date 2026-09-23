# sb9-lib

Text helpers shared by the sb9 applications. A throwaway repository used to
exercise Trix V2; nothing here is real work.

## What it is

A dependency, not an application. The package exports three pure functions and
nothing else: it opens no port, serves no page, takes no command and reads no
environment. It has no surface of its own, so it is never exercised directly;
the applications built on it are where its behaviour is observed.

## Areas

The code separates into one area. Every source file belongs to it.

| Area | Owns | Lives in |
|---|---|---|
| text-helpers | The three exported functions and their behaviour on every input, including null, undefined and the empty string | `src/index.js`, tested by `test/index.test.js` |

One area, because there is one module with one concern and no internal
boundary: the functions share no state, call nothing, and are imported
individually. See [areas/text-helpers.md](areas/text-helpers.md) for what each
function guarantees.

The line that could one day split the area is a second module with a different
concern (dates, numbers, URLs). Until such a module exists, a split would draw a
boundary through a single file.

## Layout

```
src/index.js            the three helpers, the whole implementation
test/index.test.js      their tests
.github/workflows/ci.yml  install and test on every push and pull request
package.json            the manifest; `main` and `exports` both point at src/index.js
```

There is no build step and no `src` to `dist` transform. `src/index.js` is both
the source and what consumers import.

## Build, run and test

- **Build:** none. The package is ES modules (`"type": "module"`) served as
  written, on Node 22.
- **Run:** nothing to run. The package has no entry point of its own.
- **Test:** `npm test`, which is `node --test`: the Node built-in test runner
  over `test/`. No dependencies, so no install is needed locally.
- **CI:** one GitHub Actions job, `ci`, on every push and pull request:
  checkout, Node 22, `npm install`, `npm test`.

## Delivery

Not delivered to a platform. Not published to npm. Consumers pin it as a git
dependency to a tag:

```
git+https://github.com/DanielDTech/sb9-lib.git#v0.1.0
```

A release here is therefore a tag, and adopting it is a bump in the consumer's
manifest. Because the package has no surface, it carries no QA seat of its own:
it is validated through the applications that depend on it, on their platforms.

A change that alters an exported function's behaviour is a breaking change for
every consumer pinned to a later tag, and is raised with the sessions that own
those applications before it ships.
