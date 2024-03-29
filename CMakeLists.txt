cmake_minimum_required(VERSION 3.12)

set(PROJECT_NAME ConwaysLife)

project(${PROJECT_NAME})

set(HEADER_FILES LifeSimulator.hpp Renderer.hpp RendererConsole.hpp Pattern.hpp PatternAcorn.hpp PatternBlinker.hpp PatternBlock.hpp PatternGlider.hpp PatternGosperGliderGun.hpp)
set(SOURCE_FILES LifeSimulator.cpp RendererConsole.cpp PatternAcorn.cpp PatternBlinker.cpp PatternBlock.cpp PatternGlider.cpp PatternGosperGliderGun.cpp)

add_executable(${PROJECT_NAME} ${SOURCE_FILES} ${HEADER_FILES} main.cpp)

set_property(TARGET ${PROJECT_NAME} PROPERTY CXX_STANDARD 20)

if (CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
    target_compile_options(${PROJECT_NAME} PRIVATE /W4 /permissive-)
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
    target_compile_options(${PROJECT_NAME} PRIVATE -Wall -Wextra -pedantic)
elseif (CMAKE_CXX_COMPILER_ID MATCHES "Clang")
	target_compile_options(${PROJECT_NAME} PRIVATE -Wall -Wextra -pedantic)
endif()

find_program(CLANG_FORMAT "clang-format")
if (CLANG_FORMAT)
	message("CLANG-FORMAT found at: " ${CLANG_FORMAT})
	unset(SOURCE_FILES_PATHS)
	foreach(SOURCE_FILE ${SOURCE_FILES} ${HEADER_FILES} ${UNIT_TEST_FILES} main.cpp)
		get_source_file_property(WHERE ${SOURCE_FILE} LOCATION)
		set(SOURCE_FILES_PATHS ${SOURCE_FILES_PATHS} ${WHERE})
	endforeach()

	add_custom_target(
		ClangFormat
		COMMAND ${CLANG_FORMAT}
		-i
		-style=file
		${SOURCE_FILES_PATHS})

	add_dependencies(${PROJECT_NAME} ClangFormat)
else()
	message("Unable to find Clang-Format")
endif()
