# Fuzzing

## What is fuzzing?

Fuzz testing, otherwise known as "Fuzzing", is a testing technique which is based on providing a large number of invalid, malformed or unexpected inputs into an application, potentially causing it to behave differently or even crash. It is used in:

- Development.
- Cybersecurity.

Fuzzing detects bugs such as:

- Memory leaks.
- Buffer overflows.
- Data races.
- Out of bound reads and writes.
- Use-after-free operations.
- And more.

Most of the above can (not really) be exploited by bad actors, which makes fuzzing a great tool for security engineers and bug bounty hunters to find these kind of bugs.

## What targets should be fuzzed?

A good fuzz target would be:

- Should not exit the program on provided input.
- Should not have additional threads running upon exiting the function.
- Must be as deterministic as possible, meaning minimal or no random output upon using the same input.
- Must be fast and CPU and memory efficient.
- The narrower the target, the better.
- Which doesn't use I/O (std io, disk writes and reads)

To create a fuzz target, you have to create the following function:

```c++
extern "C" int LLVMFuzzerTestOneInput(const char* data, size_t data_size) {
    somethingToTest(data, data_size);
    return 0;
}
```

## Fuzzing tools

### LibFuzzer

LibFuzzer is a builtin tool supported by LLVM and compilers derived from LLVM, such as clang. To include the fuzzer into compilation, you need to use the `-fsanitize=fuzzer` flag. Here is an example:

Consider the following code:

```c++
#include <cstdio>

bool vuln_thing(const char* data, size_t data_size) {
    char result[1024];
    result[1023] = '\0';
    for (int i = 0; i < data_size; i++) {
        result[i] = data[i];
    }
    return 0;
}

extern "C" int LLVMFuzzerTestOneInput(const char *data, size_t data_size) {
    vuln_thing(data, data_size);
    return 0;
}
```

If we compile it with a fuzzer and ASan and run it: 

```bash
$ clang++ -g -fsanitize=fuzzer,address -o test test.cpp && ./test

INFO: Running with entropic power schedule (0xFF, 100).
INFO: Seed: 1934023073
INFO: Loaded 1 modules   (4 inline 8-bit counters): 4 [0x556e793c6ba8, 0x556e793c6bac), 
INFO: Loaded 1 PC tables (4 PCs): 4 [0x556e793c6bb0,0x556e793c6bf0), 
INFO: -max_len is not provided; libFuzzer will not generate inputs larger than 4096 bytes
INFO: A corpus is not provided, starting from an empty corpus
#2      INITED cov: 4 ft: 4 corp: 1/1b exec/s: 0 rss: 34Mb
#3      NEW    cov: 4 ft: 5 corp: 2/3b lim: 4 exec/s: 0 rss: 35Mb L: 2/2 MS: 1 CrossOver-
#12     NEW    cov: 4 ft: 6 corp: 3/7b lim: 4 exec/s: 0 rss: 35Mb L: 4/4 MS: 4 ChangeBinInt-ChangeByte-CrossOver-CopyPart-
#25     NEW    cov: 4 ft: 7 corp: 4/10b lim: 4 exec/s: 0 rss: 35Mb L: 3/4 MS: 3 ChangeByte-ChangeBit-InsertByte-
#440    NEW    cov: 4 ft: 8 corp: 5/18b lim: 8 exec/s: 0 rss: 35Mb L: 8/8 MS: 5 ChangeBit-CrossOver-InsertByte-EraseBytes-InsertRepeatedBytes-
#1369   NEW    cov: 4 ft: 9 corp: 6/35b lim: 17 exec/s: 0 rss: 35Mb L: 17/17 MS: 4 InsertRepeatedBytes-ChangeByte-ShuffleBytes-InsertRepeatedBytes-
#1599   REDUCE cov: 4 ft: 9 corp: 6/34b lim: 17 exec/s: 0 rss: 35Mb L: 16/16 MS: 5 ChangeByte-ChangeByte-ChangeBit-EraseBytes-InsertRepeatedBytes-
#3269   NEW    cov: 4 ft: 10 corp: 7/67b lim: 33 exec/s: 0 rss: 35Mb L: 33/33 MS: 5 CopyPart-ChangeBinInt-InsertRepeatedBytes-ChangeBinInt-CMP- DE: "\377\377"-
#3388   REDUCE cov: 4 ft: 10 corp: 7/66b lim: 33 exec/s: 0 rss: 35Mb L: 32/32 MS: 4 ChangeBit-ChangeBit-ShuffleBytes-EraseBytes-
#13193  NEW    cov: 4 ft: 11 corp: 8/194b lim: 128 exec/s: 0 rss: 36Mb L: 128/128 MS: 5 CopyPart-CopyPart-InsertRepeatedBytes-PersAutoDict-InsertRepeatedBytes- DE: "\377\377"-
=================================================================
==17169==ERROR: AddressSanitizer: stack-buffer-overflow on address 0x7ff090c3d420 at pc 0x556e79380a5c bp 0x7ffcefcaca10 sp 0x7ffcefcaca08
WRITE of size 1 at 0x7ff090c3d420 thread T0
    #0 0x556e79380a5b in vuln_thing(char const*, unsigned long) /home/hydrogen/projects/Fuzzing/Fuzzers/test.cpp:7:19
    #1 0x556e79380b84 in LLVMFuzzerTestOneInput /home/hydrogen/projects/Fuzzing/Fuzzers/test.cpp:13:5
    #2 0x556e792246f8 in fuzzer::Fuzzer::ExecuteCallback(unsigned char const*, unsigned long) (/home/hydrogen/projects/Fuzzing/Fuzzers/test+0x556f8) (BuildId: 9cfb0e9827f4c74126fd14fd8484ce22cab7cb92)
    #3 0x556e792253d0 in fuzzer::Fuzzer::RunOne(unsigned char const*, unsigned long, bool, fuzzer::InputInfo*, bool, bool*) (/home/hydrogen/projects/Fuzzing/Fuzzers/test+0x563d0) (BuildId: 9cfb0e9827f4c74126fd14fd8484ce22cab7cb92)
    #4 0x556e79226461 in fuzzer::Fuzzer::MutateAndTestOne() (/home/hydrogen/projects/Fuzzing/Fuzzers/test+0x57461) (BuildId: 9cfb0e9827f4c74126fd14fd8484ce22cab7cb92)
    #5 0x556e79227287 in fuzzer::Fuzzer::Loop(std::vector<fuzzer::SizedFile, std::allocator<fuzzer::SizedFile>>&) (/home/hydrogen/projects/Fuzzing/Fuzzers/test+0x58287) (BuildId: 9cfb0e9827f4c74126fd14fd8484ce22cab7cb92)
    #6 0x556e79207772 in fuzzer::FuzzerDriver(int*, char***, int (*)(unsigned char const*, unsigned long)) (/home/hydrogen/projects/Fuzzing/Fuzzers/test+0x38772) (BuildId: 9cfb0e9827f4c74126fd14fd8484ce22cab7cb92)
    #7 0x556e791f14d7 in main (/home/hydrogen/projects/Fuzzing/Fuzzers/test+0x224d7) (BuildId: 9cfb0e9827f4c74126fd14fd8484ce22cab7cb92)
    #8 0x7ff092558ccf  (/usr/lib/libc.so.6+0x27ccf) (BuildId: 023ea16fd6c04ef9cf094507024e6ecdb35e02ca)
    #9 0x7ff092558d89 in __libc_start_main (/usr/lib/libc.so.6+0x27d89) (BuildId: 023ea16fd6c04ef9cf094507024e6ecdb35e02ca)
    #10 0x556e791f1514 in _start (/home/hydrogen/projects/Fuzzing/Fuzzers/test+0x22514) (BuildId: 9cfb0e9827f4c74126fd14fd8484ce22cab7cb92)

Address 0x7ff090c3d420 is located in stack of thread T0 at offset 1056 in frame
    #0 0x556e793807ff in vuln_thing(char const*, unsigned long) /home/hydrogen/projects/Fuzzing/Fuzzers/test.cpp:3

  This frame has 1 object(s):
    [32, 1056) 'result' (line 4) <== Memory access at offset 1056 overflows this variable
HINT: this may be a false positive if your program uses some custom stack unwind mechanism, swapcontext or vfork
      (longjmp and C++ exceptions *are* supported)
SUMMARY: AddressSanitizer: stack-buffer-overflow /home/hydrogen/projects/Fuzzing/Fuzzers/test.cpp:7:19 in vuln_thing(char const*, unsigned long)
Shadow bytes around the buggy address:
  0x7ff090c3d180: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x7ff090c3d200: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x7ff090c3d280: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x7ff090c3d300: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x7ff090c3d380: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
=>0x7ff090c3d400: 00 00 00 00[f3]f3 f3 f3 f3 f3 f3 f3 f3 f3 f3 f3
  0x7ff090c3d480: f3 f3 f3 f3 00 00 00 00 00 00 00 00 00 00 00 00
  0x7ff090c3d500: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x7ff090c3d580: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x7ff090c3d600: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x7ff090c3d680: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
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
==17169==ABORTING
MS: 5 CopyPart-CopyPart-CopyPart-ChangeByte-CopyPart-; base unit: 643f81c5a8c54d7ecd4494c1d726e2daea3dee2e
artifact_prefix='./'; Test unit written to ./crash-b7fba2808bac1ed52aa88884c99d3875b0b828e4
```

We get a lot of information regarding the bug found by libfuzzer and ASan (AddressSanitizer). We should also get a new file, which should have the input used for the test that caused the crash. The file should by default have the following name format: `crash-<sha1>`, where `sha1` is the `sha1` hash of the input which is in the file. To re-run the crash, you can provide the file as an argument used by the binary, like so: `./test ./crash-b7fba2808bac1ed52aa88884c99d3875b0b828e4`.

- https://github.com/google/honggfuzz/tree/master

## Reading libfuzzer data

Consider the following fragment of a libfuzzer log:

```
Dictionary: 260 entries
INFO: Running with entropic power schedule (0xFF, 100).
INFO: Seed: 1228406848
INFO: Loaded 1 modules   (31571 inline 8-bit counters): 31571 [0x55bd2c85a800, 0x55bd2c862353), 
INFO: Loaded 1 PC tables (31571 PCs): 31571 [0x55bd2c862358,0x55bd2c8dd888), 
INFO:        0 files found in CORPUS-sqlite-2016-11-14-fsanitize_fuzzer
INFO: -max_len is not provided; libFuzzer will not generate inputs larger than 4096 bytes
INFO: A corpus is not provided, starting from an empty corpus
#2	INITED cov: 2 ft: 2 corp: 1/1b exec/s: 0 rss: 37Mb
	NEW_FUNC[1/83]: 0x55bd2c4df020 in sqlite3DeleteTrigger /home/hydrogen/projects/Fuzzing/fuzzer-test-suite/./sqlite-2016-11-14/sqlite3.c:120814
	NEW_FUNC[2/83]: 0x55bd2c4dfec0 in sqlite3DbFree /home/hydrogen/projects/Fuzzing/fuzzer-test-suite/./sqlite-2016-11-14/sqlite3.c:24377
#4	NEW    cov: 563 ft: 564 corp: 2/4b lim: 4 exec/s: 0 rss: 40Mb L: 3/3 MS: 2 CrossOver-InsertByte-
	NEW_FUNC[1/2]: 0x55bd2c538c50 in sqlite3_progress_handler /home/hydrogen/projects/Fuzzing/fuzzer-test-suite/./sqlite-2016-11-14/sqlite3.c:139458
	NEW_FUNC[2/2]: 0x55bd2c62fae0 in keywordCode /home/hydrogen/projects/Fuzzing/fuzzer-test-suite/./sqlite-2016-11-14/sqlite3.c:136811
#8	pulse  cov: 607 ft: 641 corp: 5/14b lim: 4 exec/s: 1 rss: 42Mb
#16	pulse  cov: 625 ft: 663 corp: 13/40b lim: 4 exec/s: 3 rss: 43Mb
#32	pulse  cov: 634 ft: 677 corp: 24/76b lim: 4 exec/s: 6 rss: 44Mb
#60	RELOAD cov: 643 ft: 694 corp: 38/129b lim: 4 exec/s: 12 rss: 47Mb
#64	pulse  cov: 643 ft: 694 corp: 38/129b lim: 4 exec/s: 12 rss: 47Mb
#91	REDUCE cov: 643 ft: 694 corp: 38/128b lim: 4 exec/s: 18 rss: 49Mb L: 3/4 MS: 1 EraseBytes-
#113	NEW    cov: 646 ft: 697 corp: 39/132b lim: 4 exec/s: 22 rss: 51Mb L: 4/4 MS: 2 ChangeByte-ChangeBit-
#120	REDUCE cov: 647 ft: 698 corp: 40/135b lim: 4 exec/s: 24 rss: 51Mb L: 3/4 MS: 2 EraseBytes-InsertByte-
#128	pulse  cov: 647 ft: 698 corp: 40/135b lim: 4 exec/s: 25 rss: 51Mb
#129	NEW    cov: 648 ft: 699 corp: 41/138b lim: 4 exec/s: 25 rss: 51Mb L: 3/4 MS: 4 EraseBytes-ChangeByte-ChangeByte-InsertByte-
#147	NEW    cov: 649 ft: 700 corp: 42/142b lim: 4 exec/s: 29 rss: 52Mb L: 4/4 MS: 3 InsertByte-CrossOver-CrossOver-
#154	NEW    cov: 650 ft: 701 corp: 43/146b lim: 4 exec/s: 30 rss: 53Mb L: 4/4 MS: 2 CrossOver-InsertByte-
#156	NEW    cov: 651 ft: 703 corp: 44/149b lim: 4 exec/s: 31 rss: 53Mb L: 3/4 MS: 2 ChangeByte-ChangeByte-
#186	NEW    cov: 651 ft: 713 corp: 45/153b lim: 4 exec/s: 37 rss: 55Mb L: 4/4 MS: 5 EraseBytes-CopyPart-ChangeBit-ChangeBinInt-ChangeBit-
#213	REDUCE cov: 651 ft: 713 corp: 45/152b lim: 4 exec/s: 42 rss: 57Mb L: 3/4 MS: 2 ChangeByte-EraseBytes-
#224	REDUCE cov: 652 ft: 714 corp: 46/155b lim: 4 exec/s: 44 rss: 57Mb L: 3/4 MS: 1 ChangeBit-
#256	pulse  cov: 652 ft: 714 corp: 46/155b lim: 4 exec/s: 51 rss: 60Mb
```

First, we have `Dictionary: 260 entries`, this is the amount of entries in what's called a `dictionary file`.

### Dictionary files

Dictionary files are files which give fuzzers predefined inputs, which can then be mutated by a mutator.

## Resources

- https://www.synopsys.com/glossary/what-is-fuzz-testing.html
- https://en.wikipedia.org/wiki/Fuzzing
- https://owasp.org/www-community/Fuzzing

- https://www.usenix.org/conference/atc20/presentation/jeon (to read about)
- https://github.com/HexHive/FuZZan

- https://securitylab.github.com/research/fuzzing-sockets-FTP/
- https://github.com/google/fuzzing/blob/master/tutorial/libFuzzerTutorial.md
- https://github.com/google/fuzzing/blob/master/docs/good-fuzz-target.md

- https://llvm.org/docs/LibFuzzer.html