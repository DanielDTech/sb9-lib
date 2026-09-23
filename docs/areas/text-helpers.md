# Area: text-helpers

Owns every exported function in `src/index.js` and the contract each one makes
to the applications that import it. Tested by `test/index.test.js`.

## The contracts

**`slugify(text)`** returns a lowercase, dash separated slug. Every run of
characters outside `a-z` and `0-9` becomes a single dash, and leading and
trailing dashes are stripped. Non strings are coerced; `null` and `undefined`
become the empty string. Accented and non Latin characters are separators
rather than letters, so text outside the ASCII range can slug to the empty
string.

**`escapeHtml(text)`** escapes the five HTML special characters `&`, `<`, `>`,
`"` and `'`, in that order so an escaped ampersand is never escaped twice. The
result is safe as text inside an element or inside a quoted attribute value. It
is not safe inside a script block, a style block or an unquoted attribute, and
it does not sanitise a URL.

**`truncate(text, max = 80)`** returns the text unchanged when its length is at
most `max`, and otherwise the first `max - 1` characters followed by an
ellipsis, so the result is never longer than `max`. Length is counted in
JavaScript string units, which means an astral character counts as two and can
be cut in half.

## Conventions

Every function coerces its argument with `String(text ?? '')` and returns a
string; none of them throws and none of them has a side effect. A new helper
follows the same shape, and arrives with its tests in the same file.
