cmake_minimum_required(VERSION 3.12)

# Pull in PICO SDK (must be before project)
set(PICOSYSTEM_DIR $ENV{PICOSYSTEM_DIR})
include(${PICOSYSTEM_DIR}/pico_sdk_import.cmake)

# Initialize the SDK
pico_sdk_init()

project(picosys_mandelbrot C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

# Find the PicoSystem SDK
find_package(PICOSYSTEM REQUIRED)

# Set up your project and sources
picosystem_executable(
  picosys_mandelbrot
  src/picosys_mandelbrot.cpp
)

# Example build options
#pixel_double(picosys_mandelbrot)
disable_startup_logo(picosys_mandelbrot)

# --- Launcher app-SDK: default standalone; -DAPP_SLOT=<n> links for slot n ---
if (NOT LAUNCHER_SDK)
    set(LAUNCHER_SDK "${CMAKE_CURRENT_LIST_DIR}/../../launcher_sdk")
endif()
include(${LAUNCHER_SDK}/app_slot.cmake)
if (DEFINED APP_SLOT)
    launcher_app(picosys_mandelbrot SLOT ${APP_SLOT})
else()
    launcher_app(picosys_mandelbrot)
endif()
