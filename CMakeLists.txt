cmake_minimum_required(VERSION 3.0.2)
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE "Release" CACHE STRING
		"Choose the type of build, options are: Debug Release RelWithDebInfo MinSizeRel."
		FORCE)
endif(NOT CMAKE_BUILD_TYPE)

# Not a subproject -> add find_package macro
if ("^${CMAKE_SOURCE_DIR}$" STREQUAL "^${CMAKE_CURRENT_SOURCE_DIR}$")
	# Use find_package everywhere, no-op if it's a target
	macro(find_package)
		if(NOT TARGET ${ARGV0} AND NOT TARGET ${ARGV0}::${ARGV0})
			_find_package(${ARGV})
		else()
			if(TARGET ${ARGV0})
				get_target_property(TGT_VER ${ARGV0} VERSION)
				set(TGT ${ARGV0})
			else()
				get_target_property(TGT_VER ${ARGV0}::${ARGV0} VERSION)
				set(TGT ${ARGV0}::${ARGV0})
			endif()
			message(STATUS "Found ${ARGV0}: CMake Target ${TGT} (found version \"${TGT_VER}\")")
			set(${ARGV0}_FOUND TRUE)
		endif()
	endmacro()
endif()

project(Cbc-meta VERSION 2.9.9 LANGUAGES C CXX)
message(STATUS "Meta Project: Cbc")

include(CTest)
include(ParseAc)

# ZLIB
find_package(ZLIB)
if(ZLIB_FOUND)
	set(HAVE_ZLIB_H "1" CACHE INTERNAL "Use zlib")
	set(COIN_HAS_ZLIB "1" CACHE INTERNAL "Use zlib")
endif()

# PThread
set(THREADS_PREFER_PTHREAD_FLAG ON)
find_package(Threads)
if(CMAKE_USE_PTHREADS_INIT)
	set(PTHREADS_FOUND TRUE)
else()
	set(PTHREADS_FOUND FALSE)
endif()

# Flags
include (CheckIncludeFile)
check_include_file(math.h HAVE_MATH_H)
check_include_file(ctype.h HAVE_CTYPE_H)
check_include_file(inttypes.h HAVE_INTTYPES_H)
check_include_file(float.h HAVE_FLOAT_H)
check_include_file(ieeefp.h HAVE_IEEEFP_H)
check_include_file(stdarg.h HAVE_STDARG_H)
check_include_file(stddef.h HAVE_STDDEF_H)
check_include_file(stdint.h HAVE_STDINT_H)
check_include_file(stdio.h HAVE_STDIO_H)
check_include_file(stdlib.h HAVE_STDLIB_H)
check_include_file(assert.h HAVE_ASSERT_H)
check_include_file(dlfcn.h HAVE_DLFCN_H)
check_include_file(endian.h HAVE_ENDIAN_H)
check_include_file(memory.h HAVE_MEMORY_H)
check_include_file(strings.h HAVE_STRINGS_H)
check_include_file(string.h HAVE_STRING_H)
check_include_file(time.h HAVE_TIME_H)
check_include_file(unistd.h HAVE_UNISTD_H)
check_include_file(sys/stat.h HAVE_SYS_STAT_H)
check_include_file(sys/types.h HAVE_SYS_TYPES_H)

include (CheckIncludeFileCXX)
check_include_file_cxx(cmath HAVE_CMATH)
check_include_file_cxx(cctype HAVE_CCTYPE)
check_include_file_cxx(cinttypes HAVE_CINTTYPES)
check_include_file_cxx(cfloat HAVE_CFLOAT)
check_include_file_cxx(cieeefp HAVE_CIEEEFP)
check_include_file_cxx(cstdarg HAVE_CSTDARG)
check_include_file_cxx(cstddef HAVE_CSTDDEF)
check_include_file_cxx(cstdint HAVE_CSTDINT)
check_include_file_cxx(cstdio HAVE_CSTDIO)
check_include_file_cxx(cstdlib HAVE_CSTDLIB)
check_include_file_cxx(cassert HAVE_CASSERT)
check_include_file_cxx(cstring HAVE_CSTRING)
check_include_file_cxx(ctime HAVE_CTIME)

set(STDC_HEADERS 1 CACHE INTERNAL "System has ANSI C header files")

set(TEST_INCLUDES "")
if(HAVE_CMATH)
	list(APPEND TEST_INCLUDES "cmath")
endif()
if(HAVE_CFLOAT)
	list(APPEND TEST_INCLUDES "cfloat")
endif()
if(HAVE_CIEEEFP)
	list(APPEND TEST_INCLUDES "cieeefp")
endif()
if(HAVE_MATH_H)
	list(APPEND TEST_INCLUDES "math.h")
endif()
if(HAVE_FLOAT_H)
	list(APPEND TEST_INCLUDES "float.h")
endif()
if(HAVE_IEEEFP_H)
	list(APPEND TEST_INCLUDES "ieeefp.h")
endif()

# ISFINITE
include(CheckCXXSourceCompiles)
check_cxx_source_compiles(
  "#include <cmath>\nint main(){return std::isfinite(0);}"
  HAVE_STD_ISFINITE)
include(CheckFunctionExists)
include(CheckCXXSymbolExists)
check_cxx_symbol_exists(isfinite "${TEST_INCLUDES}" HAVE_ISFINITE)
check_cxx_symbol_exists(finite   "${TEST_INCLUDES}" HAVE_FINITE)
check_cxx_symbol_exists(_finite  "${TEST_INCLUDES}" HAVE__FINITE)
check_cxx_symbol_exists(__finite "${TEST_INCLUDES}" HAVE___FINITE)
if(HAVE_STD_ISFINITE)
	set(COIN_C_FINITE "std::isfinite")
elseif(HAVE_ISFINITE)
	set(COIN_C_FINITE "isfinite")
elseif(HAVE_FINITE)
	set(COIN_C_FINITE "finite")
elseif(HAVE__FINITE)
	set(COIN_C_FINITE "_finite")
elseif(HAVE___FINITE)
	set(COIN_C_FINITE "__finite")
else()
	message(FATAL_ERROR "Can't find isfinite()")
endif()
message(STATUS "Found isfinite: ${COIN_C_FINITE}")

# ISNAN
include(CheckCXXSourceCompiles)
check_cxx_source_compiles(
  "#include <cmath>\nint main(){return std::isnan(0);}"
  HAVE_STD_ISNAN)
include(CheckFunctionExists)
include(CheckCXXSymbolExists)
check_cxx_symbol_exists(isnan   "${TEST_INCLUDES}" HAVE_ISNAN)
check_cxx_symbol_exists(_isnan  "${TEST_INCLUDES}" HAVE__ISNAN)
check_cxx_symbol_exists(__isnan "${TEST_INCLUDES}" HAVE___ISNAN)
if(HAVE_STD_ISNAN)
	set(COIN_C_ISNAN "std::isnan")
elseif(HAVE_ISNAN)
	set(COIN_C_ISNAN "isnan")
elseif(HAVE__ISNAN)
	set(COIN_C_ISNAN "_isnan")
elseif(HAVE___ISNAN)
	set(COIN_C_ISNAN "__isnan")
else()
	message(FATAL_ERROR "Can't find isnan()")
endif()
message(STATUS "Found isnan: ${COIN_C_ISNAN}")

# Basic type
include(CheckTypeSize)
check_type_size("int64_t" SIZEOF_INT64_T)
check_type_size("long long" SIZEOF_LONG_LONG)
check_type_size("long" SIZEOF_LONG)
check_type_size("uint64_t" SIZEOF_UINT64_T)
check_type_size("unsigned long long" SIZEOF_ULONG_LONG)
check_type_size("unsigned long" SIZEOF_ULONG)
check_type_size("intptr_t" SIZEOF_INTPTR_T)
check_type_size("int *" SIZEOF_INT_P)

if(SIZEOF_INT64_T EQUAL "8")
  set(COIN_INT64_T "int64_t")
elseif(SIZEOF_LONG EQUAL "8")
  set(COIN_INT64_T "long")
elseif(SIZEOF_LONG_LONG EQUAL "8")
  set(COIN_INT64_T "long long")
else()
	message(FATAL_ERROR "Can't find suitable int64_t")
endif()
message(STATUS "Found int64_t: ${COIN_INT64_T}")

if(SIZEOF_UINT64_T EQUAL "8")
  set(COIN_UINT64_T "uint64_t")
elseif(SIZEOF_ULONG EQUAL "8")
  set(COIN_INT64_T "unsigned long")
elseif(SIZEOF_ULONG_LONG EQUAL "8")
  set(COIN_INT64_T "unsigned long long")
else()
	message(FATAL_ERROR "Can't find suitable uint64_t")
endif()
message(STATUS "Found uint64_t: ${COIN_UINT64_T}")

if(SIZEOF_INTPTR_T)
	set(COIN_INTPTR_T "intptr_t")
elseif(SIZEOF_INT_P)
  set(COIN_INTPTR_T "int *")
else()
	message(FATAL_ERROR "Can't find suitable intptr_t")
endif()
message(STATUS "Found intptr_t: ${COIN_INTPTR_T}")

# config options
if(MSVC)
	# Build with multiple processes
	add_definitions(/MP)
	add_definitions(/D_CRT_SECURE_NO_WARNINGS /D_CRT_SECURE_NO_DEPRECATE)
	# MSVC warning suppressions
	add_definitions(
		/wd4018 # 'expression' : signed/unsigned mismatch
		/wd4065 # switch statement contains 'default' but no 'case' labels
		/wd4101 # 'identifier' : unreferenced local variable
		/wd4102 # 'label' : unreferenced label
		/wd4244 # 'conversion' conversion from 'type1' to 'type2', possible loss of data
		/wd4267 # 'var' : conversion from 'size_t' to 'type', possible loss of data
		/wd4309 # 'conversion' : truncation of constant value
		/wd4805 # 'operation' : unsafe mix of type 'type1' and type 'type2' in operation.
		/wd4996 # The compiler encountered a deprecated declaration.
		)
endif()

include(GNUInstallDirs)

add_subdirectory(CoinUtils)
add_subdirectory(Osi)
add_subdirectory(Clp)
add_subdirectory(Cgl)
add_subdirectory(Cbc)

install(EXPORT CbcTargets
	NAMESPACE Cbc::
	DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/Cbc")
include(CMakePackageConfigHelpers)
configure_package_config_file(cmake/CbcConfig.cmake.in
	"${PROJECT_BINARY_DIR}/CbcConfig.cmake"
	INSTALL_DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/Cbc")
write_basic_package_version_file(
	"${PROJECT_BINARY_DIR}/CbcConfigVersion.cmake"
	COMPATIBILITY SameMajorVersion)
install(
	FILES
	"${PROJECT_BINARY_DIR}/CbcConfig.cmake"
	"${PROJECT_BINARY_DIR}/CbcConfigVersion.cmake"
	DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/Cbc"
	COMPONENT Devel)
