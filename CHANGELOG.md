# Changelog

## 1.0.1 — 2026-10-09

The API and behaviour are unchanged: every test passes against 1.0.0 and
1.0.1 alike.

- **`put` is O(log n), where it was O(n).** It counted the cache's entries
  with `Dict.count`, which visits every one, on every call. The cache now
  keeps its size. 50,000 puts into a cache of 50,000 took 7.6 s on node 22,
  and take 0.12 s.
- **The cache is two Dicts instead of a doubly-linked list held in a Dict.**
  The list's links could, as far as the types knew, name keys that were not
  there, so the code carried six `when` branches for cases marked impossible.
  One was reachable: `newLRUCache 0 |> put "a" 1`. It gave the right answer,
  by accident. Now each key keeps its value and the tick at which it was last
  used, and the keys are kept by tick, the first being the least recently
  used. Every `when` is a case that can happen. The internal module
  `LRUCache.LinkedList` is gone; it was never exposed.
- **A cache whose maximum size is 0 or less holds nothing**, as before, and
  `newLRUCache` now says so.
- **The README says O(log n)**, where it said O(1), which neither version was:
  a Dict is a balanced tree.
- **Tests:** a cache of size 1, of size 0 and of negative size, re-putting a
  key when full, a `get` of a missing key, and a reference model run at sizes
  1, 2, 4 and 10 under three mixes of operations, checked after every step.
  The tests' project builds with Gren 0.6.6.

## 1.0.0 — 2025-12-01

First release. An LRU cache for key/value pairs, ported to Gren from
[naddeoa/quick-cache](http://package.elm-lang.org/packages/naddeoa/quick-cache/latest)
for Elm: `newLRUCache`, `put`, `get`, `remove` and `toDict`. Any
`comparable` type can be a key, where quick-cache took only `String`s.
