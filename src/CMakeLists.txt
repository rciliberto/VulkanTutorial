cmake_minimum_required(VERSION 4.0)
project(VulkanTutorial)

find_package(Vulkan REQUIRED)
find_package(glfw3 CONFIG REQUIRED)
find_package(glm CONFIG REQUIRED)

# Create shaders directory in build folder
file(MAKE_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/shaders)

# Compile vertex shader directly to build directory
add_custom_command(
        OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/shaders/vert.spv
        COMMAND ${Vulkan_GLSLC_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/shaders/shader.vert -o ${CMAKE_CURRENT_BINARY_DIR}/shaders/vert.spv
        DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/shaders/shader.vert
        COMMENT "Compiling vertex shader..."
        VERBATIM
)

# Compile fragment shader directly to build directory
add_custom_command(
        OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/shaders/frag.spv
        COMMAND ${Vulkan_GLSLC_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/shaders/shader.frag -o ${CMAKE_CURRENT_BINARY_DIR}/shaders/frag.spv
        DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/shaders/shader.frag
        COMMENT "Compiling fragment shader..."
        VERBATIM
)

# Add custom target to ensure shaders are compiled
add_custom_target(CompileShaders ALL
        DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/shaders/vert.spv ${CMAKE_CURRENT_BINARY_DIR}/shaders/frag.spv
)

add_executable(VulkanTutorial main.cpp)

# Make sure shaders are compiled before building the executable
add_dependencies(VulkanTutorial CompileShaders)

target_link_libraries(VulkanTutorial PRIVATE glfw)
target_link_libraries(VulkanTutorial PRIVATE glm::glm)
target_link_libraries(VulkanTutorial PRIVATE Vulkan::Vulkan)