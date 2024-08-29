cmake_minimum_required(VERSION 3.17)

project(tv-test C CXX ASM)


# CMake options
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_VERBOSE_MAKEFILE ON)


# Подсасываем библиотеки
add_subdirectory(tvision)
# add_subdirectory(${LIB_ROOT}/umba)


include(.cmake/common.cmake)
# Framework нам пока не нужен
# include(.cmake/framework.cmake)


# C/C++ language settings
# https://matgomes.com/setting-cpp-standard-cmake/
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED On)
set(CMAKE_C_EXTENSIONS Off)
set(CMAKE_CXX_EXTENSIONS Off)


set(COMMON_LIBS tvision::tvision)

include_directories(".")

# Project include paths
set(INCLUDES
)


set(SOURCES test.cpp
)


if(STM32)
endif()  # STM32



add_executable(${PROJECT_NAME} ${SOURCES})
fw_add_target_options(${PROJECT_NAME}
                      "UNICODE"
                      "CONSOLE"
                      "BIGOBJ"
                      #"BIN"
                      #"HEX"
                      # "FLASH"
                      )
target_link_libraries(${PROJECT_NAME} PRIVATE "${COMMON_LIBS}")

# target_include_directories(${PROJECT_NAME} PRIVATE ${INCLUDES})

# target_compile_definitions(${PROJECT_NAME} PRIVATE "ES_4007_BMCL")
# target_compile_definitions(${PROJECT_NAME} PRIVATE "FIRMWARE_TEST_MODE")

# target_link_libraries(${PROJECT_NAME} PRIVATE "${COMMON_LIBS}")

# if(STM32)
#     # Настраиваем TheFramework
#     target_link_options(${PROJECT_NAME} PRIVATE "-Wl,--defsym=FW_HDR_FLAGS=0x7")
#     
#     # Настраиваем раскладку флешки/срама
#     target_compile_definitions(${PROJECT_NAME} PRIVATE "flash_layout")
#     target_compile_definitions(${PROJECT_NAME} PRIVATE "sram_layout")
#     
#     # Настраиваем тактирование
#     target_compile_definitions(${PROJECT_NAME} PRIVATE "HSE_VALUE=16000000" "MCU_PLL_M=16" "MCU_PLL_Q=7" "MCU_PLL_N=360" "MCU_PLL_P=2")
# endif()










