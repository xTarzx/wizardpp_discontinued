cmake_minimum_required(VERSION 3.7...3.27)

if(${CMAKE_VERSION} VERSION_LESS 3.12)
    cmake_policy(VERSION ${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION})
endif()

if (UNIX AND NOT APPLE)
 set (LINUX TRUE)
endif()

project(WiZard VERSION 0.1 LANGUAGES CXX)

set(CMAKE_CXX_FLAGS "-Wall -Werror -std=c++17")


set(WIZLIGHTPP_DIR ${PROJECT_SOURCE_DIR}/extern/wizlightcpp)
set(JANSSON_DIR ${PROJECT_SOURCE_DIR}/extern/jansson/build)
set(IMGUI_DIR ${PROJECT_SOURCE_DIR}/extern/imgui)

set(GLFW_DIR ${PROJECT_SOURCE_DIR}/extern/glfw/)

add_executable(${PROJECT_NAME}
    ${CMAKE_SOURCE_DIR}/src/wizardpp.cpp
    ${CMAKE_SOURCE_DIR}/src/wizard.cpp

    ${WIZLIGHTPP_DIR}/src/log.cpp
    ${WIZLIGHTPP_DIR}/src/udp.cpp
    ${WIZLIGHTPP_DIR}/src/bulb.cpp


    ${IMGUI_DIR}/imgui.cpp
    ${IMGUI_DIR}/imgui_draw.cpp
    ${IMGUI_DIR}/imgui_tables.cpp
    ${IMGUI_DIR}/imgui_widgets.cpp
    ${IMGUI_DIR}/backends/imgui_impl_glfw.cpp
    ${IMGUI_DIR}/backends/imgui_impl_opengl3.cpp


    ${IMGUI_DIR}/imgui_demo.cpp
)

target_include_directories(${PROJECT_NAME} PRIVATE
    ${CMAKE_SOURCE_DIR}/include
    ${WIZLIGHTPP_DIR}/include
    ${IMGUI_DIR}
    ${IMGUI_DIR}/backends

    ${JANSSON_DIR}/include
    ${GLFW_DIR}/include
)

target_link_directories(${PROJECT_NAME} PRIVATE
    ${JANSSON_DIR}/lib
    ${GLFW_DIR}/build/src
)

target_link_libraries(${PROJECT_NAME} jansson)

if (WIN32)
target_link_libraries(${PROJECT_NAME} ws2_32)
target_link_libraries(${PROJECT_NAME} opengl32 glfw3)

elseif(LINUX)
target_link_libraries(${PROJECT_NAME} GL glfw)

endif()
