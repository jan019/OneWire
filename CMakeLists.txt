# v3.21 implemented semantic changes regarding $<TARGET_OBJECTS:...>
# See https://cmake.org/cmake/help/v3.21/command/target_link_libraries.html#linking-object-libraries-via-target-objects
cmake_minimum_required(VERSION 3.21)

add_library(OneWire INTERFACE)
add_library(OneWire_usage INTERFACE)

target_include_directories(OneWire_usage INTERFACE
  src
)


target_link_libraries(OneWire_usage INTERFACE
  base_config
)

target_link_libraries(OneWire INTERFACE OneWire_usage)



add_library(OneWire_bin OBJECT EXCLUDE_FROM_ALL
  src/drivers/DSTherm.cpp
  src/OneWireNg.cpp
  src/OneWireNg_BitBang.cpp
  src/platform/OneWireNg_ArduinoIdfESP32.cpp
  src/platform/OneWireNg_ArduinoIdfESP8266.cpp
  src/platform/Platform_Delay.cpp
  src/platform/Platform_TimeCritical.cpp
)
target_link_libraries(OneWire_bin PUBLIC OneWire_usage)

target_link_libraries(OneWire INTERFACE
  OneWire_bin
  $<TARGET_OBJECTS:OneWire_bin>
)

add_definitions(-DARDUINO)

