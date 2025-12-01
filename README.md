A simple, least recently used (LRU) cache with O(1) get, put and remove operations.
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

LRUCache.get "a" => (newCacheState, Nothing)
LRUCache.get "b" => (newCacheState, Just 2)
LRUCache.get "c" => (newCacheState, Just 3)
LRUCache.get "d" => (newCacheState, Just 4)
```

## History

The basis of this code was this excellent Elm package:

http://package.elm-lang.org/packages/naddeoa/quick-cache/latest

Besides the conversion to Gren, the code was also modified to remove
one of the internal Dicts, leaving just one Dict for each LRUCache.
