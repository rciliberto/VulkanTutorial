cmake_minimum_required(VERSION 4.0)
project(VulkanTutorial)

find_package(Vulkan REQUIRED)
find_package(glfw3 CONFIG REQUIRED)
find_package(glm CONFIG REQUIRED)

add_executable(VulkanTutorial main.cpp)

target_link_libraries(VulkanTutorial PRIVATE glfw)
target_link_libraries(VulkanTutorial PRIVATE glm::glm)
target_link_libraries(VulkanTutorial PRIVATE Vulkan::Vulkan)