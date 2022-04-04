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

if(EMSCRIPTEN)
  message("EMSCRIPTEN: Enabled")
  target_link_options(component PUBLIC "SHELL: -s TOTAL_MEMORY=33554432")
  target_link_options(component PUBLIC "SHELL: -s ASSERTIONS=1")
endif()

# if(EMSCRIPTEN)
#   message("EMSCRIPTEN: Enabled")
#   set(CMAKE_CXX_FLAGS  "-O3 -g4 -lwebsocket.js -s ASSERTIONS=2 -s DEMANGLE_SUPPORT=1")

#   set_target_properties(LudumDare50 PROPERTIES SUFFIX ".html")
#   set_target_properties(LudumDare50 PROPERTIES LINK_FLAGS "-s ASYNCIFY -s ALLOW_MEMORY_GROWTH=0 -s USE_PTHREADS=1 -s WASM_MEM_MAX=536870912 -s TOTAL_MEMORY=536870912 -pthread -Wl,--shared-memory,--no-check-features -s PTHREAD_POOL_SIZE=2    -s WASM=1 -s BINARYEN_METHOD='native-wasm'")
# else()
#   message("EMSCRIPTEN: Disabled")
# endif()


install(TARGETS LudumDare50 RUNTIME DESTINATION "bin")