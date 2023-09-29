cmake_minimum_required(VERSION 3.12)

project(ClangDaily)

# Let CMake to find LLVM and Clang modules.
# Give `-DCMAKE_PREFIX_PATH` or `-DLLVM_DIR` and `-DCLANG_DIR`.
# See <https://cmake.org/cmake/help/v3.11/command/find_package.html>
find_package(Clang REQUIRED CONFIG)
find_package(LLVM REQUIRED CONFIG)


# https://llvm.org/docs/CMake.html#embedding-llvm-in-your-project
message(STATUS "hello world!")
message(STATUS "Found LLVM ${LLVM_PACKAGE_VERSION}")
message(STATUS "Using LLVMConfig.cmake in: ${LLVM_DIR}")
message(STATUS "LLVM_INCLUDE_DIRS: ${LLVM_INCLUDE_DIRS}")
message(STATUS "LLVM_DEFINITIONS: ${LLVM_DEFINITIONS}")
message(STATUS "--------------------------------")

message(STATUS "Found Clang ${Clang_PACKAGE_VERSION}")
message(STATUS "Using ClangConfig.cmake in: ${Clang_DIR}")
message(STATUS "CLANG_INCLUDE_DIRS: ${CLANG_INCLUDE_DIRS}")
message(STATUS "CLANG_INSTALL_PACKAGE_DIR: ${CLANG_INSTALL_PACKAGE_DIR}")
message(STATUS "LLVM_INSTALL_PACKAGE_DIR: ${LLVM_INSTALL_PACKAGE_DIR}")

message(STATUS "--------------------------------")

include(CMakePrintHelpers)
cmake_print_properties(TARGETS clangTooling clangAST clangBasic clangFrontend
    PROPERTIES LOCATION)

# Use given `clang` toolchain.
set(CMAKE_CXX_COMPILER "${LLVM_TOOLS_BINARY_DIR}/clang++")
set(CMAKE_C_COMPILER "${LLVM_TOOLS_BINARY_DIR}/clang")

include_directories(${CLANG_INCLUDE_DIRS})

# https://clang.llvm.org/docs/LibASTMatchersTutorial.html
add_executable(loop-convert
    LoopConvert.cpp)

# these libraries are set after `find_package(Clang REQUIRED CONFIG)`, to see full libraries at `ClangTargets.cmake` under `${Clang_DIR}` directory.
target_link_libraries(loop-convert
    clangAST
    clangBasic
    clangFrontend
    clangIndex
    clangRewrite
    clangTooling
)


install(TARGETS loop-convert RUNTIME DESTINATION bin)