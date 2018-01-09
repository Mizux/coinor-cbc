find_package(CoinUtils)
find_package(Osi)

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
	src/CbcOrClpParam.hpp
	src/Idiot.hpp)

add_library(Clp ${_SRCS} ${_HDRS})
target_include_directories(Clp PUBLIC
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
	$<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}>)
target_compile_definitions(Clp PRIVATE
	CBC_THREAD_SAFE	CBC_NO_INTERRUPT
	CLP_BUILD HAVE_CONFIG_H)
if(WIN32)
	target_compile_definitions(Clp _CRT_SECURE_NO_WARNINGS _CRT_SECURE_NO_DEPRECATE)
endif ()
target_link_libraries(Clp PUBLIC
	CoinUtils::CoinUtils
	Osi::Osi)
set_target_properties(Clp PROPERTIES
	VERSION ${VERSION})
add_library(Clp::Clp ALIAS Clp)

# OsiClp
set(_SRCS src/OsiClp/OsiClpSolverInterface.cpp)
set(_HDRS	src/OsiClp/OsiClpSolverInterface.hpp)

add_library(OsiClp ${_SRCS} ${_HDRS})
target_include_directories(OsiClp PUBLIC
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src/OsiClp>)
#target_compile_definitions(OsiClp PRIVATE ...)
if(WIN32)
	target_compile_definitions(OsiClp _CRT_SECURE_NO_WARNINGS _CRT_SECURE_NO_DEPRECATE)
endif ()
target_link_libraries(OsiClp PUBLIC
	CoinUtils::CoinUtils
	Osi::Osi
	Clp::Clp)
set_target_properties(OsiClp PROPERTIES
	VERSION ${VERSION})
add_library(OsiClp::OsiClp ALIAS OsiClp)
