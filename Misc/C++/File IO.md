# File IO

## What is File IO?

File I/O is a way to read and write data to a file. We can do it in C++ using the `<fstream>` library.

## Usage of `<fstream>`

You can use the library by `#include <fstream>`. The library contains a function in the `std` namespace (More in [here](./Namespaces.md)). 


## Reading a file

Here is an exampe program:

```txt
Hello world!
This
is
a
file!
```

```cpp
#include <fstream>
#include <fmt/core.h>

auto main() -> int {
    auto file_handler = std::fstream("data.txt");
    auto word = std::string();
    file_handler >> word;
    fmt::println("{}", word); // "Hello"
    file_handler >> word;
    fmt::println("{}", word); // "world!"
}
```

The above example shows that the `>>` operator assignes characters seperated by whitespace to the variable on the right side of the operator. This kind of operation also knows where it ended, allowing us to read the whole file using only the `>>` operator.

```cpp
#include <fstream>
#include <fmt/core.h>

auto main() -> int {
    auto file_handler = std::fstream("data.txt");
    auto word = std::string();
    while (file_handler >> word) {
        fmt::print("{} ", word);
    }
    fmt::println("");
}
```

The above example reads the whole file and seperates every word using a space. But what if we want to preserve newlines and whitespaces? 

```cpp
#include <fstream>
#include <fmt/core.h>

auto main() -> int {
    auto file_handler = std::fstream("data.txt");
    auto word = std::string();
    while (std::getline(file_handler, word)) { 
        // std::getline returns all characters until a newline appears.
        fmt::println("{}", word);
    }
}
```

## Writing to a file

Writing to a file is simple. Here's an example without `fmt`:

```cpp
#include <fstream>

auto main() -> int {
    auto fh = std::fstream("new_data.txt");
    fh << "Hello"; // This only works if the file already exists
}
```

The reason the above code works only when the specified file is present is because by default, the `std::fstream` function uses the following openmode bits:

- `std::ios::in` - "Read file".
- `std::ios::out` - "Write file".

If `std::ios::in` is set, it will first try to open and read the file, but if it does not exist, it will not create a new file. In order to create a file, you provide only the `std::ios::out` bit, optionally `std::ios::app` in order to append data, rather than removing the old content:

```cpp
#include <fstream>

auto main() -> int {
    auto fh = std::fstream("non_existent_file.txt", std::ios::out | std::ios::app); // Write file + Append to end of file
    fh << "Hello! ";
}
```

Here is a useful table of openmode behaviours:

| Openmode           | File does not exist | File exists                             | Initial cursor position |
| :---:              | :---:               | :---:                                   | :---:                   |
| in                 | Nothing             | Opens file, does not clear file upon open | 0                       |
| out                | New file             | Opens file, clears file upon open         | 0                       |
| app                | New file             | Opens file, does not clear file upon open | N                       |
| in + out (default) | Nothing             | Opens file, does not clear file upon open | 0                       |
| out + app          | New file             | Opens file, does not clear file upon open | N                       |
| in + out + app     | New file             | Opens file, does not clear file upon open | 0                       |
| in + app           | New file             | Opens file, does not clear file upon open | 0                       |

*An interesting thing I've discovered is that the pointer cursor for `fstream` seems to be the same in both the read pointer and the write pointer.*

## Read and writing

TO read and write to a file at the same time, you can use the `clear` function to switch between operations:

```cpp
#include <fstream>
#include <fmt/ostream.h>

auto main() -> int {
    auto fh = std::fstream("file.txt", std::ios::in | std::ios::app ); // File will be automatically open if is does not exist
    fmt::print(fh, "Hello world! ");
    fh.clear(); // Clears the error flags, will be described later
    // The pointer should point to the last
}
```

## Resources

- https://stackoverflow.com/questions/15670359/fstream-seekg-seekp-and-write