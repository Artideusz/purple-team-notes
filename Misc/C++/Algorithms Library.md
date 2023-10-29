# Algorithms library

## What is the algorithm library

The `<algorithm>` library consists of many different algorithms, such as:

- `sort`
- `unique`
- `find_element`
- And many more!

## How to use it?

The `<algorithm>` library was introduced in C++11, but we will use functionality introduced in C++20, such as `std::ranges::<algorithm>`.

We can use algorithms from this library by `#include <algorithm>`:

```c++
#include <algorithm>
#include <vector>

auto main() -> int {
    auto v = std::ranges::vector<int>{2,5,4,1,7,3,7,8}; // Talked about in `Containers.md`
    fmt::println("Before sort: {}", v); // [2,5,4,1,7,3,7,8]
    std::ranges::sort(v); // In-place sort
    fmt::println("After sort: {}", v); //  [1,2,3,4,5,7,7,8]
};
```

## `std::ranges::sort`

## `std::ranges::unique`

## `std::ranges::erase`

## `std::ranges::reverse`

## `std::ranges::swap`

## `std::ranges::min` and `std::ranges::min_element`

## `std::ranges::max` and `std::ranges::max_element`

## `std::ranges::iter_swap`

## `std::ranges::distance`

## `std::ranges::find`

## `std::ranges::find_if`

## `std::ranges::partition`

## `std::ranges::transform`


## Resources

- https://en.cppreference.com/w/cpp/algorithm
