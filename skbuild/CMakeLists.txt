cmake_minimum_required(VERSION 3.15)

project(IsoSpecPy CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

#find_package(PythonExtensions REQUIRED)
#find_package(Python COMPONENTS Interpreter Development REQUIRED)

# Runtime ISA dispatch. This matters most here: the wheel is built with no
# -march at all, so without it every user gets the x86-64 baseline -- 2-double
# vectors -- no matter what their CPU can do.
include(${CMAKE_CURRENT_LIST_DIR}/../src/IsoSpec++/IsaDispatch.cmake)

add_library(IsoSpecCppPy SHARED ../src/IsoSpec++/unity-build.cpp ${isospec_ISA_SRCS})
target_compile_features(IsoSpecCppPy PRIVATE cxx_std_20)
#python_extension_module(IsoSpecCppPy)
install(TARGETS IsoSpecCppPy LIBRARY DESTINATION IsoSpecPy)
install(DIRECTORY ../src/IsoSpec++/ DESTINATION IsoSpecPy/IsoSpec++
        FILES_MATCHING PATTERN "*.h" PATTERN "*.hpp" PATTERN "*.hxx"
)
