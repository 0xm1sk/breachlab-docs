# Phantom Level 09: Stack Day

## Objective
The goal of this level is to transition from basic buffer overflows to a precision strike. You will learn how to bypass "Silent Locks"—security mitigations that do not necessarily crash your program but prevent the exploit from achieving its goal.

The target is a SUID binary called `kern-tool`, owned by the user `flagkeeper9`. Your mission is to change the permissions of the flag file so it can be read by the current user.

---

## The Vulnerability: The `strcpy` Bug
The binary utilizes the `strcpy()` function to copy user-supplied arguments into a fixed-size buffer on the stack.

**The Danger:** `strcpy` is a "blind" copy. It does not check if the input fits in the destination buffer; it continues copying until it encounters a **Null Byte (`0x00`)**. By providing an input longer than the buffer, we can overwrite adjacent memory, specifically the **Saved RIP**.

### Fundamental Concepts for Beginners
To understand the exploit, you must understand these architectural components:
- **RIP (Instruction Pointer):** The CPU's "bookmark." It holds the memory address of the next instruction to execute. By overwriting the RIP, we control the execution flow of the program.
- **RBP (Base Pointer):** Used to keep track of the current function's stack frame.
- **The Stack:** A region of memory that grows downwards. When a function is called, it pushes the return address (Saved RIP) and the previous base pointer (Saved RBP) onto the stack.
- **Smashed Stack:** This occurs when a buffer overflow writes past its boundary, overwriting the Saved RBP and eventually the Saved RIP, allowing the attacker to redirect execution.

---

## The Four Silent Locks

### Lock 1: The SUID Trap (Privilege Drop)
**The Problem:** The binary has the SUID bit set to `flagkeeper9`. In many modern Linux environments, if an exploit spawns a shell (`/bin/sh`), the shell detects that the Effective User ID (EUID) differs from the Real User ID (RUID) and automatically drops privileges back to the lower-privileged user. You will have a shell, but you will still lack the permissions to read the flag.

**The Solution:** Avoid spawning an interactive shell. Instead, utilize a **Syscall**. A syscall is a direct request to the Linux kernel. In this case, we use the `chmod` syscall to change the flag file's permissions to `0644` (world-readable), allowing us to read it after the binary exits.

### Lock 2: The NUL-Byte Constraint
**The Problem:** Because `strcpy` stops copying at the first `0x00` byte, any null byte in your shellcode will truncate the payload. Most standard shellcodes use instructions like `mov rax, 0`, which contain null bytes in their opcode.

**The Solution: Assembly Optimization**
We use instructions that result in zero without using the `0x00` byte in the machine code:
- **`xor rax, rax`**: XORing a register with itself always results in 0 and contains no null bytes.
- **`push` / `pop`**: These are used to move values into registers or onto the stack without utilizing `mov` instructions that might contain nulls.

### Lock 3: ASLR (Address Space Layout Randomization)
**The Problem:** ASLR randomly offsets the base address of the stack every time the program is executed. This makes it impossible to hardcode the exact address of your shellcode.

**The Solution: The 12-Bit Offset**
While the "page" address is randomized, the relative offset within that page (the last 12 bits) remains constant across executions.
- Example: In `0x7ffc12345f90`, the `f90` is deterministic.
- Only the `0x7ffc12345` portion is randomized.
- **The Strategy:** We implement a Python loop to brute-force the page address. Given the limited number of possible pages, a high-speed loop can find the correct address in a matter of seconds or minutes.

### Lock 4: The UTF-8 Killer
**The Problem:** Binary payloads often contain bytes above `0x7f`. If passed through standard string-processing functions in Python or shell environments, these bytes may be re-encoded as UTF-8, corrupting the shellcode.

**The Solution:** Treat all payloads as **raw bytes** (`b"..."` in Python). Use `subprocess.run` or `sys.stdout.buffer.write` to ensure the data is transmitted exactly as intended without encoding interference.

---

## Step-by-Step Execution

### Step 1: Determining the Offset (K=72)
We must identify the exact number of bytes needed to reach the Saved RIP.
1. Start the binary in GDB.
2. Feed the program a long pattern of characters.
3. Identify the crash point. The buffer is 64 bytes. We must also overwrite the Saved RBP (8 bytes).
4. **Calculation:** $64 \text{ bytes (buffer)} + 8 \text{ bytes (Saved RBP)} = 72 \text{ bytes}$.
5. **Payload Layout:** `[ 72 bytes of shellcode/padding ]` $\rightarrow$ `[ 8 bytes of Target Address ]`.

### Step 2: Crafting the Shellcode
The shellcode must be exactly 64 bytes and NUL-free. It is designed to:
1. Load the `chmod` syscall number into `rax`.
2. Place the path to the flag file in `rdi`.
3. Set the mode to `0644` in `rsi`.
4. Trigger the `syscall` instruction.

**The Role of NOPs:**
A `\x90` byte is a **NOP (No-Operation)**. The CPU ignores NOPs and moves to the next instruction. We use them as "padding" if the shellcode is shorter than the 64-byte buffer, ensuring the target address is placed exactly where the Saved RIP is expected.

### Step 3: The Brute Force Implementation
Since ASLR cannot be disabled, we automate the address guessing:

```python
# Technical logic for ASLR Brute Force
while True:
    guessed_address = generate_random_page() + constant_offset
    payload = shellcode + padding + guessed_address
    execute_binary(payload)
    if check_file_permissions(FLAG_PATH) == 0644:
        print("Success: Address hit!")
        break
```

---

## Final Summary Table

| Concept | Technical Problem | Engineering Solution |
| :--- | :--- | :--- |
| **Overflow** | `strcpy` lacks boundary checks | Offset calculated at 72 bytes |
| **Privileges** | SUID shell privilege drop | Use direct `chmod` syscall |
| **Null Bytes** | `strcpy` truncation at `0x00` | Use `xor` and `push/pop` assembly |
| **Randomization** | ASLR shifts stack base | Brute-force page with constant 12-bit offset |
| **Encoding** | UTF-8 corruption of binary data | Use raw byte objects (`b""`) |

**Logic over luck. This is the foundation of professional exploitation.**
