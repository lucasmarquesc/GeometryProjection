cmake_minimum_required(VERSION 4.1)
project(projection2)

set(CMAKE_CXX_STANDARD 17)

find_package(OpenGL REQUIRED)
find_package(glfw3 REQUIRED)

add_executable(projection2
        main.cpp
        /home/unir3/external/glad/src/glad.c
)

target_include_directories(projection2 PRIVATE
        /home/unir3/external/glad/include
)

target_link_libraries(projection2 PRIVATE
        OpenGL::GL
        glfw
)