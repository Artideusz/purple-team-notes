# CMake

## What is CMake?

CMake is a family of tools allowing us to compile programs, automation of the compilation process and simple management of project delivery.

## Commands and syntax

There are 3 required commands:

- `add_executable(<executable_name> ...<source_files>)` - Creation of a new executable file.
- `project(<project_name>)` - Name of the project.
- `cmake_minimum_required()` - The minimal CMake version required to run the project.

CMake has other useful commands, like:

- `set(CMAKE_CXX_STANDARD <C/C++ version>)`
    - Recommended version of C/C++.
- `set(CMAKE_CXX_STANDARD_REQUIRED ON)` 
    - Force recommended version of C/C++.
- `include(<CMake_library>)`
    - Include a ...
- `target_link_libraries(existing_executable_name <library>)`
    - Link an external library to the executable.

## How to use CMake?

- `cmake ./<project>` - Setting up CMake in the project folder (CMakeLists.txt MUST exist in the folder).
- `cmake --build ./<path_to_project>` - Building the project (there MUST be "CMakeLists.txt" inside the project directory).

## Resources

- https://en.wikipedia.org/wiki/CMake
- https://cmake.org/cmake/help/latest/guide/tutorial/A%20Basic%20Starting%20Point.html