cmake_minimum_required(VERSION 3.0.2)

project(MyGmock)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/Modules/")

MESSAGE(${CMAKE_MODULE_PATH})

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_DEFAULT_STARTUP_PROJECT test_gm)
set(CMAKE_BUILD_TYPE Debug)

# googletest
enable_testing()
find_package(GTest REQUIRED)
find_package(GMock REQUIRED)

# vscode cmake tools
include(CMakeToolsHelpers OPTIONAL)

# include
include_directories("./include")

# include
include_directories(${GTEST_INCLUDE_DIRS})

SET( GCC_COVERAGE_COMPILE_FLAGS "-g -fprofile-arcs -ftest-coverage -Wall -Wextra -Werror -std=c++11")
SET( CMAKE_C_FLAGS  ${CMAKE_C_FLAGS} ${GCC_COVERAGE_COMPILE_FLAGS})
SET( CMAKE_CXX_FLAGS  ${CMAKE_CXX_FLAGS} ${GCC_COVERAGE_COMPILE_FLAGS})

SET(GCC_COVERAGE_LINK_FLAGS    "-pthread")

SET( CMAKE_EXE_LINKER_FLAGS  "${CMAKE_EXE_LINKER_FLAGS} ${GCC_COVERAGE_LINK_FLAGS}" )

# exec
add_executable(MyCmock ./src/MyCmock.cpp)
add_executable(MyStaticMock ./src/MyStaticMock.cpp)
add_executable(MyInOutParam ./src/MyInOutParam.cpp)

# linking
MESSAGE(${GTEST_BOTH_LIBRARIES})
target_link_libraries(MyCmock ${GTEST_BOTH_LIBRARIES} ${GMOCK_BOTH_LIBRARIES})
target_link_libraries(MyStaticMock ${GTEST_BOTH_LIBRARIES} ${GMOCK_BOTH_LIBRARIES})
target_link_libraries(MyInOutParam ${GTEST_BOTH_LIBRARIES} ${GMOCK_BOTH_LIBRARIES})

# test
add_test(GMock-C-Test MyCmock)
add_test(GMock-Static-Test MyStaticMock)
add_test(MyInOutParam-Test MyInOutParam)







