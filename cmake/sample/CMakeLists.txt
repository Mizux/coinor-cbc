cmake_minimum_required(VERSION 3.8.2)
project(Sample VERSION 1.0.0 LANGUAGES CXX)

find_package(ZLIB REQUIRED)
find_package(Cbc REQUIRED)

add_executable(sample main.cpp)
target_compile_features(sample PUBLIC cxx_std_11)
set_target_properties(sample PROPERTIES VERSION ${PROJECT_VERSION})
target_link_libraries(sample PUBLIC
	Cbc::OsiCbc	Cbc::CbcSolver
	Clp::OsiClp	Clp::ClpSolver
	)

include(GNUInstallDirs)
install(TARGETS sample
	EXPORT SampleTargets
	DESTINATION ${CMAKE_INSTALL_BINDIR})
