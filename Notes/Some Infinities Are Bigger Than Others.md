---
created: 2026-07-19
passion: 4
status: In Progress
tags:
  - note
  - journal
last: 2026-07-25
---
## Explanation
There seem to be multiple ways to think about the separation between two numbers. One is the numerical `difference` obtained by subtraction. Another is the `interval` between them, which contains infinitely many real numbers. Although every `interval` contains infinitely many points, intervals can have different lengths. This raises an interesting question: how can two `intervals` contain the same number of points while one is twice as long as the other?

Difference is what we are taught in math, and that's subtracting one number from the other resulting in the `difference` between them, i.e. `3-2` has a difference of `1`.

The `interval` between two numbers is the set of all real numbers that lie between them. Any `interval`, no matter how short, contains infinitely many real numbers. For example, the `interval` between 1 and 2 contains values such as 1.5, 1.01, 1.000001, and infinitely many others. There is no "next" real number after 1, because between any two distinct real numbers there is always another real number.

If the interval between 1 and 2 contains infinitely many real numbers, then the interval between 1 and 3 does as well. Intuitively, however, the second interval feels as though it should contain "more" because it is twice as long. Standard mathematics says that both intervals contain the same number of real numbers (they have the same cardinality), even though one interval has twice the length. This suggests that "size" is not a single concept when discussing infinity. The length of an interval and the cardinality of the set of points it contains measure fundamentally different things.

## Two Intervals Contain The Same Number Of Points
formula that measures one number to another: `f(x)=2x−1`

**Example Usage:**

| Input (x) | Compute (2x-1) | Output |
| --------: | -------------: | -----: |
|       1.1 |     (2(1.1)-1) |    1.2 |
|      1.25 |    (2(1.25)-1) |    1.5 |
|       1.5 |     (2(1.5)-1) |    2.0 |
|      1.75 |    (2(1.75)-1) |    2.5 |
|       1.9 |     (2(1.9)-1) |    2.8 |

When mapping the points between `1 & 2` and also `1 & 3` we see that point in one interval has exactly one corresponding point in the other:

| (1,2) | (1,3) |
| ----: | ----: |
|   1.1 |   1.2 |
|   1.2 |   1.4 |
|   1.3 |   1.6 |
|   1.4 |   1.8 |
|   1.5 |   2.0 |
There is never a point in `(1,3)`without a partner and there is never a point in `(1,2)` that has two partners.

## Expanding The Interval
Going from `(1,2)` to `(1,3)` does create more room, but it does **not** create more points. Using a rubber band as an example: if you put `x` amount of dots on it, then stretch it to twice its length, you've create more space, but not more dots.

The question is:
> Expansion may not create more points, but if it creates more space, then why can we not add points to it?

