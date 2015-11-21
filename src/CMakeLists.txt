cmake_minimum_required(VERSION 2.6)

project(libqrencode)
string(TOLOWER ${PROJECT_NAME} PROJECT_NAME_LOWER)
set(PROJ_TARGET_NAME libqrencode)


#---------------------------------------
#---------------------------------------
set(CMAKE_DEBUG_POSTFIX "d")
#---------------------------------------
# Set the build type.  Options are:
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries

#set(CMAKE_BUILD_TYPE RelWithDebInfo)
if(NOT CMAKE_BUILD_TYPE)
    #set(CMAKE_BUILD_TYPE RelWithDebInfo CACHE STRING "Build type" FORCE)
    set(CMAKE_BUILD_TYPE Debug)
endif()

#set the default path for built executables to the "bin" directory
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
#set the default path for built libraries to the "lib" directory
set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)
# set output path for tests
set(TEST_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)

#set(CMAKE_INSTALL_PREFIX ${PROJECT_BINARY_DIR}/bin)
set(CMAKE_INSTALL_PREFIX "${CMAKE_SOURCE_DIR}/../${PROJECT_NAME}-installation")
IF(CMAKE_INSTALL_PREFIX)
set(CMAKE_INSTALL_PREFIX "${CMAKE_SOURCE_DIR}/../${PROJECT_NAME}-installation")
ENDIF()

message(STATUS "Install prefix: ${CMAKE_INSTALL_PREFIX}")
message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")


ADD_DEFINITIONS(-DUNICODE -D_UNICODE)
ADD_DEFINITIONS(-DHAVE_CONFIG_H)

set( QRENCODE_CONFIG_DIR "${PROJECT_SOURCE_DIR}")
set( QRENCODE_DIR "${PROJECT_SOURCE_DIR}/qrencode-3.4.4")

include_directories("${QRENCODE_CONFIG_DIR}")
SET( CONFIG_HDRS "${QRENCODE_CONFIG_DIR}/config.h" )

include_directories("${QRENCODE_DIR}")
file ( GLOB QR_HDRS "${QRENCODE_DIR}/*.h")
file ( GLOB QR_SRCS "${QRENCODE_DIR}/*.c")
SET(QR_INCLUDE_HDRS "${QRENCODE_DIR}/qrencode.h")
list(REMOVE_ITEM QR_SRCS "${QRENCODE_DIR}/qrenc.c")

SET(THISPROJ_INCLUDE_HDRS ${QR_INCLUDE_HDRS})

SET(THISPROJ_HDRS ${QR_HDRS} ${CONFIG_HDRS})
SET(THISPROJ_SRCS ${QR_SRCS} )
foreach(s ${THISPROJ_HDRS})
message(STATUS "${s}")
endforeach()
foreach(s ${THISPROJ_SRCS})
message(STATUS "${s}")
endforeach()

set(ALL_HEADERS
    ${THISPROJ_HDRS}
)

set(ALL_SOURCES
    ${ALL_HEADERS}
    ${THISPROJ_SRCS}
    )

ADD_LIBRARY(${PROJ_TARGET_NAME} STATIC ${ALL_SOURCES})

INSTALL(TARGETS ${PROJ_TARGET_NAME}
        ARCHIVE DESTINATION lib)
INSTALL(FILES ${THISPROJ_INCLUDE_HDRS} DESTINATION include)

