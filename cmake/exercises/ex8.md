# Exercise 8: Generator Expressions (Advanced)

**Task**: Use generator expressions to set different compiler flags and definitions for Debug and Release builds.

**Requirements**:
- Add `-DDEBUG_MODE` definition only for Debug builds
- Add `-DRELEASE_MODE` definition only for Release builds
- Add `-O3` optimization only for Release builds (on non-MSVC compilers)
- Print different messages in your code based on the build type

<details>
<summary>Solution</summary>

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.15)
project(GeneratorExpressions)

add_executable(app main.cpp)

# Add compile definitions based on configuration
target_compile_definitions(app PRIVATE
    $<$<CONFIG:Debug>:DEBUG_MODE>
    $<$<CONFIG:Release>:RELEASE_MODE>
)

# Add optimization flags for Release builds (non-MSVC)
target_compile_options(app PRIVATE
    $<$<AND:$<CONFIG:Release>,$<CXX_COMPILER_ID:GNU,Clang,AppleClang>>:-O3>
)

# Alternative: Set properties based on build type
set_target_properties(app PROPERTIES
    CXX_STANDARD 17
    CXX_STANDARD_REQUIRED ON
)
```

**main.cpp**:
```cpp
#include <iostream>

int main() {
    std::cout << "Application started" << std::endl;
    
#ifdef DEBUG_MODE
    std::cout << "[DEBUG BUILD]" << std::endl;
    std::cout << "Debug information enabled" << std::endl;
    std::cout << "Extra checks enabled" << std::endl;
#endif

#ifdef RELEASE_MODE
    std::cout << "[RELEASE BUILD]" << std::endl;
    std::cout << "Optimizations enabled" << std::endl;
#endif

    return 0;
}
```

**Build commands**:
```bash
# Debug build
cmake -DCMAKE_BUILD_TYPE=Debug ..
cmake --build .

# Release build
cmake -DCMAKE_BUILD_TYPE=Release ..
cmake --build .
```

</details>
