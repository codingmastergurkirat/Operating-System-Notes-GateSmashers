# Producer–Consumer Problem

## Overview

The producer–consumer problem contains two cooperative processes:

- **Producer:** creates items and places them in a shared buffer.
- **Consumer:** removes and consumes items from that buffer.

They share a circular buffer and an integer `count`. The producer uses `in` to identify the next empty slot, while the consumer uses `out` to identify the next item. For a buffer of size `n = 8`, indexes range from `0` to `7`.

```c
// Producer
while (true) {
    produce_item(item_p);
    while (count == n) ;
    buffer[in] = item_p;
    in = (in + 1) % n;
    count = count + 1;
}

// Consumer
while (true) {
    while (count == 0) ;
    item_c = buffer[out];
    out = (out + 1) % n;
    count = count - 1;
    consume_item(item_c);
}
```

The empty loops represent busy waiting. `count == n` means full and `count == 0` means empty.

## Circular Indexing

`in = (in + 1) % n` and `out = (out + 1) % n` wrap an index from the last slot back to slot `0`.

## Race on `count`

An expression such as `count = count + 1` may involve:

1. Loading `count` into a register.
2. Incrementing the register.
3. Storing the register back to memory.

The decrement has the same multi-step nature. If `count = 3`, the producer may load and calculate `4`, then be pre-empted. The consumer can load the still-visible `3`, calculate `2`, and also be pre-empted. If the producer stores `4` and the consumer later stores `2`, the final value becomes `2`, even though one item was added and one removed, so the correct count remains `3`.

This is a race condition: one update overwrites another.

## Synchronization

The shared buffer and `count` updates must be protected. Techniques named in the lecture include binary semaphores, monitors, and locks. Testing only a convenient execution order is insufficient because another valid interleaving may corrupt the state.

### Key Takeaways

- The producer adds; the consumer removes.
- `in` and `out` implement circular-buffer indexing.
- `count` tracks occupied slots.
- Increment and decrement operations are not necessarily indivisible.
- Unsynchronized updates can make `count` inconsistent with buffer contents.
