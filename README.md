<h1 align="center">RPI Pico c/c++ playground</h1>

RPI Pico C/C++ sdk playground

## 1. Install Cmake and the GCC cross compiler

```bash
sudo apt install cmake gcc-arm-none-eabi libnewlib-arm-none-eabi libstdc++-arm-none-eabi-newlib
```

## 2. Create playground directory
```bash
$ mkdir pico_playground
$ cd pico_playground
```

## 3. Add the pico SDK to the project
There are several ways to do this all of which are documented in the [sdk github repo](https://github.com/raspberrypi/pico-sdk) i use the submodule method:
```bash
$ git submodule add git@github.com:raspberrypi/pico-sdk.git
$ cd pico-sdk
$ git submodule update --init
$ cd ..
```

## 4. Create project directory
Create a `blink` project directory inside your playground directory:
```bash
$ mkdir blink
$ cd blink
```

### 5. Add the blink source file
Using your favorite text editor create the `blink.c` source file and add the following:
```c
#include "pico/stdlib.h"

int main() {
#ifndef PICO_DEFAULT_LED_PIN
#warning blink example requires a board with a regular LED
#else
    const uint LED_PIN = PICO_DEFAULT_LED_PIN;
    gpio_init(LED_PIN);
    gpio_set_dir(LED_PIN, GPIO_OUT);
    while (true) {
        gpio_put(LED_PIN, 1);
        sleep_ms(250);
        gpio_put(LED_PIN, 0);
        sleep_ms(250);
    }
#endif
}
```
### 6. Setup the build system with CMake
Within the same project directory create a `CMakeLists.txt` like:
```cmake
cmake_minimum_required(VERSION 3.13)

# initialize pico-sdk from submodule
# note: this must happen before project()
include(../pico-sdk/pico_sdk_init.cmake)

project(blink)

# initialize the Raspberry Pi Pico SDK
pico_sdk_init()

add_executable(blink blink.c)

# Add pico_stdlib library which aggregates commonly used features
target_link_libraries(blink pico_stdlib)

# create map/bin/hex/uf2 file in addition to ELF.
pico_add_extra_outputs(blink)
```

## 7. Setup the build directory and build the project
```bash
$ mkdir build
$ cmake -S . -B build/
cmake --build build
```
You should now have a `blink.elf` to load via a debugger or `blink.uf2` that can be installed on the Pico via drag and drop. 


## Updating the sdk
To do this go into the `pico-sdk` directory which contains your copy of the SDK and run the following;
```bash
$ cd pico-sdk
$ git pull
$ git submodule update
```

## <b>Resources</b>
1. [Documentation]()
2. [Pico-sdk github repo]()

## <b>License</b>
