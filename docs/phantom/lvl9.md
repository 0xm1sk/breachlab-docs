# Phantom Level 09: Stack Day

## Objective
The goal of this level is to transition from basic buffer overflows to a precision strike. You will learn how to bypass "Silent Locks"—security mitigations that do not necessarily crash your program but prevent the exploit from achieving its goal.

The target is a SUID binary called `kern-tool`, owned by the user `flagkeeper9`. Your mission is to change the permissions of the flag file so it can be read by the current user.

---

## 1. Reconnaissance: The Vulnerability
The binary runs with the effective user ID of `flagkeeper9`. A simple test with a long argument reveals a segmentation fault, hinting at a stack-based buffer overflow.

### The `strcpy` Bug
By analyzing the binary in GDB, we find that the function `check_module` utilizes `strcpy()` to copy user input into a fixed-size buffer on the stack.

```asm
0x0000000000401194 <+30>: call 0x401060 <strcpy@plt>
```

`strcpy` is a "blind" copy; it doesn't check the length of the input and continues copying until it hits a **Null Byte (`0x00`)**. By providing a payload longer than the buffer, we can overwrite the **Saved RIP (Instruction Pointer)** and redirect the CPU to our own code.

### Fundamental Concepts for Beginners
- **RIP (Instruction Pointer):** The CPU's "bookmark." It holds the address of the next instruction to execute. Controlling the RIP means controlling the program.
- **RBP (Base Pointer):** Used to track the current function's stack frame.
- **The Stack:** A region of memory that grows downwards. It stores local variables and the return address (Saved RIP).
- **Smashed Stack:** When an overflow writes past the buffer, it overwrites the Saved RBP and then the Saved RIP.

---

## 2. The Four Silent Locks

### Lock 1: The SUID Trap (Privilege Drop)
**The Problem:** The binary is SUID. If you spawn a shell (`/bin/sh`), modern shells detect that the Effective User ID (EUID) differs from the Real User ID (RUID) and automatically drop privileges. You get a shell, but you still cannot read the flag.

**The Solution:** Avoid interactive shells. Use a direct **Syscall** to the kernel. Specifically, use `chmod` to change the flag file's permissions to `0644` (world-readable).

### Lock 2: The NUL-Byte Constraint
**The Problem:** Because `strcpy` stops at the first `0x00`, any null byte in your shellcode will truncate the payload. Most standard shellcodes use `mov rax, 0`, which contains a null byte.

**The Solution: Assembly Optimization**
Use instructions that produce zero without using the `0x00` byte:
- **`xor rax, rax`**: XORing a register with itself always results in 0.
- **`push` / `pop`**: Use these to move values into registers without using `mov` instructions that contain nulls.

### Lock 3: ASLR (Address Space Layout Randomization)
**The Problem:** ASLR shifts the base address of the stack every time the program runs, making it impossible to hardcode the shellcode's address.

**The Solution: The 12-Bit Secret**
While the "page" address is randomized, the **relative offset** within that page (the last 12 bits) is constant.
- If the address is `0x7ffc12345f90`, the `f90` part stays the same.
- **The Strategy:** Implement a Python loop to brute-force the page address. By keeping the constant offset and guessing the page, you will eventually hit the correct location.

### Lock 4: The UTF-8 Killer
**The Problem:** Binary payloads with bytes above `0x7f` can be corrupted if processed as UTF-8 strings.

**The Solution:** Always handle payloads as **raw bytes** (`b"..."` in Python) and transmit them using `subprocess.run` or `sys.stdout.buffer.write`.

---

## 3. Step-by-Step Execution

### Step 1: Determining the Offset (K=72)
We need to find the exact distance to the Saved RIP. In GDB, run the binary with a long pattern of "A"s:

```bash
gdb /usr/local/bin/kern-tool
(gdb) run AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
```

When it crashes, inspect the registers:
```bash
(gdb) info registers
rbp 0x4141414141414141
```
The buffer is 64 bytes. We must also overwrite the Saved RBP (8 bytes).
**Math:** $64 \text{ (buffer)} + 8 \text{ (Saved RBP)} = 72 \text{ bytes}$.
Any payload longer than 72 bytes will overwrite the return address.

### Step 2: Crafting the Shellcode
Your shellcode must be exactly 64 bytes, NUL-free, and perform the following:
1. Set the `chmod` syscall number in `rax` (Syscall 90).
2. Put the address of the flag file in `rdi`.
3. Set the mode to `0644` (0x1a4) in `rsi`.
4. Trigger the `syscall` instruction.

**The Role of NOPs:**
A `\x90` byte is a **NOP (No-Operation)**. The CPU ignores it and moves to the next instruction. Use NOPs as padding if your shellcode is shorter than 64 bytes to ensure the target address lands perfectly on the RIP.

### Step 3: Implementing the ASLR Brute Force
Since you cannot disable ASLR, you must automate the guessing process.

**Finding the Offset:**
Break GDB just before the `strcpy` call and read the buffer address:
```bash
(gdb) break *check_module+30
(gdb) run TEST
(gdb) x/gx $rbp-0x40
# Example output: 0x7fffffffe990 (Offset is 0x990)
```

**The Brute-Force Framework:**
Use a Python loop to iterate through possible page addresses until the flag becomes readable.

```python
# Framework Logic
while True:
    guessed_page = random_page_base() 
    target_addr = guessed_page | OFFSET_LOW12
    
    # Construct: [64-byte Shellcode] + [8-byte Padding] + [8-byte Address]
    payload = shellcode + padding + target_addr_bytes
    
    run_binary(payload)
    
    if os.access(FLAG_PATH, os.R_OK):
        print("Success! Flag is now readable.")
        break
```

---

## 🏁 Final Summary Table

| Mitigation | Technical Problem | Engineering Solution |
| :--- | :--- | :--- |
| **Symmetry** | `strcpy` lacks boundary checks | Offset calculated at 72 bytes |
| **Privileges** | SUID shell privilege drop | Use direct `chmod` syscall |
| **Null Bytes** | `strcpy` truncation at `0x00` | Use `xor` and `push/pop` assembly |
| **Randomization** | ASLR shifts stack base | Brute-force page with constant 12-bit offset |
| **Encoding** | UTF-8 corruption of binary data | Use raw byte objects (`b""`) |

**Logic over luck. This is the foundation of professional binary exploitation.**
