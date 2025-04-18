cmake_minimum_required(VERSION 3.13)
include(pico-sdk/pico_sdk_init.cmake)

project(firmware)
pico_sdk_init()

add_executable(firmware
  main.c
  usb_descriptors.c
  mongoose.c
  net.c
  pico-sdk/lib/tinyusb/lib/networking/rndis_reports.c)

target_include_directories(firmware PUBLIC
  .
  pico-sdk/lib/tinyusb/lib/networking)

target_link_libraries(firmware pico_stdlib pico_rand hardware_spi tinyusb_device)
pico_add_extra_outputs(firmware)     # create map/bin/hex file etc.

pico_enable_stdio_usb(firmware 0)    # Route stdio
pico_enable_stdio_uart(firmware 1)   # to the UART

# Prevent extreme optimization from breaking OTA
target_compile_options(firmware PRIVATE -fno-tree-loop-distribute-patterns)

# Mongoose build flags in mongoose_config.h

# Example build options
