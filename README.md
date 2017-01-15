# Deterministic Dictionaries for Racket

[![Build Status](https://travis-ci.org/pnwamk/racket-ddict.svg?branch=master)](https://travis-ci.org/pnwamk/racket-ddict)

This library provides a simple dictionary data type that resembles Racket's built in `hash` data structure, except that it provides deterministic (LIFO) ordering for iteration and key/value lists. This library does not have any dependencies aside from `racket/base`.

Functionality is intended to match Racket's built in `hash` in the user API and performance as much as possible, so that `ddict`'s can be a simple drop-in replacement for `hash`'s when determinstic ordering is desired.

In order to provide deterministic ordering of the entries, this data type uses a `hash` along with a linked-list of the keys. This means there is a small overhead on most operations (see `extras/perf.rkt` for some simple  benchmarking), and a `ddict` will use more memory than a raw `hash`. The extra memory usage will be < 2x of an equivalent `hash` if no "remove" operations are performed, and will always be less than 3x regardless. This is accomplished by occassional compaction that occurrs internally which removes keys which are no longer in use. This compaction is triggered automatically when enough removal operations occur (ie. when too many garbage keys are present) and can also be performed upon manually via the `ddict-compact!` procedure (which is a noop if no unused keys are present). Note that `ddict-compact!` operates on the internal state of a `ddict` only and will not alter any user-visible properties of the ddict (mutable or immutable) except of course that `ddict-compact?` will then return `#t` instead of `#f` (if it was fragmented at all before the call to `ddict-compact!`).

# API

The API for these dictionaries is intended to be identical to the `hash` API as much as possible. The following identifiers are provided:

```
ddict?
immutable-ddict?
mutable-ddict?
ddict-equal?
ddict-eqv?
ddict-eq?
ddict
ddicteqv
ddicteq
mutable-ddict
mutable-ddicteqv
mutable-ddicteq
make-ddict
make-ddicteqv
make-ddicteq
make-mutable-ddict
make-mutable-ddicteqv
make-mutable-ddicteq
ddict-set
ddict-set*
ddict-set!
ddict-set*!
ddict-ref
ddict-ref!
ddict-update
ddict-update!
ddict-remove
ddict-remove!
ddict-clear!
ddict-clear
ddict-copy
ddict-copy-clear
ddict-has-key?
ddict-empty?
ddict-compact?
ddict-compact!
ddict-count
ddict-keys
ddict-values
ddict->list
in-ddict
in-immutable-ddict
in-ddict-keys
in-ddict-values
ddict-map
ddict-for-each
for/ddict
for/ddicteqv
for/ddicteq
for*/ddict
for*/ddicteqv
for*/ddicteq
for/mutable-ddict
for/mutable-ddicteqv
for/mutable-ddicteq
for*/mutable-ddict
for*/mutable-ddicteqv
for*/mutable-ddicteq
```
