# Exercise 5: Build Options (Intermediate)

**Task**: Add configurable build options to your project.

**Requirements**:
- Add an option to enable/disable building tests
- Add an option to enable/disable verbose logging
- When verbose logging is enabled, add a preprocessor definition `VERBOSE_LOGGING`
- Create a simple test executable that only builds when tests are enabled

<details>
<summary>Solution</summary>

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.15)
project(ConfigurableProject)

# Build options
option(BUILD_TESTS "Build the test suite" ON)
option(ENABLE_VERBOSE_LOGGING "Enable verbose logging" OFF)

# Main executable
add_executable(app main.cpp)

# Add preprocessor definition if verbose logging is enabled
if(ENABLE_VERBOSE_LOGGING)
    target_compile_definitions(app PRIVATE VERBOSE_LOGGING)
endif()

# Conditionally build tests
if(BUILD_TESTS)
    add_executable(test_app test.cpp)
    message(STATUS "Tests will be built")
else()
    message(STATUS "Tests disabled")
endif()
```

**main.cpp**:
```cpp
#include <iostream>

int main() {
    std::cout << "Application running..." << std::endl;
    
#ifdef VERBOSE_LOGGING
    std::cout << "[VERBOSE] Detailed logging enabled" << std::endl;
    std::cout << "[VERBOSE] Performing operations..." << std::endl;
#endif
    
    std::cout << "Application finished" << std::endl;
    return 0;
}
```

**test.cpp**:
```cpp
#include <iostream>

int main() {
    std::cout << "Running tests..." << std::endl;
    std::cout << "All tests passed!" << std::endl;
    return 0;
}
```

**Build commands**:
```bash
# Build with tests disabled and verbose logging enabled
cmake -DBUILD_TESTS=OFF -DENABLE_VERBOSE_LOGGING=ON ..

# Build with default options (tests ON, verbose OFF)
cmake ..
```

</details>
