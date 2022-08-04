cmake_minimum_required(VERSION 3.13)

# initialize pico-sdk from submodule
# note: this must happen before project()
include(../pico-sdk/pico_sdk_init.cmake)

project(blink)

# initialize the Raspberry Pi Pico SDK
pico_sdk_init()

add_executable(blink blink.c)

# Pull in common dependencies
target_link_libraries(blink pico_stdlib)

# Create map/bin/hex file etc.
pico_add_extra_outputs(blink)
