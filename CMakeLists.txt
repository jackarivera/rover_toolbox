cmake_minimum_required (VERSION 3.11)

# --- Fetch FTXUI --------------------------------------------------------------
include(FetchContent)

set(FETCHCONTENT_UPDATES_DISCONNECTED TRUE)
FetchContent_Declare(ftxui
  GIT_REPOSITORY https://github.com/ArthurSonzogni/ftxui
  GIT_TAG v4.0.0
)

FetchContent_GetProperties(ftxui)
if(NOT ftxui_POPULATED)
  FetchContent_Populate(ftxui)
  add_subdirectory(${ftxui_SOURCE_DIR} ${ftxui_BINARY_DIR} EXCLUDE_FROM_ALL)
endif()

# ------------------------------------------------------------------------------

find_package(nlohmann_json 3.2.0 REQUIRED)


project(rover_install
  LANGUAGES CXX
  VERSION 1.0.0
)

add_executable(rover_installer 
  src/main.cpp 
  src/UI/logger.cpp 
  src/UI/ros_install_tab.cpp
  src/UI/rover_install_tab.cpp
  src/include/logger.hpp 
  src/include/robot_info.hpp 
  src/include/ui.hpp 
)
target_include_directories(rover_installer PRIVATE src)

target_link_libraries(rover_installer
  PRIVATE ftxui::screen
  PRIVATE ftxui::dom
  PRIVATE ftxui::component
  PRIVATE nlohmann_json::nlohmann_json
)

set_target_properties(rover_installer PROPERTIES CXX_STANDARD 17)