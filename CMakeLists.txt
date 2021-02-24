cmake_minimum_required(VERSION 3.13.0)

project(calculator
    LANGUAGES CXX
    DESCRIPTION "Calculator"
)

add_executable(${PROJECT_NAME})

target_include_directories(${PROJECT_NAME} PRIVATE include)

target_sources(${PROJECT_NAME}
PRIVATE
    src/main.cpp
    src/calculator.cpp
)

target_compile_options(${PROJECT_NAME}
PRIVATE
    -Wall
    -Wextra
    -Wuninitialized
)

target_compile_features(${PROJECT_NAME} PRIVATE cxx_std_14)

set_target_properties(${PROJECT_NAME} PROPERTIES
    AUTOMOC ON
    # AUTORCC ON
    # AUTOUIC ON
    # AUTOUIC_SEARCH_PATHS src/forms
)

if (NOT CMAKE_BUILD_TYPE)
    set (build_type release)
else()
    string(TOLOWER ${CMAKE_BUILD_TYPE} build_type)
endif()

if (build_type STREQUAL debug)
    target_compile_definitions(${PROJECT_NAME} PRIVATE DEBUG)
    target_compile_options(${PROJECT_NAME} PRIVATE -g3 -O0 -ggdb3)
elseif (build_type STREQUAL release)
    target_compile_options(${PROJECT_NAME} PRIVATE -g0 -O3)
endif()

find_package(Qt5 COMPONENTS Widgets REQUIRED)

target_link_libraries(${PROJECT_NAME}
    Qt5::Widgets
    ${CMAKE_REQUIRED_LIBRARIES}
    ${ALL_LIBRARIES}
)

string(TOLOWER ${CMAKE_SYSTEM_NAME} system_name)
if (system_name STREQUAL windows)
else()
    target_link_libraries(${PROJECT_NAME} ${CMAKE_DL_LIBS})
endif()

