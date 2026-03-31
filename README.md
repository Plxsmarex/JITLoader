# JITLoader
Using the `.NET` **JIT compiler's** `RWX` memory to decrypt and execute shellcode without using **APIs** or **syscalls**.

# How it works
`.NET` uses a **just-in-time** (JIT) compiler to convert `bytecode` to `binary` in memory, however to perform this, the compiler allocates memory with the read write execute (`RWX`) protection, this means we can write and execute our own code directly in the **JIT compiler's** memory.

Using this to execute shellcode will allow us to bypass all forms of **API** and **syscall** monitoring, as we won't need to protect or allocate memory ourselves. Furthermore, the **JIT compiler's** memory is likely ignored by some **EDRs**, so execution from unbacked `RWX` memory probably won't be as suspicious. The small amount of code required will also make static detection of **JITLoader** harder.

The issue with this is that the `RWX` memory is often too small, meaning writing larger shellcode to it will usually end up overflowing and writing into **non-writable** memory. **JITLoader** solves this issue by attempting to expand the **JIT compiler's** memory in a newly created method to a size where the shellcode can fit, then copying the shellcode to the expanded memory, and executing it.

As `powershell` is written in `.NET`, we can perform the same thing. I have included a `powershell` version of the program as well, both will just execute messagebox shellcode.

# Credits
https://github.com/Mr-Un1k0d3r/DotnetNoVirtualProtectShellcodeLoader - The main idea.
