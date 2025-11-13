cmake_minimum_required(VERSION 4.0)
project(VulkanTutorial)

find_package(Vulkan REQUIRED)
find_package(glfw3 CONFIG REQUIRED)
find_package(glm CONFIG REQUIRED)

# Add custom command to compile shaders
add_custom_command(
        OUTPUT ${CMAKE_CURRENT_SOURCE_DIR}/shaders/vert.spv ${CMAKE_CURRENT_SOURCE_DIR}/shaders/frag.spv
        COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/shaders/compile.sh
        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/shaders
        DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/shaders/shader.vert ${CMAKE_CURRENT_SOURCE_DIR}/shaders/shader.frag
        COMMENT "Compiling shaders..."
        VERBATIM
)

# Add custom target to ensure shaders are compiled
add_custom_target(CompileShaders ALL
        DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/shaders/vert.spv ${CMAKE_CURRENT_SOURCE_DIR}/shaders/frag.spv
)

add_executable(VulkanTutorial main.cpp)

# Make sure shaders are compiled before building the executable
add_dependencies(VulkanTutorial CompileShaders)

# Copy shader files to build directory
add_custom_command(TARGET VulkanTutorial POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E make_directory $<TARGET_FILE_DIR:VulkanTutorial>/shaders
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
                ${CMAKE_CURRENT_SOURCE_DIR}/shaders/vert.spv
                ${CMAKE_CURRENT_SOURCE_DIR}/shaders/frag.spv
                $<TARGET_FILE_DIR:VulkanTutorial>/shaders/
        COMMENT "Copying shader files to build directory..."
)

target_link_libraries(VulkanTutorial PRIVATE glfw)
target_link_libraries(VulkanTutorial PRIVATE glm::glm)
target_link_libraries(VulkanTutorial PRIVATE Vulkan::Vulkan)