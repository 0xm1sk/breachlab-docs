# Phantom Level 09: Stack Day

## Objective

The goal of this level is to transition from basic buffer overflows to a precision strike. You will learn how to bypass "Silent Locks"—security mitigations that do not necessarily crash your program but prevent the exploit from achieving its goal.

The target is a SUID binary called `kern-tool`, owned by the user `flagkeeper9`. Your mission is to modify the permissions of the flag file so it can be read by the current user.

---

## 1. Reconnaissance: The Vulnerability

The binary runs with the effective user ID of `flagkeeper9`. An initial test with a long argument reveals a segmentation fault, signaling a stack-based buffer overflow.

### The `strcpy` Bug

By analyzing the binary in GDB, you can observe that the function `check_module` utilizes `strcpy()` to copy user input into a fixed-size buffer on the stack.

`strcpy` is a "blind" copy; it doesn't check the length of the input and continues copying until it hits a **Null Byte (`0x00`)**. By providing a payload longer than the buffer, you can overwrite the **Saved RIP (Instruction Pointer)** and redirect the CPU to your own code.

### Fundamental Concepts for Beginners

- **RIP (Instruction Pointer):** The CPU's "bookmark." It holds the address of the next instruction to execute. Controlling the RIP means controlling the program flow.

- **RBP (Base Pointer):** Used to track the current function's stack frame.

- **The Stack:** A region of memory that grows downwards. It stores local variables and the return address (Saved RIP).

- **Smashed Stack:** When an overflow writes past the buffer, it overwrites the Saved RBP and then the Saved RIP.

---

## 2. The Four Silent Locks

### Lock 1: The SUID Trap (Privilege Drop)

**The Problem:** The binary is SUID. If you spawn a shell (`/bin/sh`), modern shells detect that the Effective User ID (EUID) differs from the Real User ID (RUID) and automatically drop privileges. You get a shell, but you still cannot read the flag.

**The Solution:** Avoid interactive shells. Instead, use a direct **Syscall** to the kernel. Research the specific syscall used to change file permissions (`chmod`) to make the flag world-readable.

### Lock 2: The NUL-Byte Constraint

**The Problem:** Because `strcpy` stops at the first `0x00`, any null byte in your shellcode will truncate the payload. Standard instructions like `mov rax, 0` contain null bytes in their machine code.

**The Solution: Assembly Optimization**

Use instructions that produce zero without using the `0x00` byte:

- **XORing:** XORing a register with itself (e.g., `xor rax, rax`) results in 0 without using a null byte.

- **Stack Manipulation:** Use `push` and `pop` to move values into registers to avoid `mov` instructions that might contain nulls.

### Lock 3: ASLR (Address Space Layout Randomization)

**The Problem:** ASLR shifts the base address of the stack every time the program runs, making it impossible to hardcode the shellcode's address.

**The Solution: The 12-Bit Offset**

While the "page" address is randomized, the **relative offset** within that page (the last 12 bits) is constant across executions.

- **The Strategy:** Implement a loop that guesses the page address. By combining a guessed page base with the deterministic 12-bit offset, you can eventually hit the correct location of your shellcode.

### Lock 4: The UTF-8 Killer

**The Problem:** Binary payloads with bytes above `0x7f` can be corrupted if processed as UTF-8 strings.

**The Solution:** Always handle payloads as **raw bytes** (`b"..."` in Python) and transmit them using `subprocess.run` or `sys.stdout.buffer.write`.

---

## 3. Step-by-Step Execution

### Step 1: Determining the Offset

You must identify the exact distance from the start of the buffer to the Saved RIP.

1. Start the binary in GDB.
2. Send a long pattern of characters (or a cyclic pattern).
3. Analyze the crash to see where the RIP was overwritten.
4. **Calculation:** Determine the buffer size and account for the Saved RBP. The total offset is the number of bytes needed to reach the return address.

### Step 2: Crafting the Shellcode

Your shellcode must be NUL-free and perform a specific action. Research the `chmod` syscall on x86-64 Linux:

1. Identify the syscall number for `chmod` and place it in `rax`.
2. Place the address of the flag file in `rdi`.
3. Set the target permissions (e.g., `0644`) in `rsi`.
4. Trigger the `syscall` instruction.

**Padding with NOPs:**

Use `\x90` (No-Operation) bytes to fill the remaining space in the buffer. This ensures your payload is the exact length required to reach the RIP.

### Step 3: Implementing the ASLR Brute Force

Since you cannot disable ASLR, you must automate the search for the correct stack address.

**Methodology:**

1. Use GDB to find the constant low 12 bits of the buffer address.
2. Create a Python loop that:
   - Generates a random page address.
   - Combines it with the constant 12-bit offset.
   - Executes the binary with the payload.
   - Checks if the flag file's permissions have changed.
3. Repeat until the `chmod` syscall hits and the flag becomes readable.

---

## Final Summary Table

| Mitigation | Technical Problem | Methodology Solution |
| :--- | :--- | :--- |
| **Overflow** | `strcpy` lacks boundary checks | Calculate distance to Saved RIP |
| **Privileges** | SUID shell privilege drop | Direct `chmod` syscall |
| **Null Bytes** | `strcpy` truncation at `0x00` | Use `xor` and `push/pop` assembly |
| **Randomization** | ASLR shifts stack base | Brute-force page with constant offset |
| **Encoding** | UTF-8 corruption of binary data | Use raw byte objects (`b""`) |

**Logic over luck. This is the foundation of professional binary exploitation.**
