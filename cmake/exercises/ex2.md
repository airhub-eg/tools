# Exercise 2: Multiple Source Files

**Task**: Create a project with multiple source files organized properly.

**Structure**:
```
project/
├── CMakeLists.txt
├── main.cpp
├── math_ops.cpp
└── math_ops.h
```

**Requirements**:
- `math_ops.h` should declare functions `add()` and `multiply()`
- `math_ops.cpp` should implement these functions
- `main.cpp` should use these functions
- The executable should be named "calculator"

<details>
<summary>Solution</summary>

**math_ops.h**:
```cpp
#ifndef MATH_OPS_H
#define MATH_OPS_H

int add(int a, int b);
int multiply(int a, int b);

#endif
```

**math_ops.cpp**:
```cpp
#include "math_ops.h"

int add(int a, int b) {
    return a + b;
}

int multiply(int a, int b) {
    return a * b;
}
```

**main.cpp**:
```cpp
#include <iostream>
#include "math_ops.h"

int main() {
    std::cout << "5 + 3 = " << add(5, 3) << std::endl;
    std::cout << "5 * 3 = " << multiply(5, 3) << std::endl;
    return 0;
}
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.15)
project(Calculator)

add_executable(calculator 
    main.cpp 
    math_ops.cpp
)
```

**Alternative using variables**:
```cmake
cmake_minimum_required(VERSION 3.15)
project(Calculator)

set(SOURCES
    main.cpp
    math_ops.cpp
)

add_executable(calculator ${SOURCES})
```

</details>