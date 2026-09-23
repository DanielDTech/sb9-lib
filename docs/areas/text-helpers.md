# Area: text-helpers

Owns every exported function in `src/index.js` and its behaviour on every input.
Tested by `test/index.test.js`.

## Shared contract

All three functions coerce their first argument with `String(text ?? '')`, so
`null` and `undefined` become the empty string rather than throwing, and a
number or an object is stringified. None of them throw, and none of them mutate
anything: given the same input they return the same output.

## slugify(text)

A lowercase, dash separated slug. The text is lowercased, then every run of
characters outside `a-z` and `0-9` collapses to a single dash, then leading and
trailing dashes are stripped. `'Hello World'` becomes `'hello-world'`;
`'  many   spaces  '` becomes `'many-spaces'`; `''` stays `''`.

Accented and non-Latin characters fall outside `a-z0-9` and are therefore
treated as separators, not transliterated: `'café'` becomes `'caf'`. That is the
current contract, not an oversight to fix silently.

## escapeHtml(text)

The five HTML special characters escaped, so any text is safe inside an element:
`&` first (otherwise it would double escape the entities that follow), then `<`,
`>`, `"` and `'`, the last two as `&quot;` and `&#39;`.

Escaping `&` first is the ordering the correctness depends on. It is the one
thing in this area that a rewrite can quietly break, and the test asserts the
whole escaped string rather than the characters individually for that reason.

## truncate(text, max = 80)

The text as is when it fits in `max` characters, otherwise cut to `max - 1`
characters with a single ellipsis character (`…`, U+2026, one character, not
three dots) appended. The result is therefore never longer than `max`. `max`
below 1 yields just the ellipsis, because the slice length is floored at 0.

Length is counted in JavaScript string units, so a character outside the basic
multilingual plane counts as two and can be cut in half. No consumer relies on
that today.
