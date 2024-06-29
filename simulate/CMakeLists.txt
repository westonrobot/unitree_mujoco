cmake_minimum_required(VERSION 3.16)
project(unitree_mujoco)

SET(CMAKE_BUILD_TYPE Release)

enable_language(C)
enable_language(CXX)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

list(APPEND CMAKE_PREFIX_PATH "/opt/weston_robot/lib/cmake")

find_package(mujoco REQUIRED)
find_package(unitree_sdk2 REQUIRED)

# include_directories(/usr/local/include/ddscxx /usr/local/include/iceoryx/v2.0.2)
# link_libraries(unitree_sdk2 ddsc ddscxx rt pthread)

FILE (GLOB SIM_SRC
        src/joystick/joystick.cc
        src/mujoco/*.cc
        src/unitree_sdk2_bridge/*.cc)
add_executable(unitree_mujoco ${SIM_SRC} src/main.cc)
target_link_libraries(unitree_mujoco PUBLIC
    pthread
    unitree_sdk2
    mujoco::mujoco
    glfw
    yaml-cpp
    unitree_sdk2)

add_executable(test test/test_unitree_sdk2.cpp)
target_link_libraries(test unitree_sdk2)

add_executable(jstest src/joystick/jstest.cc src/joystick/joystick.cc)
target_link_libraries(jstest pthread)

