# sb9-lib architecture

Text helpers shared by the sb9 applications. The package has no surface of its
own: no server, no page, no command line. It is exercised through the
applications built on it, and it is delivered to them as source.

## Areas

The code separates into one area, because there is one concern: pure text
transformation with no input or output of its own.

| Area | Owns | Lives in |
|---|---|---|
| text-helpers | The three exported functions and their contracts: `slugify`, `escapeHtml`, `truncate` | `src/index.js`, tested by `test/index.test.js` |

`docs/areas/text-helpers.md` states each function's contract. There is no
second area to split against: every file in `src/` belongs to this one.

## How it is built

Nothing is built. The package is plain ES modules on Node 22 with no
dependencies, no bundler and no compile step. `package.json` declares
`"type": "module"` and points both `main` and `exports` at `src/index.js`, so a
consumer importing the package gets that file as written.

## How it is run

It is not run. There is no entry point to execute and no script but the test
script. The only way to exercise this code is to import it from an application.

## How it is tested

`npm test`, which runs `node --test` over `test/`. The tests use `node:test`
and `node:assert/strict`; there is no framework to install. CI
(`.github/workflows/ci.yml`) runs `npm install` then `npm test` on Node 22 for
every push and every pull request, and that job is the gate on merging.

## How it is delivered

Not on a platform. This package is consumed as a git dependency pinned to a
tag:

```
git+https://github.com/DanielDTech/sb9-lib.git#v0.1.0
```

A release is therefore a tag, and a consumer takes it by bumping the ref it
pins. Nothing is published to a registry and nothing is deployed. Because the
package has no surface, it carries no QA seat of its own: the applications that
import it are where its behaviour is validated, by their QA, on their platform.

## What depends on it

`sb9-web` does not import this package today; it calls an API for the same
work. Any consumer that appears is a downstream of a tag, never of `master`, so
a change here reaches nobody until a tag is cut and a consumer bumps it.
