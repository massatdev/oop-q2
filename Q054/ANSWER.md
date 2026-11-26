# Q054 — Your answer

## (a) The eight lines

1. `totalKg(shirts)` — **compiles**. `Shirt` extends `Garment`, so the list can be read as a list of garments.
2. `totalKg(anything)` — **does not compile**. `Object` is not a subtype of `Garment`.
3. `addSpareTowels(basket, 2)` — **compiles**. A `List<Garment>` can accept `Towel` objects.
4. `addSpareTowels(bathTowels, 1)` — **does not compile**. A `List<BathTowel>` cannot accept an arbitrary `Towel`.
5. `transfer(bathTowels, towels)` — **compiles**. `BathTowel` is a `Towel`, so the source produces and destination accepts `Towel`.
6. `transfer(towels, shirts)` — **does not compile**. There is no suitable type `T`: the source contains towels, while the destination can accept only shirts.
7. `List<Garment> g = shirts` — **does not compile**. Java generic lists are invariant; `List<Shirt>` is not a subtype of `List<Garment>`.
8. `view.add(new Shirt("x"))` — **does not compile**. `view` could refer to a list of some other `Garment` subtype, so adding a `Shirt` would not be safe.

## (c) The rule

PECS means **Producer Extends, Consumer Super**. Use `? extends T` when a collection produces values to read, and `? super T` when it consumes values to write. In `transfer`, `src` produces `T` values and `dst` consumes them.

## (d) Python

Python expresses read-only covariance with `Sequence[Garment]` and iterable covariance with `Iterable[T]`. Mutable collections such as `List` and `MutableSequence` are invariant, so `add_spare_towels(basket, 2)` and `add_spare_towels(bath_towels, 1)` are rejected when the parameter is `MutableSequence[Towel]`.

For a `? super Towel`-style destination, define a contravariant protocol that only accepts towels, such as `Sink[T_contra]` with an `append` method. Then `add_spare_towels_anywhere(basket, 2)` is accepted because a `List[Garment]` can accept towels.

With the given `transfer` signature, `transfer(bath_towels, towels)` and `transfer(shirts, basket)` are accepted; `transfer(towels, shirts)` and the `Towel`-to-`List[Shirt]` call are rejected by mypy.