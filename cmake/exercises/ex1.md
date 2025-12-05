# Exercise 1: Basic Executable

**Task**: Create a CMake project that compiles a simple "Hello World" program.

**Files needed**:
- `main.cpp`: A simple C++ program that prints "Hello, CMake!"
- `CMakeLists.txt`: Your CMake configuration

**Requirements**:
- Use CMake version 3.15 or higher
- Name the project "HelloCMake"
- Create an executable named "hello"

<details>
<summary>Solution</summary>

**main.cpp**:
```cpp
#include <iostream>

int main() {
    std::cout << "Hello, CMake!" << std::endl;
    return 0;
}
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.15)
project(HelloCMake)

add_executable(hello main.cpp)
```

**Build commands**:
```bash
mkdir build
cd build
cmake ..
cmake --build .
./hello  # or .\hello.exe on Windows
```

</details>

