# Exercise 7: Finding External Packages (Advanced)

**Task**: Use `find_package` to locate and use an external library (using Threads as an example since it's universally available).

**Requirements**:
- Use `find_package` to find the Threads library
- Create a simple multi-threaded program
- Make the Threads dependency required
- Handle the case where the package is not found

<details>
<summary>Solution</summary>

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.15)
project(ThreadedApp)

# Find the Threads package (required)
find_package(Threads REQUIRED)

add_executable(app main.cpp)

# Link against the Threads library
target_link_libraries(app PRIVATE Threads::Threads)

# Require C++11 for thread support
target_compile_features(app PRIVATE cxx_std_11)
```

**main.cpp**:
```cpp
#include <iostream>
#include <thread>
#include <vector>
#include <chrono>

void worker(int id) {
    std::cout << "Thread " << id << " starting..." << std::endl;
    std::this_thread::sleep_for(std::chrono::milliseconds(100));
    std::cout << "Thread " << id << " finished!" << std::endl;
}

int main() {
    std::cout << "Main thread starting" << std::endl;
    
    std::vector<std::thread> threads;
    for (int i = 0; i < 5; ++i) {
        threads.emplace_back(worker, i);
    }
    
    for (auto& t : threads) {
        t.join();
    }
    
    std::cout << "All threads completed" << std::endl;
    return 0;
}
```

</details>
