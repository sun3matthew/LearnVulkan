# Minimum required CMake version
cmake_minimum_required(VERSION 3.10)

# Project name and version
project(LearnVulkan VERSION 1.0)

# Specify the C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# Find all source files in src/
file(GLOB_RECURSE SRC_FILES ${CMAKE_SOURCE_DIR}/src/*.cpp ${CMAKE_SOURCE_DIR}/src/*.c)

# Add executable target
add_executable(${PROJECT_NAME} ${SRC_FILES})

# Include directories
target_include_directories(${PROJECT_NAME} PRIVATE ${CMAKE_SOURCE_DIR}/include )

# include_directories(external/PerlinNoise)

set(TINYGLTF_HEADER_ONLY ON CACHE INTERNAL "" FORCE)
set(TINYGLTF_INSTALL OFF CACHE INTERNAL "" FORCE)
add_subdirectory(external/tinygltf)
target_include_directories(${PROJECT_NAME} PRIVATE "external/tinygltf")

# add_library(stb STATIC external/stb/src/stb_image.cpp)
# target_include_directories(stb PUBLIC external/stb/include)
# target_link_libraries(${PROJECT_NAME} PRIVATE stb)

add_subdirectory(external/glm)
target_link_libraries(${PROJECT_NAME} PRIVATE glm)

add_subdirectory(external/glfw)
target_link_libraries(${PROJECT_NAME} PRIVATE glfw)

# Include Vulkan
find_package(Vulkan REQUIRED)
target_include_directories(${PROJECT_NAME} PRIVATE $ENV{VULKAN_SDK}/include)
target_link_libraries(${PROJECT_NAME} PRIVATE Vulkan::Vulkan)

# Compiler definitions
target_compile_definitions(${PROJECT_NAME} PRIVATE GL_SILENCE_DEPRECATION)

# Copy resources directory to the build folder
file(COPY ${CMAKE_SOURCE_DIR}/resources/ DESTINATION ${CMAKE_BINARY_DIR}/resources)
