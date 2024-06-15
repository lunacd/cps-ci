# libcps
add_library(libcps cps/env.cpp cps/loader.cpp cps/printer.cpp cps/search.cpp
                   cps/utils.cpp cps/version.cpp)

set(CMAKE_CXX_STANDARD 23)

# Configure config.hpp
set(functions unreachable)
set(headers utility)
set(BUILTIN_FEATURE_MACROS "")
include(CheckCXXSymbolExists)
foreach(function header IN ZIP_LISTS functions headers)
  string(TOUPPER ${function} function_upper)
  check_cxx_symbol_exists(std::${function} ${header}
                          HAS_BUILTIN_${function_upper})
  if(${HAS_BUILTIN_${function_upper}})
    set(BUILTIN_FEATURE_MACROS
        "${BUILTIN_FEATURE_MACROS}\n#define CPS_USE_BUILTIN_${function_upper} 1"
    )
  else()
    set(BUILTIN_FEATURE_MACROS
        "${BUILTIN_FEATURE_MACROS}\n#define CPS_USE_BUILTIN_${function_upper} 0"
    )
  endif()
endforeach()
configure_file(cps/config.hpp.in cps/config.hpp)

target_include_directories(libcps PUBLIC .)
target_include_directories(libcps PUBLIC "${CMAKE_CURRENT_BINARY_DIR}")

# cps-config
add_executable(cps-config cps-config/main.cpp)
target_link_libraries(cps-config PRIVATE libcps)

# Dependencies
find_package(tl-expected REQUIRED)
target_link_libraries(libcps PRIVATE tl::expected)

find_package(fmt REQUIRED)
target_link_libraries(libcps PRIVATE fmt::fmt)

find_package(jsoncpp REQUIRED)
target_link_libraries(libcps PRIVATE JsonCpp::JsonCpp)

find_package(CLI11 REQUIRED)
target_link_libraries(cps-config PRIVATE CLI11::CLI11)
