# imgui test executable, with full warnings enabled
add_executable(test_imgui test.cpp)
if(ENABLE_CONAN)
target_link_libraries(test_imgui PRIVATE project_warnings project_options CONAN_PKG::imgui-sfml)
else()
include(FetchContent)
set(SFML_VERSION 2.5.1)
set(IMGUI_VERSION 1.76)
set(IMGUI_SFML_VERSION 2.1)
if(LINK_DEPS_STATIC)
  set(BUILD_SHARED_LIBS OFF)
else()
  set(BUILD_SHARED_LIBS ON)
endif()
# It's nice to get stripped-down release zips instead of cloning
# a repo and checking out a tag
FetchContent_Declare(
  SFML
  URL "https://github.com/SFML/SFML/archive/${SFML_VERSION}.zip"
)

FetchContent_Declare(
  imgui
  URL "https://github.com/ocornut/imgui/archive/v${IMGUI_VERSION}.zip"
)

# Or by tag...
FetchContent_Declare(
  imgui-sfml
  GIT_REPOSITORY https://github.com/eliasdaler/imgui-sfml.git
  GIT_TAG        master
  # GIT_COMMIT 5f54b69b6aee18db846c81633f86f78c2586dded
  # ^ or like this - sometimes it's better because FetchContent won't look
  # into remote to see if branch head was updated or not - good for stable
  # tags like 'vX.X' corresponding to releases
)

include(cmake/SFML.cmake)
include(cmake/ImGui.cmake)
include(cmake/ImGui-SFML.cmake)
#target_include_directories(${SOURCE_DIR} ${IMGUI_PATH})
target_link_libraries(test_imgui PRIVATE project_warnings project_options
            ImGui-SFML::ImGui-SFML
        )
endif()
