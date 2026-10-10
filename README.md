A simple, least recently used (LRU) cache with O(log n) get, put and remove operations.
As with Dicts, the key can be of any type that meets the "comparable" constraint.
The value can be of any type.

An LRU cache is one that has a fixed maximum size and evicts entries
when the size gets over that limit. It will always get rid of the least recently used
entry. An entry is "used" when it is retrieved ("get") or updated ("put").

```elm
myCache =
    LRUCache.newLRUCache 3
        |> LRUCache.put "a" 1
        |> LRUCache.put "b" 2
        |> LRUCache.put "c" 3
        |> LRUCache.put "d" 4

-- "a" was evicted
LRUCache.get "a" => {cache: newCacheState, value: Nothing}
LRUCache.get "b" => {cache: newCacheState, value: Just 2}
LRUCache.get "c" => {cache: newCacheState, value: Just 3}
LRUCache.get "d" => {cache: newCacheState, value: Just 4}

-- Getting "b" makes it more recently used than "c" and "d"
LRUCache.get "b" => {cache: newCacheState, value: Just 2}

-- Inserting "e" evicts "c"
LRUCache.put "e" 5 => myCache => newCacheState
LRUCache.get "c" => {cache: newCacheState, value: Nothing}
```

## History

The basis of this code was this excellent Elm package:

http://package.elm-lang.org/packages/naddeoa/quick-cache/latest

Besides the conversion to Gren, the code was also modified to
allow more than just Strings to be keys. Now, any comparable type
can be a key.

The original kept a doubly-linked list inside a Dict, whose links could, as
far as the types knew, name keys that were not there, so the code carried
several `when` branches for cases that could never happen. The cache is now
two Dicts: each key with its value and the tick at which it was last used,
and the keys by tick, whose first entry is the least recently used. Every
`when` in it is a case that can happen.
