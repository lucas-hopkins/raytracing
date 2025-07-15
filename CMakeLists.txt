#---------------------------------------------------------------
# CMake Build Configuration for the Ray Tracing Weekend Series
#
# See README.md for guidance
#---------------------------------------------------------------

cmake_minimum_required ( VERSION 3.10...4.0.0 )

project ( RTWeekend LANGUAGES CXX )

# Set to C++11
set ( CMAKE_CXX_STANDARD          11)
set ( CMAKE_CXX_STANDARD_REQUIRED ON)
set ( CMAKE_CXX_EXTENSIONS       OFF)

# Source

set ( EXTERNAL
	src/external/stb_image.h
)

set ( SOURCE_ONE_WEEKEND
	src/raytracer/main.cc
)

include_directories(src)

# Specific compiler flags below. We're going to add options for all possible compilers

message (STATUS "Compiler ID: " $(CMAKE_CXX_COMPILER_ID))
message (STATUS "Release flags: "$(CMAKE_CXX_FLAGS_RELEASE))
message (STATUS "Debug flags: "$(CMAKE_CXX_FLAGS_DEBUG))

if (CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
	# /wd #### - Disable warning
	# /we #### - treat warning as error
	add_compile_options("/W4")  # Enable level-4 warnings
	add_compile_options("/we 4265") # Class has virtual functions, but its non-trivial destructor is not virtual
	add_compile_options("/we 5204") # Class has virtual functions, but its trivial destructor is not virtual
	add_compile_options("/wd 4100") # unreferenced formal parameter
elseif ( CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
	add_compile_options(-Wnon-virtual-dtor) # Class has virtual functions, but its detructor is non virtual
	add_compile_options(-Wreorder) # Data member will be initialized after [other] data member
	add_compile_options(-Wmaybe-uninitialized) # Variable improperly initialized
	add_compile_options(-Wunused-variable) # Variable is defined but unused
elseif ( CMAKE_CXX_COMPILER_ID MATCHES "Clang")
	add_compile_options(-Wnon-virtual-dtor) # Class has virtual functions, but its destructor is non virtual
	add_compile_options(-Wreorder) # Data member will be initialized after [other] data member
	add_compile_options(-Wsometimes-uninitialized) # Variable improperly initialized
	add_compile_options(-Wunuse-variable) # Variable is defined but unused
endif()

# Executables

add_executable(inOneWeekend	${EXTERNAL} ${SOURCE_ONE_WEEKEND})
