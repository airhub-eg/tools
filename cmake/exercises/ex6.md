# Exercise 6: C++ Standard and Compile Features (Intermediate)

**Task**: Configure your project to use C++17 features and set compiler warnings.

**Requirements**:
- Require C++17 standard
- Enable compiler warnings
- Use a C++17 feature in your code (like structured bindings or std::optional)
- Make the C++ standard requirement propagate to anything that links to your library

<details>
<summary>Solution</summary>

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.15)
project(Cpp17Project)

# Create a library
add_library(datalib STATIC
    data_processor.cpp
)

# Require C++17 for this library (and anything that links to it)
target_compile_features(datalib PUBLIC cxx_std_17)

# Create executable
add_executable(app main.cpp)

# Link the library (C++17 requirement automatically propagates)
target_link_libraries(app PRIVATE datalib)

# Enable warnings
if(MSVC)
    target_compile_options(app PRIVATE /W4)
else()
    target_compile_options(app PRIVATE -Wall -Wextra -Wpedantic)
endif()
```

**data_processor.h**:
```cpp
#ifndef DATA_PROCESSOR_H
#define DATA_PROCESSOR_H

#include <optional>
#include <string>
#include <map>

std::optional<int> find_value(const std::string& key);

#endif
```

**data_processor.cpp**:
```cpp
#include "data_processor.h"

std::optional<int> find_value(const std::string& key) {
    static std::map<std::string, int> data = {
        {"apple", 1},
        {"banana", 2},
        {"cherry", 3}
    };
    
    auto it = data.find(key);
    if (it != data.end()) {
        return it->second;
    }
    return std::nullopt;  // C++17 feature
}
```

**main.cpp**:
```cpp
#include <iostream>
#include "data_processor.h"

int main() {
    // C++17 structured bindings
    auto [key1, key2] = std::make_pair("apple", "orange");
    
    if (auto value = find_value(key1); value.has_value()) {
        std::cout << key1 << " = " << *value << std::endl;
    }
    
    if (auto value = find_value(key2); value.has_value()) {
        std::cout << key2 << " = " << *value << std::endl;
    } else {
        std::cout << key2 << " not found" << std::endl;
    }
    
    return 0;
}
```

</details>
