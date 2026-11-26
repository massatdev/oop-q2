# Q051 — Your answer

## (d) HashMap order vs Python dict order

TODO(Q051): explain why printing `map.keySet()` from a `HashMap` may come out in a
different order on another machine or Java version, and what the Python `dict`
guarantees instead (and whether that guarantee is alphabetical).


Answer:

>Python `dict` guarantees that iteration follows insertion order, keys are returned in the order that they are insterted, it doesn't guarantee alphabetical order. Different than `map.keySet()` because it's different _(somehow)_ from each version and machine.
