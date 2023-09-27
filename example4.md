# Example4

**Essential Packages on Ubuntu**

1. apt-get update

```python
apt-get update
```

2.  apt-get install -y sudo vim wget unzip g++ cmake curl pkg-config libssl-dev libsasl2-dev git python3 nano

```c
apt-get install -y sudo vim wget unzip g++ cmake curl pkg-config libssl-dev libsasl2-dev git python3 nano
```

3. mkdir clientcpp 

```python
mkdir clientcpp   
```

4. cd clientcpp 

```python
cd clientcpp
```

**Extract Crow Framework**

5.  wget https://github.com/CrowCpp/Crow/releases/download/v1.0%2B5/crow-v1.0+5.tar.gz

```python
wget https://github.com/CrowCpp/Crow/releases/download/v1.0%2B5/crow-v1.0+5.tar.gz
```

6.  mkdir crow

```python
mkdir crow
```

7.  tar xvfz crow-v1.0+5.tar.gz -C crow --strip-components=1

```python
tar xvfz crow-v1.0+5.tar.gz -C crow --strip-components=1
```

**Extract Boost Libraries**

8.  wget https://boostorg.jfrog.io/artifactory/main/release/1.83.0/source/boost_1_83_0.tar.gz

```python
wget https://boostorg.jfrog.io/artifactory/main/release/1.83.0/source/boost_1_83_0.tar.gz
```

9.  tar -xzvf boost_1_83_0.tar.gz

```python
tar -xzvf boost_1_83_0.tar.gz
```

**Extract cpr library and build**

10. git clone https://github.com/libcpr/cpr.git

```python
git clone https://github.com/libcpr/cpr.git
```

11. cd CPR && mkdir build && cd build

```python
cd cpr && mkdir build && cd build
```

12. cmake .. -DCPR_USE_SYSTEM_CURL=ON

```python
cmake .. -DCPR_USE_SYSTEM_CURL=ON
```

13. cmake --build . --parallel

```python
cmake --build . --parallel
```

14. sudo cmake --install .

```python
sudo cmake --install .
```

<br>

15.  nano main.cpp

```python
nano main.cpp
```

```c
#include "crow.h"
#include <cpr/cpr.h>

int main() {
    crow::SimpleApp app;

    CROW_ROUTE(app, "/send_request")
    ([]{
        // Using CPR to send an HTTP GET request
        cpr::Response r = cpr::Get(cpr::Url{"http://localhost:8080"});

        if (r.status_code == 200) {  // HTTP status code: OK
            return crow::response(200, r.text);
        } else {
            return crow::response(500, "Failed to get data");
        }
    });

    app.port(8081).multithreaded().run();
}
```

16.  nano CMakeLists.txt

```python
nano CMakeLists.txt
```

```cmake
cmake_minimum_required(VERSION 3.15)
project(clientcpp)

# Define the include directories
set(INCLUDE_PATHS ./boost_1_83_0 ./crow/include)

# Add the executable target
add_executable(clientcpp main.cpp)

# Include the defined paths
target_include_directories(clientcpp PUBLIC ${INCLUDE_PATHS})

# Specify the C++ standard
set_target_properties(clientcpp PROPERTIES
    CXX_STANDARD 17
    CXX_STANDARD_REQUIRED TRUE
)

find_package(cpr REQUIRED)
target_link_libraries(clientcpp PRIVATE cpr::cpr)
```

**Build the Application**

17.  mkdir build

```python
mkdir build
```

18.  cd build

```python
cd build
```

19.  cmake ..

```python
cmake ..
```

20.  make

```python
make
```

**Run the Application**

21.  ./clientcpp

```python
./clientcpp &
```