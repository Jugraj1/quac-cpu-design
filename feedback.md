---
mark: 87

section_marks:
  part1: 30
  part2: 35
  report: 22
---

## Part 1
Good work

## Part 2
Doing signed 8 bit numbers means having to analyse situations in which it would make no sense on the programmer side to get a result
(251 * 2 = -10 makes no sense in a 16 bit architecture unless this is known)

## Report
Analysis of flag register not very relevant to HD submission

Handling of negative numbers not clearly explained. Negative when 8 or 16 bits?

Analysis of putting the quotient/remainder result in a single register?

Good analysis of critical path for multiplier. Not quite mentioned for division (uses 8 adders so 8x slow down in clock speed)?



