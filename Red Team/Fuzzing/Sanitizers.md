# Sanitizers in libfuzzer

## What are sanitizers?

Sanitizers are tools that detect bugs at runtime. They are built into some C/C++ compilers, mainly Clang/LLVM and GCC (as features). They are used for detecting specific bugs that can be difficult to find in your application, such as:

- Memory leaks (LeakSanitizer)
- Heap and stack buffer overflow (AddressSanitizer)
- Data races (ThreadSanitizer)
- Uninitialized memory reads (MemorySanitizer)

## How to use them?

Here is an example on how to use sanitizers:

Consider the following code:

```c++
// out_of_bounds.cpp
#include <iostream>

int main() {

    int a[5] = {0,1,2,3,4};

    std::cout << a[0] << "\n";

    // bug time!

    std::cout << a[10] << "\n";

    return 0;
}
```

Upon compiling:

```
$ g++ -o oob out_of_bounds.cpp
$ 
```

The code was compiled without any errors. When we run it though:
```
$ ./oob
0
21199284
```

We created a random number generator! Just joking :) It is a garbage value pointed by an address on `0x<location of a[0]> + (10 * 4)`
By default, sanitizers are not used - you have to enable them using flags. To detect the above bug, we just compile the source code with an additional flag: `-fsanitize=address`, which enables the `AddressSanitizer` tool. So:

```
$ g++ -g -fsanitize=address -o oob out_of_bounds.cpp
$ 
```

All good, now running the binary:

```
$ ./oob
0
=================================================================
==5545==ERROR: AddressSanitizer: stack-buffer-overflow on address 0x7efc18900048 at pc 0x5568fb1d73e9 bp 0x7fff5b4a7890 sp 0x7fff5b4a7880
READ of size 4 at 0x7efc18900048 thread T0
    #0 0x5568fb1d73e8 in main /home/hydrogen/projects/Fuzzing/Sanitizers/out_of_bounds.cpp:11
    #1 0x7efc1a845ccf  (/usr/lib/libc.so.6+0x27ccf) (BuildId: 023ea16fd6c04ef9cf094507024e6ecdb35e02ca)
    #2 0x7efc1a845d89 in __libc_start_main (/usr/lib/libc.so.6+0x27d89) (BuildId: 023ea16fd6c04ef9cf094507024e6ecdb35e02ca)
    #3 0x5568fb1d70f4 in _start (/home/hydrogen/projects/Fuzzing/Sanitizers/oob+0x10f4) (BuildId: c98bf239db18c5c7212c906e0575ab3899909f96)

Address 0x7efc18900048 is located in stack of thread T0 at offset 72 in frame
    #0 0x5568fb1d71d8 in main /home/hydrogen/projects/Fuzzing/Sanitizers/out_of_bounds.cpp:3

  This frame has 1 object(s):
    [32, 52) 'a' (line 5) <== Memory access at offset 72 overflows this variable
HINT: this may be a false positive if your program uses some custom stack unwind mechanism, swapcontext or vfork
      (longjmp and C++ exceptions *are* supported)
SUMMARY: AddressSanitizer: stack-buffer-overflow /home/hydrogen/projects/Fuzzing/Sanitizers/out_of_bounds.cpp:11 in main
Shadow bytes around the buggy address:
  0x7efc188ffd80: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x7efc188ffe00: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x7efc188ffe80: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x7efc188fff00: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x7efc188fff80: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
=>0x7efc18900000: f1 f1 f1 f1 00 00 04 f3 f3[f3]f3 f3 00 00 00 00
  0x7efc18900080: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x7efc18900100: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x7efc18900180: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x7efc18900200: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x7efc18900280: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
Shadow byte legend (one shadow byte represents 8 application bytes):
  Addressable:           00
  Partially addressable: 01 02 03 04 05 06 07 
  Heap left redzone:       fa
  Freed heap region:       fd
  Stack left redzone:      f1
  Stack mid redzone:       f2
  Stack right redzone:     f3
  Stack after return:      f5
  Stack use after scope:   f8
  Global redzone:          f9
  Global init order:       f6
  Poisoned by user:        f7
  Container overflow:      fc
  Array cookie:            ac
  Intra object redzone:    bb
  ASan internal:           fe
  Left alloca redzone:     ca
  Right alloca redzone:    cb
==5545==ABORTING
```

As we can see, the binary gave us debug information about the bug in our code. This functionality can and is leveraged mainly in fuzz testing.


## Less important, but cool information I collected while learning

- AddressSanitizer will not work if you compile the binary on a linux-hardened kernel (https://github.com/google/sanitizers/issues/820).

## Resources

- https://github.com/google/fuzzing/blob/master/docs/intro-to-fuzzing.md
- https://github.com/google/sanitizers/wiki/
- https://en.wikipedia.org/wiki/Code_sanitizer
- https://hpc-wiki.info/hpc/Compiler_Sanitizers
- https://www.codingninjas.com/studio/library/accessing-an-array-out-of-bounds-in-cc