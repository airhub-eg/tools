# Exercise 4: Subdirectories

**Task**: Organize a project with multiple subdirectories, each with its own CMakeLists.txt.

**Structure**:
```
project/
├── CMakeLists.txt
├── app/
│   ├── CMakeLists.txt
│   └── main.cpp
└── lib/
    ├── CMakeLists.txt
    ├── math/
    │   ├── math_ops.h
    │   └── math_ops.cpp
    └── string/
        ├── string_utils.h
        └── string_utils.cpp
```

**Requirements**:
- Create two separate libraries: "mathlib" and "stringlib"
- Each library should be defined in its own subdirectory
- The main application should link to both libraries

<details>
<summary>Solution</summary>

**Root CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.15)
project(MultiLibProject VERSION 1.0.0)

# Add subdirectories
add_subdirectory(lib)
add_subdirectory(app)
```

**lib/CMakeLists.txt**:
```cmake
# Math library
add_library(mathlib STATIC
    math/math_ops.cpp
)

target_include_directories(mathlib PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/math
)

# String library
add_library(stringlib STATIC
    string/string_utils.cpp
)

target_include_directories(stringlib PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/string
)
```

**app/CMakeLists.txt**:
```cmake
add_executable(app main.cpp)

target_link_libraries(app 
    PRIVATE 
        mathlib 
        stringlib
)
```

**lib/math/math_ops.h**:
```cpp
#ifndef MATH_OPS_H
#define MATH_OPS_H

int add(int a, int b);
int multiply(int a, int b);

#endif
```

**lib/math/math_ops.cpp**:
```cpp
#include "math_ops.h"

int add(int a, int b) { return a + b; }
int multiply(int a, int b) { return a * b; }
```

**lib/string/string_utils.h**:
```cpp
#ifndef STRING_UTILS_H
#define STRING_UTILS_H

#include <string>

std::string to_upper(const std::string& str);

#endif
```

**lib/string/string_utils.cpp**:
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
```

**app/main.cpp**:
```cpp
#include <iostream>
#include "math_ops.h"
#include "string_utils.h"

int main() {
    std::cout << "5 + 3 = " << add(5, 3) << std::endl;
    std::cout << "5 * 3 = " << multiply(5, 3) << std::endl;
    std::cout << "Uppercase: " << to_upper("hello") << std::endl;
    return 0;
}
```

</details>