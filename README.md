# JITLoader
Using the .NET JIT compiler's RWX memory to decrypt and execute shellcode without using APIs or syscalls.

# How it works
.NET uses a just-in-time compiler to compile code in memory, the JIT compiler works in a RWX memory area, this means we can write and execute machinecode directly in the JIT compiler's memory.

Using this to execute shellcode will allow us to bypass API hooking and syscall detection methods, furthermore the JIT compiler's memory is likely ignored by some EDRs, so execution from unbacked RWX memory won't be suspicious. The small amount of code required will also make static detection of JITLoader much harder.

The issue with this is that the JIT memory is often too small, meaning writing larger shellcode to it will usually end up overflowing and writing into protected memory. JITLoader solves this issue by attempting to expand the JIT memory in a newly created method to a size where the shellcode can fit, then copying the shellcode to the expanded memory, and executing it using a function pointer.

As powershell is also .NET, I have included a powershell version of the program as well, both will just execute messagebox shellcode.

# Credits
https://github.com/Mr-Un1k0d3r/DotnetNoVirtualProtectShellcodeLoader - The main idea.

ChatGPT - Help with some of the code.
