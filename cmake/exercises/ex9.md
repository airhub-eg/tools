# Exercise 9: Installation Rules (Advanced)

**Task**: Create installation rules for your project.

**Requirements**:
- Install the executable to `bin/`
- Install a library to `lib/`
- Install header files to `include/myproject/`
- Create a simple config file and install it to `etc/`

<details>
<summary>Solution</summary>

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.15)
project(InstallableProject VERSION 1.2.3)

# Create library
add_library(mylib STATIC
    src/mylib.cpp
)

target_include_directories(mylib PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

# Create executable
add_executable(myapp src/main.cpp)
target_link_libraries(myapp PRIVATE mylib)

# Installation rules
install(TARGETS myapp mylib
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
)

# Install headers
install(FILES include/mylib.h
    DESTINATION include/myproject
)

# Install config file
install(FILES config/app.conf
    DESTINATION etc/myproject
)

# Install license and readme
install(FILES LICENSE README.md
    DESTINATION share/doc/myproject
)
```

**Directory structure**:
```
project/
├── CMakeLists.txt
├── include/
│   └── mylib.h
├── src/
│   ├── mylib.cpp
│   └── main.cpp
├── config/
│   └── app.conf
├── LICENSE
└── README.md
```

**include/mylib.h**:
```cpp
#ifndef MYLIB_H
#define MYLIB_H

void greet();

#endif
```

**src/mylib.cpp**:
```cpp
#include "mylib.h"
#include <iostream>

void greet() {
    std::cout << "Hello from mylib!" << std::endl;
}
```

**src/main.cpp**:
```cpp
#include "mylib.h"

int main() {
    greet();
    return 0;
}
```

**config/app.conf**:
```
# Application configuration
version=1.2.3
debug=false
```

**Build and install**:
```bash
mkdir build && cd build
cmake -DCMAKE_INSTALL_PREFIX=/usr/local ..
cmake --build .
sudo cmake --install .

# Or install to custom location
cmake --install . --prefix ~/myinstall
```

</details>
