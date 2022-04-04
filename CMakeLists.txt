cmake_minimum_required(VERSION 3.11)

project(LudumDare50
  LANGUAGES CXX
  VERSION 3.0.0
)


# --- Fetch FTXUI --------------------------------------------------------------
include(FetchContent)

set(FETCHCONTENT_UPDATES_DISCONNECTED TRUE)
FetchContent_Declare(ftxui
  GIT_REPOSITORY https://github.com/ArthurSonzogni/ftxui
  GIT_TAG v0.11
)

FetchContent_GetProperties(ftxui)
if(NOT ftxui_POPULATED)
  FetchContent_Populate(ftxui)
  add_subdirectory(${ftxui_SOURCE_DIR} ${ftxui_BINARY_DIR} EXCLUDE_FROM_ALL)
endif()

# ------------------------------------------------------------------------------

project(LudumDare50
  LANGUAGES CXX
  VERSION 1.0.0
)

add_executable(LudumDare50 main.cpp)
# target_include_directories(LudumDare50 PRIVATE src)

# set_target_properties(LudumDare50 PROPERTIES LINK_FLAGS "--shared-memory --no-check-features")
# -Wl,--shared-memory,--no-check-features     https://github.com/emscripten-core/emscripten/issues/8503

target_link_libraries(LudumDare50
  PRIVATE ftxui::screen
  PRIVATE ftxui::dom
  PRIVATE ftxui::component # Not needed for this example.
)

install(TARGETS LudumDare50 RUNTIME DESTINATION "bin")

