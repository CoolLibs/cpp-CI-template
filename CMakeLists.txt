cmake_minimum_required(VERSION 3.8)

add_library(cpp_CI_template)
add_library(cpp_CI_template::cpp_CI_template ALIAS cpp_CI_template)
target_compile_features(cpp_CI_template PUBLIC cxx_std_20)

# ---Add source files---
target_include_directories(cpp_CI_template PUBLIC include)
target_sources(cpp_CI_template PRIVATE
    src/cpp_CI_template.cpp
)

# ---Setup warnings---
if (CPP_CI_TEMPLATE_ENABLE_WARNINGS_AS_ERRORS)
    message("-- [cpp_CI_template] Enabling warnings as errors for cpp_CI_template")
    if(MSVC)
        target_compile_options(cpp_CI_template PRIVATE /WX /W4)
    else()
        target_compile_options(cpp_CI_template PRIVATE -Werror -Wall -Wextra -Wpedantic -pedantic-errors -Wconversion -Wsign-conversion)
    endif()
else()
    message("-- [cpp_CI_template] Not using warnings as errors for cpp_CI_template")
endif()
