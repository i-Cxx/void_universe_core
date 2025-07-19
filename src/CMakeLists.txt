# Generated Cmake Pico project file

cmake_minimum_required(VERSION 3.13)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Initialise pico_sdk from installed location
# (note this can come from environment, CMake cache etc)

# == DO NOT EDIT THE FOLLOWING LINES for the Raspberry Pi Pico VS Code Extension to work ==
if(WIN32)
    set(USERHOME $ENV{USERPROFILE})
else()
    set(USERHOME $ENV{HOME})
endif()
set(sdkVersion 2.1.1)
set(toolchainVersion 14_2_Rel1)
set(picotoolVersion 2.1.1)
set(picoVscode ${USERHOME}/.pico-sdk/cmake/pico-vscode.cmake)
if (EXISTS ${picoVscode})
    include(${picoVscode})
endif()
# ====================================================================================
set(PICO_BOARD pico CACHE STRING "Board type")


# set(ENV{PICO_SDK_PATH} "${CMAKE_SOURCE_DIR}/pico-sdk")

# Set flags and directory variables
# set(FREERTOS_CFG_DIRECTORY "${CMAKE_SOURCE_DIR}/Config")
# set(FREERTOS_SRC_DIRECTORY "${CMAKE_SOURCE_DIR}/FreeRTOS-Kernel")


# Pull in Raspberry Pi Pico SDK (must be before project)
include(pico_sdk_import.cmake)

project(void_universe_core C CXX ASM)

# Initialise the Raspberry Pi Pico SDK
pico_sdk_init()

# Add executable. Default name is the project name, version 0.1

# Add FreeRTOS as a library
# add_library(FreeRTOS STATIC
#     ${FREERTOS_SRC_DIRECTORY}/event_groups.c
#     ${FREERTOS_SRC_DIRECTORY}/list.c
#     ${FREERTOS_SRC_DIRECTORY}/queue.c
#     ${FREERTOS_SRC_DIRECTORY}/stream_buffer.c
#     ${FREERTOS_SRC_DIRECTORY}/tasks.c
#     ${FREERTOS_SRC_DIRECTORY}/timers.c
#     ${FREERTOS_SRC_DIRECTORY}/portable/MemMang/heap_3.c
#     ${FREERTOS_SRC_DIRECTORY}/portable/GCC/ARM_CM0/port.c
# )

# Build FreeRTOS
# target_include_directories(FreeRTOS PUBLIC
#     ${FREERTOS_CFG_DIRECTORY}/
#     ${FREERTOS_SRC_DIRECTORY}/include
#     ${FREERTOS_SRC_DIRECTORY}/portable/GCC/ARM_CM0
# )


add_library(freertos_config INTERFACE)
target_include_directories(freertos_config INTERFACE
    ${CMAKE_SOURCE_DIR}/include
)

# Webserver Subdirectory einbinden (erst hier, weil in src/)
add_subdirectory(webserver)




add_executable(void_universe_core 
                void_universe_core.c 
)




pico_set_program_name(void_universe_core "void_universe_core")
pico_set_program_version(void_universe_core "0.1")

# Modify the below lines to enable/disable output over UART/USB
pico_enable_stdio_uart(void_universe_core 0)
pico_enable_stdio_usb(void_universe_core 1)

# Add the standard library to the build
target_link_libraries(void_universe_core
        pico_stdlib)

# Add the standard include files to the build
target_include_directories(void_universe_core PRIVATE
        ${CMAKE_CURRENT_LIST_DIR}
)

# Add any user requested libraries
target_link_libraries(void_universe_core 
        pico_stdlib
        hardware_spi
        hardware_gpio
        hardware_i2c
        hardware_timer
        pico_stdlib
    
    
        freertos_kernel      
        freertos_config     

        lwip
        lwipcore
        lwipallapps
        
        webserver     

        
        tinyusb_device
        
        led_lib
        lcd1602_lib
        ssd1306_lib
        )


add_compile_definitions($<$<CONFIG:Debug>:DEBUG=1>)
# Compiler Optionen
target_compile_options(infinity_rts PRIVATE -Wall -Wextra -O2)

# UART für printf aktivieren
pico_enable_stdio_uart(infinity_rts 1)
pico_enable_stdio_usb(infinity_rts 0)


pico_add_extra_outputs(void_universe_core)



