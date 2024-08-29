cmake_minimum_required(VERSION 3.17)

project(tvision C CXX ASM)


if(NOT DEFINED CMAKE_BUILD_TYPE AND NOT DEFINED CMAKE_CONFIGURATION_TYPES)
    set(CMAKE_BUILD_TYPE Release CACHE STRING "Build type" FORCE)
    set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()


set(MODULE_ROOT "${CMAKE_CURRENT_LIST_DIR}")



# https://cmake.org/cmake/help/latest/variable/CMAKE_LANG_COMPILER_ID.html

if (CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
    add_compile_definitions(${PROJECT_NAME} PRIVATE "TVComp_GCC")
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
    add_compile_definitions(${PROJECT_NAME} PRIVATE "TVComp_GCC")
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
    add_compile_definitions(${PROJECT_NAME} PRIVATE "TVComp_MSC")
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "Embarcadero" OR CMAKE_CXX_COMPILER_ID STREQUAL "Borland")
    add_compile_definitions(${PROJECT_NAME} PRIVATE "TVComp_BCPP")
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "OpenWatcom" OR CMAKE_CXX_COMPILER_ID STREQUAL "Borland")
    add_compile_definitions(${PROJECT_NAME} PRIVATE "TVComp_Watcom")
endif()



file(GLOB CLASSES_SRC         "${MODULE_ROOT}/classes/*.cc"          )


file(GLOB CLASSES_ALCON_SRC   "${MODULE_ROOT}/classes/alcon/*.cc"    ) # Allegro ???
file(GLOB CLASSES_DOS_SRC     "${MODULE_ROOT}/classes/dos/*.cc" "${MODULE_ROOT}/classes/dos/*.c")
file(GLOB CLASSES_LINUX_SRC   "${MODULE_ROOT}/classes/linux/*.cc"    )
file(GLOB CLASSES_QNX4_SRC    "${MODULE_ROOT}/classes/qnx4/*.cc"     )
file(GLOB CLASSES_QNXRTP_SRC  "${MODULE_ROOT}/classes/qnxrtp/*.cc"   )
file(GLOB CLASSES_UNIX_SRC    "${MODULE_ROOT}/classes/unix/*.cc"     )
file(GLOB CLASSES_WIN32_SRC   "${MODULE_ROOT}/classes/win32/*.cc"    )
file(GLOB CLASSES_WINGR_SRC   "${MODULE_ROOT}/classes/wingr/*.cc"    )
file(GLOB CLASSES_WINNT_SRC   "${MODULE_ROOT}/classes/winnt/*.cc"    )
file(GLOB CLASSES_X11_SRC     "${MODULE_ROOT}/classes/x11/*.cc"      )

file(GLOB COMPAT_SRC          "${MODULE_ROOT}/compat/*.cc" "${MODULE_ROOT}/compat/*.c")

file(GLOB NAMES_SRC           "${MODULE_ROOT}/names/*.cc" "${MODULE_ROOT}/names/*.c")

file(GLOB STREAM_SRC          "${MODULE_ROOT}/stream/*.cc")

# https://gitlab.kitware.com/cmake/community/-/wikis/doc/tutorials/How-To-Write-Platform-Checks
# https://stackoverflow.com/questions/9160335/os-specific-instructions-in-cmake-how-to
# https://cmake.org/cmake/help/latest/variable/CMAKE_SYSTEM_NAME.html
# https://coderefinery.github.io/cmake-workshop/environment/
# if(CMAKE_SYSTEM_NAME STREQUAL "Linux")


if (WIN32)

    # list(APPEND SOURCES ${CLASSES_WIN32_SRC})
    # # list(APPEND SOURCES ${CLASSES_WINNT_SRC}) # ???
    set(PALTFROM_SOURCES ${CLASSES_WIN32_SRC})
    # set(PALTFROM_SOURCES ${CLASSES_WINNT_SRC}) # ???

elseif(CMAKE_SYSTEM_NAME STREQUAL "DOS")

    # list(APPEND SOURCES ${CLASSES_DOS_SRC})
    set(PALTFROM_SOURCES ${CLASSES_DOS_SRC})

elseif(CMAKE_SYSTEM_NAME STREQUAL "Linux")

    # list(APPEND SOURCES ${CLASSES_LINUX_SRC})
    set(PALTFROM_SOURCES ${CLASSES_LINUX_SRC})

elseif(CMAKE_SYSTEM_NAME STREQUAL "BSDOS" OR CMAKE_SYSTEM_NAME STREQUAL "FreeBSD" OR CMAKE_SYSTEM_NAME STREQUAL "HP-UX" OR CMAKE_SYSTEM_NAME STREQUAL "kFreeBSD" OR CMAKE_SYSTEM_NAME STREQUAL "MirBSD" OR CMAKE_SYSTEM_NAME STREQUAL "NetBSD" OR CMAKE_SYSTEM_NAME STREQUAL "OpenBSD" )

    # list(APPEND SOURCES ${CLASSES_UNIX_SRC})
    set(PALTFROM_SOURCES ${CLASSES_UNIX_SRC})

elseif(CMAKE_SYSTEM_NAME STREQUAL "QNX")

    # list(APPEND SOURCES ${CLASSES_QNX4_SRC})
    set(PALTFROM_SOURCES ${CLASSES_QNX4_SRC})

else()

    set(PALTFROM_SOURCES )

endif()


set(SOURCES ${CLASSES_SRC} ${PALTFROM_SOURCES})

source_group(TREE ${CMAKE_CURRENT_SOURCE_DIR} PREFIX "Sources" FILES ${SOURCES})


file(GLOB_RECURSE HEADERS "${MODULE_ROOT}/include/*.h")
source_group(TREE ${CMAKE_CURRENT_SOURCE_DIR}/include PREFIX "Headers" FILES ${HEADERS})

add_library(${PROJECT_NAME} ${SOURCES} ${HEADERS})
add_library(${PROJECT_NAME}::${PROJECT_NAME} ALIAS ${PROJECT_NAME})

target_include_directories(${PROJECT_NAME} PUBLIC  ${MODULE_ROOT}/include)
target_include_directories(${PROJECT_NAME} PRIVATE ${MODULE_ROOT}/..)



# # list(FILTER sources EXCLUDE REGEX ".*CMakeCXXCompilerId\\.cpp$")
# source_group(TREE ${CMAKE_CURRENT_SOURCE_DIR} PREFIX "Sources" FILES ${sources})

# file(GLOB_RECURSE headers "${MODULE_ROOT}/*.h")
# list(FILTER headers EXCLUDE REGEX ".*zz_.*\\.h$")
# source_group(TREE ${CMAKE_CURRENT_SOURCE_DIR} PREFIX "Headers" FILES ${headers})


# add_library(${PROJECT_NAME} ${sources} ${headers})
# add_library(${PROJECT_NAME}::${PROJECT_NAME} ALIAS ${PROJECT_NAME})

# target_include_directories(${PROJECT_NAME} PRIVATE ${MODULE_ROOT}/..)




# file(GLOB_RECURSE headers "${MODULE_ROOT}/*.h")
# source_group(TREE ${CMAKE_CURRENT_SOURCE_DIR}/inc/umba PREFIX "Headers" FILES ${headers})


# add_library(${PROJECT_NAME} ${sources} ${headers})
# add_library(${PROJECT_NAME}::${PROJECT_NAME} ALIAS ${PROJECT_NAME})

# target_include_directories(${PROJECT_NAME} PUBLIC  ${MODULE_ROOT}/inc)
# target_include_directories(${PROJECT_NAME} PRIVATE ${MODULE_ROOT}/..)




