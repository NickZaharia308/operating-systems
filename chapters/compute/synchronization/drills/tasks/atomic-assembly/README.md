# Atomic Assembly

No, this section is not about nukes, sadly :(.
Instead, we aim to get accustomed to the way in which the x86 ISA provides atomic instructions.

This mechanism looks very simple.
It is but **one instruction prefix**: `lock`.
It is not an instruction with its own separate opcode, but a prefix that slightly modifies the opcode of the following instructions to make the CPU execute it atomically (i.e. with exclusive access to the data bus).

`lock` must only be place before an instruction that executes a _read-modify-write_ action.
For example, we cannot place it before a `mov` instruction, as the action of a `mov` is simply `read` or `write`.
Instead, we can place it in front of an `inc` instruction if its operand is memory.

Look at the code in `race-condition/support/asm/race_condition_lock.S`.
It's an Assembly equivalent of the code you've already seen many times so far (such as `race-condition/support/c/race_condition.c`).
Assemble and run it a few times.
Notice the different results you get.

Now add the `lock` prefix before `inc` and `dec`.
Reassemble and rerun the code.
And now we have synchronized the two threads by leveraging CPU support.
