# Complete Project

**Task**: Create a complete project that combines multiple CMake concepts.

**Requirements**:
- Multiple libraries (static and shared)
- Subdirectory structure
- External package dependency (Threads)
- Build options
- Installation rules
- Testing with CTest
- Documentation target that generates docs (simulated)
- C++17 standard

<details>
<summary>Solution Structure</summary>

**Project Structure**:
```
project/
├── CMakeLists.txt
├── README.md
├── LICENSE
├── docs/
│   └── Doxyfile
├── include/
│   └── myproject/
│       ├── core.h
│       └── utils.h
├── src/
│   ├── CMakeLists.txt
│   ├── core/
│   │   └── core.cpp
│   └── utils/
│       └── utils.cpp
├── apps/
│   ├── CMakeLists.txt
│   └── main.cpp
└── tests/
    ├── CMakeLists.txt
    ├── test_core.cpp
    └── test_utils.cpp
```

**Root CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.15)
project(CompleteProject VERSION 2.1.0 LANGUAGES CXX)

# Options
option(BUILD_SHARED_LIBS "Build shared libraries" ON)
option(BUILD_TESTS "Build tests" ON)
option(BUILD_DOCS "Build documentation" OFF)

# C++ Standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Find dependencies
find_package(Threads REQUIRED)

# Include subdirectories
add_subdirectory(src)
add_subdirectory(apps)

if(BUILD_TESTS)
    enable_testing()
    add_subdirectory(tests)
endif()

# Documentation target
if(BUILD_DOCS)
    find_package(Doxygen)
    if(DOXYGEN_FOUND)
        add_custom_target(docs
            COMMAND ${DOXYGEN_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/docs/Doxyfile
            WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
            COMMENT "Generating documentation with Doxygen"
            VERBATIM
        )
    endif()
endif()

# Installation of documentation
install(FILES README.md LICENSE
    DESTINATION share/doc/${PROJECT_NAME}
)
```

**src/CMakeLists.txt**:
```cmake
# Core library
add_library(core
    core/core.cpp
)

target_include_directories(core PUBLIC
    $<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

target_compile_features(core PUBLIC cxx_std_17)
target_link_libraries(core PUBLIC Threads::Threads)

# Utils library
add_library(utils
    utils/utils.cpp
)

target_include_directories(utils PUBLIC
    $<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

target_compile_features(utils PUBLIC cxx_std_17)
target_link_libraries(utils PUBLIC core)

# Installation
install(TARGETS core utils
    EXPORT ${PROJECT_NAME}Targets
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
)

install(DIRECTORY ${CMAKE_SOURCE_DIR}/include/
    DESTINATION include
)
```

**apps/CMakeLists.txt**:
```cmake
add_executable(myapp main.cpp)

target_link_libraries(myapp PRIVATE core utils)

install(TARGETS myapp
    DESTINATION bin
)
```

**tests/CMakeLists.txt**:
```cmake
add_executable(test_core test_core.cpp)
target_link_libraries(test_core PRIVATE core)

add_executable(test_utils test_utils.cpp)
target_link_libraries(test_utils PRIVATE utils)

add_test(NAME CoreTests COMMAND test_core)
add_test(NAME UtilsTests COMMAND test_utils)

set_tests_properties(CoreTests UtilsTests PROPERTIES
    TIMEOUT 10
)
```

</details>
