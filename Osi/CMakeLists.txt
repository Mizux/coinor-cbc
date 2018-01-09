find_package(CoinUtils)

set(NAME "OSI")
parse_ac(VERSION MAJOR MINOR PATCH)

set(COIN_${NAME}_CHECKLEVEL "0" CACHE INTERNAL
	"${NAME} check level")
set(COIN_${NAME}_VERBOSITY "0" CACHE INTERNAL
	"${NAME} verbosity level")
configure_file(config.h.cmake.in config.h)
configure_file(config_osi.h.cmake.in config_osi.h)

set(_SRCS
	src/Osi/OsiAuxInfo.cpp
	src/Osi/OsiBranchingObject.cpp
	src/Osi/OsiChooseVariable.cpp
	src/Osi/OsiColCut.cpp
	src/Osi/OsiCut.cpp
	src/Osi/OsiCuts.cpp
	src/Osi/OsiNames.cpp
	src/Osi/OsiPresolve.cpp
	src/Osi/OsiRowCut.cpp
	src/Osi/OsiRowCutDebugger.cpp
	src/Osi/OsiSolverBranch.cpp
	src/Osi/OsiSolverInterface.cpp)

set(_HDRS
	src/Osi/OsiAuxInfo.hpp
	src/Osi/OsiBranchingObject.hpp
	src/Osi/OsiChooseVariable.hpp
	src/Osi/OsiColCut.hpp
	src/Osi/OsiCollections.hpp
	src/Osi/OsiCut.hpp
	src/Osi/OsiCuts.hpp
	src/Osi/OsiPresolve.hpp
	src/Osi/OsiRowCut.hpp
	src/Osi/OsiRowCutDebugger.hpp
	src/Osi/OsiSolverBranch.hpp
	src/Osi/OsiSolverInterface.hpp
	src/Osi/OsiSolverParameters.hpp)

add_library(Osi ${_SRCS} ${_HDRS})
target_include_directories(Osi PUBLIC
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src/Osi>
	$<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}>)
target_compile_definitions(Osi PUBLIC
	CBC_THREAD_SAFE	CBC_NO_INTERRUPT
	OSI_BUILD HAVE_CONFIG_H)
if(WIN32)
	target_compile_definitions(Osi _CRT_SECURE_NO_WARNINGS _CRT_SECURE_NO_DEPRECATE)
endif ()
target_link_libraries(Osi PRIVATE
	CoinUtils::CoinUtils)
set_target_properties(Osi PROPERTIES
	VERSION ${VERSION})
add_library(Osi::Osi ALIAS Osi)
