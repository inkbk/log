if(APPLE)
  set(CMAKE_MACOSX_RPATH ON)
endif()


set(CMAKE_C_COMPILER /usr/local/Cellar/gcc/9.1.0/bin/gcc-9)
set(CMAKE_CXX_COMPILER /usr/local/Cellar/gcc/9.1.0/bin/g++-9)

cmake_minimum_required(VERSION 2.8)
project(droplet-render)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${PROJECT_SOURCE_DIR}/cmake/")

if(NOT CMAKE_BUILD_TYPE)
	set(CMAKE_BUILD_TYPE "Release")
endif()

set(CMAKE_CXX_FLAGS "-std=c++11 -mcmodel=medium -Wno-unused-parameter")
set(CMAKE_CXX_FLAGS_RELEASE "-O3")

set(TBB_ROOT /usr/local/Cellar/tbb/2019_U5_1)
set(ILMBASE_ROOT /usr/local/Cellar/ilmbase/2.3.0)
set(BLOSC_ROOT /usr/local)
set(OPENVDB_ROOT /usr/local)
set(Python_ADDITIONAL_VERSIONS 3.7)
set(EMBREE_ROOT_DIR /opt/local)
set(MINIMUM_BLOSC_VERSION 1.5)

find_package(tbb REQUIRED)
find_package(PythonInterp 3 REQUIRED)
find_package(PythonLibs 3 REQUIRED)
find_package(OpenVDB REQUIRED)
find_package(ilmbase REQUIRED)
find_package(BLOSC REQUIRED)
find_package(embree 3.0 REQUIRED)

message("EMBREE found: ${EMBREE_LIBRARY}")

include_directories(
/usr/local/include
/opt/local/include
${Tbb_INCLUDE_DIRS}
${OpenVDB_INCLUDE_DIRS}
${PYTHON_INCLUDE_DIRS}
${IlmBase_INCLUDE_DIRS}
${Blosc_INCLUDE_DIRS}
${EMBREE_INCLUDE_DIRS}
)

message(STATUS "PYTHON_EXECUTABLE:FILEPATH=${PYTHON_EXECUTABLE}")
message(STATUS "PYTHON_LIBRARY:FILEPATH=${PYTHON_LIBRARY}")
message(STATUS "PYTHON_INCLUDE_DIR:FILEPATH=${PYTHON_INCLUDE_DIR}")
message(STATUS "PYTHON_FRAMEWORK_INCLUDES=${PYTHON_FRAMEWORK_INCLUDES}")
message(STATUS "PYTHONLIBS_VERSION_STRING=${PYTHONLIBS_VERSION_STRING}")
message(STATUS "Python_FRAMEWORKS=${Python_FRAMEWORKS}")


set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -msse4")

add_definitions(-DDROPLET_CPU)

file(GLOB SOURCES src/*.cpp)

add_library(droplet SHARED ${SOURCES})

TARGET_LINK_LIBRARIES(droplet 
${Tbb_LIBRARIES} 
${OPENVDB_LIBRARIES} 
${Blosc_LIBRARIES}
${OPT_LIBS} 
${PYTHON_LIBRARIES} 
${ILMBase_LIBRARIES} 
/usr/local/Cellar/ilmbase/2.3.0/lib/libHalf.dylib
${EMBREE_LIBRARY}
)