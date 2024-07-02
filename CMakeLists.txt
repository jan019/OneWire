# v3.21 implemented semantic changes regarding $<TARGET_OBJECTS:...>
# See https://cmake.org/cmake/help/v3.21/command/target_link_libraries.html#linking-object-libraries-via-target-objects
cmake_minimum_required(VERSION 3.21)

add_library(OneWireNg INTERFACE)
add_library(OneWireNg_usage INTERFACE)

target_include_directories(OneWireNg_usage INTERFACE
  src
)


target_link_libraries(OneWireNg_usage INTERFACE
  base_config
)

target_link_libraries(OneWireNg INTERFACE OneWireNg_usage)



add_library(OneWireNg_bin OBJECT EXCLUDE_FROM_ALL
  src/drivers/DSTherm.cpp
  src/OneWireNg.cpp
  src/OneWireNg_BitBang.cpp
  src/platform/OneWireNg_ArduinoIdfESP32.cpp
  src/platform/OneWireNg_ArduinoIdfESP8266.cpp
  src/platform/Platform_Delay.cpp
  src/platform/Platform_TimeCritical.cpp
)
target_link_libraries(OneWireNg_bin PUBLIC OneWireNg_usage)

target_link_libraries(OneWireNg INTERFACE
  OneWireNg_bin
  $<TARGET_OBJECTS:OneWireNg_bin>
)

add_definitions(-DARDUINO)

