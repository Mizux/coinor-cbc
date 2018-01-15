message(STATUS "Subproject: Clp")

find_package(CoinUtils REQUIRED)
find_package(Osi REQUIRED)

set(NAME "CLP")
parse_ac(VERSION MAJOR MINOR PATCH)

set(COIN_${NAME}_CHECKLEVEL "0" CACHE INTERNAL
	"${NAME} check level")
set(COIN_${NAME}_VERBOSITY "0" CACHE INTERNAL
	"${NAME} verbosity level")
configure_file(config.h.cmake.in config.h)
configure_file(config_clp.h.cmake.in config_clp.h)

set(_SRCS
	src/ClpCholeskyBase.cpp
	src/ClpCholeskyDense.cpp
	src/ClpConstraint.cpp
	src/ClpConstraintLinear.cpp
	src/ClpConstraintQuadratic.cpp
	src/Clp_C_Interface.cpp
	src/ClpDualRowDantzig.cpp
	src/ClpDualRowPivot.cpp
	src/ClpDualRowSteepest.cpp
	src/ClpDummyMatrix.cpp
	src/ClpDynamicExampleMatrix.cpp
	src/ClpDynamicMatrix.cpp
	src/ClpEventHandler.cpp
	src/ClpFactorization.cpp
	src/ClpGubDynamicMatrix.cpp
	src/ClpGubMatrix.cpp
	src/ClpHelperFunctions.cpp
	src/ClpInterior.cpp
	src/ClpLinearObjective.cpp
	src/ClpMatrixBase.cpp
	src/ClpMessage.cpp
	src/ClpModel.cpp
	src/ClpNetworkBasis.cpp
	src/ClpNetworkMatrix.cpp
	src/ClpNonLinearCost.cpp
	src/ClpNode.cpp
	src/ClpObjective.cpp
	src/ClpPackedMatrix.cpp
	src/ClpPlusMinusOneMatrix.cpp
	src/ClpPredictorCorrector.cpp
	src/ClpPdco.cpp
	src/ClpPdcoBase.cpp
	src/ClpLsqr.cpp
	src/ClpPresolve.cpp
	src/ClpPrimalColumnDantzig.cpp
	src/ClpPrimalColumnPivot.cpp
	src/ClpPrimalColumnSteepest.cpp
	src/ClpQuadraticObjective.cpp
	src/ClpSimplex.cpp
	src/ClpSimplexDual.cpp
	src/ClpSimplexNonlinear.cpp
	src/ClpSimplexOther.cpp
	src/ClpSimplexPrimal.cpp
	src/ClpSolve.cpp
	src/Idiot.cpp
	src/IdiSolve.cpp)

set(_HDRS
	src/Clp_C_Interface.h
	src/ClpConfig.h
	src/ClpCholeskyBase.hpp
	src/ClpCholeskyDense.hpp
	src/ClpConstraint.hpp
	src/ClpConstraintLinear.hpp
	src/ClpConstraintQuadratic.hpp
	src/ClpDualRowDantzig.hpp
	src/ClpDualRowPivot.hpp
	src/ClpDualRowSteepest.hpp
	src/ClpDummyMatrix.hpp
	src/ClpDynamicExampleMatrix.hpp
	src/ClpDynamicMatrix.hpp
	src/ClpEventHandler.hpp
	src/ClpFactorization.hpp
	src/ClpGubDynamicMatrix.hpp
	src/ClpGubMatrix.hpp
	src/ClpInterior.hpp
	src/ClpLinearObjective.hpp
	src/ClpMatrixBase.hpp
	src/ClpMessage.hpp
	src/ClpModel.hpp
	src/ClpNetworkMatrix.hpp
	src/ClpNonLinearCost.hpp
	src/ClpNode.hpp
	src/ClpObjective.hpp
	src/ClpPackedMatrix.hpp
	src/ClpPdcoBase.hpp
	src/ClpPlusMinusOneMatrix.hpp
	src/ClpParameters.hpp
	src/ClpPresolve.hpp
	src/ClpPrimalColumnDantzig.hpp
	src/ClpPrimalColumnPivot.hpp
	src/ClpPrimalColumnSteepest.hpp
	src/ClpQuadraticObjective.hpp
	src/ClpSimplex.hpp
	src/ClpSimplexNonlinear.hpp
	src/ClpSimplexOther.hpp
	src/ClpSimplexDual.hpp
	src/ClpSimplexPrimal.hpp
	src/ClpSolve.hpp
	src/Idiot.hpp)

add_library(Clp ${_SRCS} ${_HDRS})
target_include_directories(Clp PUBLIC
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
	$<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}>
	$<INSTALL_INTERFACE:include/coin>)
target_compile_definitions(Clp
	PUBLIC HAVE_CONFIG_H
	PRIVATE CLP_BUILD)
if(WIN32)
	target_compile_definitions(Clp PRIVATE _CRT_SECURE_NO_WARNINGS _CRT_SECURE_NO_DEPRECATE)
endif()
target_compile_features(Clp PUBLIC cxx_std_11)
target_link_libraries(Clp PUBLIC
	CoinUtils::CoinUtils
	Osi::Osi)
set_target_properties(Clp PROPERTIES
	PUBLIC_HEADER "${_HDRS};${CMAKE_CURRENT_BINARY_DIR}/config_clp.h"
	VERSION ${VERSION})
add_library(Clp::Clp ALIAS Clp)

# Install
include(GNUInstallDirs)
install(TARGETS Clp
	INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
	PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/coin/
	ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
	)

##############
##  OsiClp  ##
##############
set(_SRCS src/OsiClp/OsiClpSolverInterface.cpp)
set(_HDRS	src/OsiClp/OsiClpSolverInterface.hpp)

add_library(OsiClp ${_SRCS} ${_HDRS})
target_include_directories(OsiClp PUBLIC
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src/OsiClp>)
#target_compile_definitions(OsiClp PRIVATE ...)
if(WIN32)
	target_compile_definitions(OsiClp PRIVATE _CRT_SECURE_NO_WARNINGS _CRT_SECURE_NO_DEPRECATE)
endif()
target_compile_features(OsiClp PUBLIC cxx_std_11)
target_link_libraries(OsiClp PUBLIC
	CoinUtils::CoinUtils
	Osi::Osi
	Clp::Clp)
set_target_properties(OsiClp PROPERTIES
	PUBLIC_HEADER "${_HDRS}"
	VERSION ${VERSION})
add_library(Clp::OsiClp ALIAS OsiClp)

# Install
include(GNUInstallDirs)
install(TARGETS OsiClp
	INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
	PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/coin/
	ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
	)

#################
##  ClpSolver  ##
#################
set(_SRCS
	src/ClpSolver.cpp
	src/CbcOrClpParam.cpp
	src/Clp_ampl.cpp
	src/MyEventHandler.cpp
	src/MyMessageHandler.cpp)

set(_HDRS
	src/CbcOrClpParam.hpp
	src/Clp_ampl.h
	src/MyEventHandler.hpp
	src/MyMessageHandler.hpp
	src/unitTest.cpp)

add_library(ClpSolver ${_SRCS} ${_HDRS})
target_include_directories(ClpSolver PUBLIC
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>)
#target_compile_definitions(ClpSolver PRIVATE ...)
if(WIN32)
	target_compile_definitions(ClpSolver PRIVATE _CRT_SECURE_NO_WARNINGS _CRT_SECURE_NO_DEPRECATE)
endif()
target_compile_features(ClpSolver PUBLIC cxx_std_11)
target_link_libraries(ClpSolver PUBLIC
	Clp::Clp)
set_target_properties(ClpSolver PROPERTIES
	PUBLIC_HEADER "${_HDRS}"
	VERSION ${VERSION})
add_library(Clp::ClpSolver ALIAS ClpSolver)

# Install
include(GNUInstallDirs)
install(TARGETS ClpSolver
	INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
	PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/coin/
	ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
	)
