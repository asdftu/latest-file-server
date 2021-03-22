cmake_minimum_required(VERSION 3.1)

PROJECT(app_latest_file_server)



MESSAGE(STATUS "operation system is ${CMAKE_SYSTEM}")
IF (CMAKE_SYSTEM_NAME MATCHES "Linux")
  MESSAGE(STATUS "current platform: Linux ")
ELSEIF (CMAKE_SYSTEM_NAME MATCHES "Windows")
  MESSAGE(STATUS "current platform: Windows")
ELSEIF (CMAKE_SYSTEM_NAME MATCHES "Darwin")
  MESSAGE(STATUS "current platform: MacOS")
ELSE ()
  MESSAGE(STATUS "other platform: ${CMAKE_SYSTEM_NAME}")
ENDIF()


IF( NOT CMAKE_BUILD_TYPE )
   SET( CMAKE_BUILD_TYPE debug ... FORCE )
ENDIF()

SET(BUILD_MODE_DIR ".")

IF (CMAKE_SYSTEM_NAME MATCHES "Windows")
  IF(CMAKE_BUILD_TYPE MATCHES debug)
    message("Debug build.")
    SET(BUILD_MODE_DIR "Debug")
  ELSEIF(CMAKE_BUILD_TYPE MATCHES release)
    message("Release build.")
    SET(BUILD_MODE_DIR "Release")
  ELSE()
    MESSAGE(FATAL_ERROR  "Unkown Build Type, It should be either debug or release")
  ENDIF()
ENDIF()


SET(PROJECT_DIR ${CMAKE_CURRENT_SOURCE_DIR})

SET(LIBS_DIR ${PROJECT_DIR}/libs)
#libuv
INCLUDE_DIRECTORIES(${LIBS_DIR}/libuv/include)
LINK_DIRECTORIES(${LIBS_DIR}/libuv/${BUILD_MODE_DIR}/lib)
#uv-cpp
INCLUDE_DIRECTORIES(${LIBS_DIR})
LINK_DIRECTORIES(${LIBS_DIR}/uv-cpp/${BUILD_MODE_DIR}/lib)


INCLUDE_DIRECTORIES(${PROJECT_DIR}/src)
AUX_SOURCE_DIRECTORY(${PROJECT_DIR}/src PROJECT_SOURCES)

ADD_EXECUTABLE(app_latest_file_server ${PROJECT_SOURCES})
TARGET_LINK_LIBRARIES(app_latest_file_server uv_cpp)

