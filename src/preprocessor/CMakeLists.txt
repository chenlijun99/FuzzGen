#
# Copyright (C) 2018 The Android Open Source Project
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
cmake_minimum_required(VERSION 3.13)
project(FuzzGenPreprocessor)

find_package(LLVM 6.0.0 REQUIRED CONFIG)

include_directories(${LLVM_INCLUDE_DIRS})
link_directories(${LLVM_LIBRARY_DIRS})
add_definitions(${LLVM_DEFINITIONS})

set(LLVM_LINK_COMPONENTS ${LLVM_TARGETS_TO_BUILD} asmparser support mc)
llvm_map_components_to_libnames(llvm_libs ${LLVM_LINK_COMPONENTS})

set(TARGET_NAME fuzzgen-preprocessor)
add_executable(${TARGET_NAME} preprocessor.cpp)
set_target_properties(${TARGET_NAME} PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}"
)

target_link_libraries(${TARGET_NAME}
	PRIVATE
	LLVM
	clangBasic
	clangFrontend
	clangAST
	clangAnalysis
	clangBasic
	clangDriver
	clangEdit
	clangFrontend
	clangFrontendTool
	clangLex
	clangParse
	clangSema
	clangEdit
	clangCrossTU
	clangIndex
	clangSerialization
	clangToolingCore
	clangTooling
	${llvm_libs}
)
target_compile_options(${TARGET_NAME}
	PRIVATE
	-std=c++0x
	)
install(TARGETS ${TARGET_NAME} RUNTIME DESTINATION bin)

