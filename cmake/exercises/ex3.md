# Exercise 3: Static Library
**Task**: Create a static library and link it to an executable.

**Structure**:
```
project/
├── CMakeLists.txt
├── app/
│   └── main.cpp
└── lib/
    ├── string_utils.h
    └── string_utils.cpp
```

**Requirements**:
- Create a static library called "stringutils" from the lib/ directory
- The library should have functions to uppercase and lowercase strings
- Link the library to an executable named "app"
- Use proper include directories

<details>
<summary>Solution</summary>

**lib/string_utils.h**:
```cpp
#ifndef STRING_UTILS_H
#define STRING_UTILS_H

#include <string>

std::string to_upper(const std::string& str);
std::string to_lower(const std::string& str);

#endif
```

**lib/string_utils.cpp**:
```cpp
#include "string_utils.h"
#include <algorithm>
#include <cctype>

std::string to_upper(const std::string& str) {
    std::string result = str;
    std::transform(result.begin(), result.end(), result.begin(),
                   [](unsigned char c) { return std::toupper(c); });
    return result;
}

std::string to_lower(const std::string& str) {
    std::string result = str;
    std::transform(result.begin(), result.end(), result.begin(),
                   [](unsigned char c) { return std::tolower(c); });
    return result;
}
```

**app/main.cpp**:
```cpp
#include <iostream>
#include "string_utils.h"

int main() {
    std::string text = "Hello CMake";
    std::cout << "Original: " << text << std::endl;
    std::cout << "Uppercase: " << to_upper(text) << std::endl;
    std::cout << "Lowercase: " << to_lower(text) << std::endl;
    return 0;
}
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.15)
project(StringUtils VERSION 1.0.0)

# Create the static library
add_library(stringutils STATIC
    lib/string_utils.cpp
)

# Set include directories for the library
target_include_directories(stringutils PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/lib
)

# Create the executable
add_executable(app app/main.cpp)

# Link the library to the executable
target_link_libraries(app PRIVATE stringutils)
```

</details>