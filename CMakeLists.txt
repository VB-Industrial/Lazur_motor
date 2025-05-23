cmake_minimum_required(VERSION 3.22)

#
# This file is generated only once,
# and is not re-generated if converter is called multiple times.
#
# User is free to modify the file as much as necessary
#

# Setup compiler settings
set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS ON)
set (CMAKE_CXX_STANDARD 20)


# Define the build type
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Debug")
endif()

# Set the project name
set(CMAKE_PROJECT_NAME Lazur_motor)
add_definitions(-DARM_MATH_CM4 -D__FPU_PRESENT -DSTM32_G -DMONITOR) #-DENABLE_CALIBRATION) #to enable calibration upon start temp disabled

# Include toolchain file
include("cmake/gcc-arm-none-eabi.cmake")

# Enable compile command to ease indexing with e.g. clangd
set(CMAKE_EXPORT_COMPILE_COMMANDS TRUE)

# Enable CMake support for ASM and C languages
enable_language(C ASM)

# Core project settings
project(${CMAKE_PROJECT_NAME})
message("Build type: " ${CMAKE_BUILD_TYPE})

# Create an executable object type
add_executable(${CMAKE_PROJECT_NAME})

# Add STM32CubeMX generated sources
add_subdirectory(cmake/stm32cubemx)


# Add linked libraries
target_link_libraries(${CMAKE_PROJECT_NAME}
    stm32cubemx
    ${CMAKE_CURRENT_SOURCE_DIR}/Drivers/CMSIS/DSP/Lib/GCC/libarm_cortexM4lf_math.a
    # Add user defined libraries
)

# Link directories setup
target_link_directories(${CMAKE_PROJECT_NAME} PRIVATE
    # Add user defined library search paths
)

file(GLOB_RECURSE USER_SOURCES "App/*.cpp")
file(GLOB_RECURSE LIBCXXCANARD_SOURCES "VB/libcxxcanard/cyphal/*.cpp" "VB/libcxxcanard/libs/*.c")
file(GLOB_RECURSE LIBVOLTBRO_SOURCES "VB/libvoltbro/*.cpp" "VB/libvoltbro/*.c")
target_sources(${CMAKE_PROJECT_NAME} PRIVATE
        ${USER_SOURCES}
        ${LIBVOLTBRO_SOURCES}
        ${LIBCXXCANARD_SOURCES}
)

target_include_directories(${CMAKE_PROJECT_NAME} PRIVATE
        App
        VB/libcxxcanard
        VB/libcxxcanard/libs
        VB/libvoltbro
        VB/libvoltbro/voltbro
        Drivers/CMSIS/DSP/Include
)

# Add project symbols (macros)
target_compile_definitions(${CMAKE_PROJECT_NAME} PRIVATE
    # Add user defined symbols
)

target_compile_definitions(${CMAKE_PROJECT_NAME} PRIVATE
    -DARM_MATH_CM4
    -D__FPU_PRESENT
)

# Add custom commands to generate .bin and .hex files
add_custom_command(TARGET ${CMAKE_PROJECT_NAME} POST_BUILD
COMMAND ${CMAKE_OBJCOPY} -O binary $<TARGET_FILE:${CMAKE_PROJECT_NAME}> ${CMAKE_PROJECT_NAME}.bin
COMMAND ${CMAKE_OBJCOPY} -O ihex $<TARGET_FILE:${CMAKE_PROJECT_NAME}> ${CMAKE_PROJECT_NAME}.hex
COMMENT "Building ${CMAKE_PROJECT_NAME}.bin and ${CMAKE_PROJECT_NAME}.hex"

COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_PROJECT_NAME}.elf ../${CMAKE_PROJECT_NAME}.elf
COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_PROJECT_NAME}.bin ../${CMAKE_PROJECT_NAME}.bin
COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_PROJECT_NAME}.hex ../${CMAKE_PROJECT_NAME}.hex
COMMENT "Copy ${PROJECT_BINARY_DIR}/${CMAKE_PROJECT_NAME}.hex , bin , elf"
)
