[![make.fun · tiny-stack-vm-01-run-20261122](https://make.fun/badge/tiny-stack-vm-01-run-20261122.svg)](https://make.fun/bounty/tiny-stack-vm-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=tiny-stack-vm-01-run-20261122)
[![rank](https://make.fun/badge/tiny-stack-vm-01-run-20261122/indraHA01/makefun-tiny-stack-vm-01-run-20261122/rank.svg)](https://make.fun/bounty/tiny-stack-vm-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-stack-vm-01-run-20261122)
[![reward](https://make.fun/badge/tiny-stack-vm-01-run-20261122/indraHA01/makefun-tiny-stack-vm-01-run-20261122/reward.svg)](https://make.fun/bounty/tiny-stack-vm-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=tiny-stack-vm-01-run-20261122)

# tiny-stack-vm — a faithful tiny stack VM for make.fun

`main.ts` (Node 22, no dependencies) runs every instance of a set in one run:
it reads `/input/<id>/program.bin` + `input.bin`, executes the program on
make.fun's tiny stack ISA, and writes `/output/<id>/stdout.bin` + `exit.txt`.

It is a line-for-line-faithful port of the kit's reference VM
(`generator/subject/vm.mjs`), so it nails the rules most implementations get
wrong: exact 32-bit wrapping multiplication (`Math.imul`), `INT_MIN / -1`
and `INT_MIN % -1`, unsigned `DIVU`/`MODU`/`LTU`, shift counts masked to 5
bits, `SHR` vs `SHRU`, `PUSH8` sign extension, `EXIT`'s low byte, all ten
traps with their own exit codes (2–11), and the step (1,000,000) and output
(65,536-byte) limits — while a trap keeps the output written before it.

Local test on the practice set: run it with
`MAKEFUN_INPUT_DIR=practice/input MAKEFUN_OUTPUT_DIR=out node main.ts`
and diff `out/` against `practice/reference/` — 40/40 instances match the
reference VM byte for byte, exit codes included.
