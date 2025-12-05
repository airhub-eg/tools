# CMake

CMake is a cross-platform, open-source build system generator that has become the de facto standard for C and C++ projects. Rather than building your code directly, CMake generates native build files (like Makefiles or Visual Studio projects) that are then used to compile your project.

## Core Concepts

### What CMake Does

CMake sits between your source code and the actual build tools. You write configuration files (`CMakeLists.txt`) that describe your project, and CMake translates these into the appropriate build system for your platform. This means you can maintain a single set of build instructions that work on Linux, Windows, macOS, and other platforms.

### The Build Process

The typical CMake workflow involves two distinct phases:

**Configuration Phase**: CMake reads your `CMakeLists.txt` files, processes the instructions, detects your compiler and system capabilities, and generates build files appropriate for your environment.

**Build Phase**: You use the generated build system (Make, Ninja, Visual Studio, etc.) to actually compile your code.

This separation allows CMake to handle platform differences while letting specialized build tools do what they do best.

## CMakeLists.txt Structure

Every CMake project starts with a `CMakeLists.txt` file in the root directory. At minimum, you need to specify the CMake version requirement and your project name:

```cmake
cmake_minimum_required(VERSION 3.15)
project(MyProject VERSION 1.0.0 LANGUAGES CXX)
```

The `cmake_minimum_required` command ensures users have a compatible CMake version. The `project` command defines your project name, optionally specifying version numbers and which programming languages you're using.

## Targets: The Heart of Modern CMake

Modern CMake revolves around the concept of **targets**. A target represents something to be built (an executable, library) or a logical grouping of build settings.

### Creating Targets

You create executable targets with `add_executable`:

```cmake
add_executable(myapp main.cpp utils.cpp)
```

Libraries are created with `add_library`:

```cmake
add_library(mylib STATIC library.cpp helper.cpp)
# or SHARED for shared/dynamic libraries
# or INTERFACE for header-only libraries
```

### Target Properties

Each target has properties that control how it's built. Rather than setting global variables that affect everything, modern CMake attaches settings directly to targets. This is done with commands like `target_include_directories`, `target_compile_features`, `target_link_libraries`, and `target_compile_options`.

```cmake
target_include_directories(myapp PRIVATE include/)
target_compile_features(myapp PRIVATE cxx_std_17)
target_link_libraries(myapp PRIVATE mylib)
```

The keywords `PRIVATE`, `PUBLIC`, and `INTERFACE` control how these properties propagate:

- **PRIVATE**: Used only when building this target
- **PUBLIC**: Used when building this target AND by anything that links to it
- **INTERFACE**: Not used by this target, but used by anything that links to it

This propagation system is what makes CMake's dependency management powerful. When you link against a library, you automatically inherit its public include directories and compile options.

## Variables and Cache

CMake has two types of variables:

**Normal variables** are like local variables in programming, scoped to the current `CMakeLists.txt` file and any subdirectories. You set them with `set()`:

```cmake
set(SOURCES main.cpp utils.cpp helper.cpp)
add_executable(myapp ${SOURCES})
```

**Cache variables** persist between CMake runs and can be set from the command line. These are useful for configuration options:

```cmake
option(BUILD_TESTS "Build test suite" ON)
set(CUSTOM_PATH "/usr/local" CACHE PATH "Custom installation path")
```

Users can override cache variables when configuring:

```bash
cmake -DBUILD_TESTS=OFF -DCUSTOM_PATH=/opt/local ..
```

## Finding and Using Dependencies

CMake provides mechanisms to find and use external libraries. The `find_package` command is the standard way to locate dependencies:

```cmake
find_package(Boost 1.70 REQUIRED COMPONENTS filesystem system)
target_link_libraries(myapp PRIVATE Boost::filesystem Boost::system)
```

For this to work, CMake needs to find a "find module" or "package configuration file" that describes where the package is located. Many popular libraries provide CMake support files that make this seamless.

If a library doesn't have CMake support, you can use `find_library` and `find_path` to locate it manually, or write your own find module.

## Generator Expressions

Generator expressions are evaluated during build system generation (not during configuration) and allow conditional logic based on build configuration. They use the syntax `$<...>`:

```cmake
target_compile_definitions(myapp PRIVATE 
    $<$<CONFIG:Debug>:DEBUG_BUILD>
    $<$<CONFIG:Release>:NDEBUG>
)
```

This adds different preprocessor definitions depending on whether you're building Debug or Release. Generator expressions are powerful for handling multi-configuration generators like Visual Studio or Xcode.

## Subdirectories and Project Organization

Large projects are typically split across multiple directories, each with its own `CMakeLists.txt`:

```cmake
# Root CMakeLists.txt
cmake_minimum_required(VERSION 3.15)
project(MyProject)

add_subdirectory(src)
add_subdirectory(tests)
add_subdirectory(external)
```

Each subdirectory's `CMakeLists.txt` is processed in order, creating a hierarchical build structure. Variables and targets from parent directories are visible to children, but not vice versa (unless explicitly exported).

## Building Out-of-Source

CMake strongly encourages out-of-source builds, where generated files are placed in a separate directory from your source code:

```bash
mkdir build
cd build
cmake ..
cmake --build .
```

This keeps your source tree clean and allows you to maintain multiple build configurations simultaneously (like separate debug and release build directories).

## Installation

CMake handles installation through the `install` command, which copies built targets and other files to appropriate system locations:

```cmake
install(TARGETS myapp mylib
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
)

install(FILES include/mylib.h DESTINATION include)
```

After building, users run:

```bash
cmake --install . --prefix /usr/local
```

## Advanced Features

### Custom Commands and Targets

You can run arbitrary commands during the build with `add_custom_command` and create custom build steps with `add_custom_target`. This is useful for code generation, running tests, or other non-compilation tasks.

### Testing with CTest

CMake includes CTest, a testing framework. You enable it with `enable_testing()` and add tests with `add_test()`:

```cmake
enable_testing()
add_test(NAME mytest COMMAND mytest_executable)
```

### Packaging with CPack

CPack generates installers and packages for various platforms (DEB, RPM, ZIP, etc.), configured through your CMake files.

### FetchContent and ExternalProject

For managing dependencies directly from source, `FetchContent` (modern) and `ExternalProject` (older but more flexible) allow you to download and build dependencies as part of your build process.

## Imp.

Modern CMake emphasizes target-based design over directory-based variables. Always prefer `target_*` commands over their global equivalents. Avoid modifying global CMAKE variables like `CMAKE_CXX_FLAGS` when target properties will suffice.

Keep your `CMakeLists.txt` files simple and declarative. They describe what to build, not how to build it—let CMake handle platform-specific details.

Use `find_package` and imported targets rather than manually specifying library paths and flags. This makes your build system more portable and maintainable.

Version your `cmake_minimum_required` appropriately, using features available in that version to avoid surprising behavior.

## Common Commands Reference

- `add_executable()`, `add_library()`: Create build targets
- `target_link_libraries()`: Link libraries to targets
- `target_include_directories()`: Add include paths
- `target_compile_features()`: Require language features
- `target_compile_definitions()`: Add preprocessor definitions
- `find_package()`: Locate external dependencies
- `set()`, `option()`: Define variables
- `message()`: Print messages during configuration
- `if()`, `foreach()`, `while()`: Control flow
- `function()`, `macro()`: Define reusable components
