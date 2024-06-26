cmake_minimum_required(VERSION 3.27.8)
project(GarDEADner)

set(CMAKE_CXX_STANDARD 23)

find_package(Vulkan REQUIRED COMPONENTS shaderc_combined)

set(CPM_SOURCE_CACHE external)
include(cmake/CPM.cmake)
CPMAddPackage("gh:charles-lunarg/vk-bootstrap#v${Vulkan_VERSION}")
CPMAddPackage("gh:libsdl-org/SDL#a4f962fd46b5b0b4ed9d5eb1e40a203cc0e5f422")
CPMAddPackage("gh:libsdl-org/SDL_ttf#ad1ecab775ac285ba7546b6ee90252549a25452c")
CPMAddPackage("gh:libsdl-org/SDL_image#60013b1f94676e38d15c57de3e5cd36483c78187")
CPMAddPackage("gh:Neargye/magic_enum#dd6a39d0ba1852cf06907e0f0573a2a10d23c2ad")
CPMAddPackage("gh:KhronosGroup/SPIRV-Reflect#vulkan-sdk-${Vulkan_VERSION}.0")
CPMAddPackage("gh:zeux/volk#vulkan-sdk-${Vulkan_VERSION}.0")
CPMAddPackage("gh:GPUOpen-LibrariesAndSDKs/VulkanMemoryAllocator#v3.1.0")
CPMAddPackage("gh:g-truc/glm#1.0.1")

add_executable(GarDEADner
        src/Component.cpp
        src/Component.hpp
        src/Entity.cpp
        src/Entity.hpp
        src/Game/Level.cpp
        src/Game/Level.hpp
        src/Game/Time.cpp
        src/Game/Time.hpp
        src/InputEngine/Input.cpp
        src/InputEngine/Input.hpp
        src/InputEngine/KeyboardController.cpp
        src/InputEngine/KeyboardController.hpp
        src/RenderEngine/CommandBuffer.cpp
        src/RenderEngine/CommandBuffer.hpp
        src/RenderEngine/DescriptorAllocator.cpp
        src/RenderEngine/DescriptorAllocator.hpp
        src/RenderEngine/GraphicsDevice.cpp
        src/RenderEngine/GraphicsDevice.hpp
        src/RenderEngine/GraphicsInstance.cpp
        src/RenderEngine/GraphicsInstance.hpp
        src/RenderEngine/Image.cpp
        src/RenderEngine/Image.hpp
        src/RenderEngine/Pipeline.cpp
        src/RenderEngine/Pipeline.hpp
        src/RenderEngine/RenderPass.cpp
        src/RenderEngine/RenderPass.hpp
        src/RenderEngine/Renderer.cpp
        src/RenderEngine/Renderer.hpp
        src/RenderEngine/Resource.cpp
        src/RenderEngine/Resource.hpp
        src/RenderEngine/Shader.cpp
        src/RenderEngine/Shader.hpp
        src/RenderEngine/ShaderUsage.cpp
        src/RenderEngine/ShaderUsage.hpp
        src/RenderEngine/Window.cpp
        src/RenderEngine/Window.hpp

        ${SPIRV-Reflect_SOURCE_DIR}/spirv_reflect.c
)
target_sources(GarDEADner PRIVATE main.cpp)
target_compile_definitions(GarDEADner PRIVATE VK_NO_PROTOTYPES NO_SDL_VULKAN_TYPEDEFS)
target_include_directories(GarDEADner PRIVATE ${CMAKE_SOURCE_DIR} ${Vulkan_INCLUDE_DIRS} ${vk-bootstrap_SOURCE_DIR}/src ${SDL_SOURCE_DIR}/include ${SDL_ttf_SOURCE_DIR}/include ${SDL_image_SOURCE_DIR} ${magic_enum_SOURCE_DIR}/include ${SPIRV-Reflect_SOURCE_DIR})
target_link_libraries(GarDEADner PRIVATE volk::volk_headers vk-bootstrap::vk-bootstrap SDL3::SDL3-shared SDL3_ttf::SDL3_ttf-shared SDL3_image::SDL3_image-shared GPUOpen::VulkanMemoryAllocator Vulkan::shaderc_combined glm::glm)
target_link_options(GarDEADner PRIVATE -Wl,-rpath=$ORIGIN)
