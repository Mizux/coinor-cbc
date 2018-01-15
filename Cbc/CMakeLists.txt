message(STATUS "Subproject: Cbc")

find_package(CoinUtils REQUIRED)
find_package(Osi REQUIRED)
find_package(Clp REQUIRED)
find_package(Cgl REQUIRED)

set(NAME "CBC")
parse_ac(VERSION MAJOR MINOR PATCH)

set(COIN_${NAME}_CHECKLEVEL "0" CACHE INTERNAL
	"${NAME} check level")
set(COIN_${NAME}_VERBOSITY "0" CACHE INTERNAL
	"${NAME} verbosity level")
configure_file(config.h.cmake.in config.h)
configure_file(config_cbc.h.cmake.in config_cbc.h)

set(_SRCS
	#src/CbcBranchBase.cpp
	src/CbcBranchAllDifferent.cpp
	src/CbcBranchCut.cpp
	src/CbcBranchDecision.cpp
	src/CbcBranchDefaultDecision.cpp
	src/CbcBranchDynamic.cpp
	src/CbcBranchingObject.cpp
	src/CbcBranchLotsize.cpp
	src/CbcBranchToFixLots.cpp
	src/CbcClique.cpp
	src/CbcCompareDefault.cpp
	src/CbcCompareDepth.cpp
	src/CbcCompareEstimate.cpp
	src/CbcCompareObjective.cpp
	src/CbcConsequence.cpp
	src/CbcCountRowCut.cpp
	src/CbcCutGenerator.cpp
	src/CbcCutModifier.cpp
	src/CbcCutSubsetModifier.cpp
	src/CbcDummyBranchingObject.cpp
	src/CbcEventHandler.cpp
	src/CbcFathom.cpp
	src/CbcFathomDynamicProgramming.cpp
	src/CbcFixVariable.cpp
	src/CbcFollowOn.cpp
	src/CbcFullNodeInfo.cpp
	src/CbcGenBaB.cpp
	src/CbcGenCbcParam.cpp
	src/CbcGenCbcParamUtils.cpp
	src/CbcGenCtlBlk.cpp
	src/CbcGeneral.cpp
	src/CbcGeneralDepth.cpp
	src/CbcGenMessages.cpp
	src/CbcGenOsiParam.cpp
	src/CbcGenOsiParamUtils.cpp
	src/CbcGenParam.cpp
	src/CbcGenParamUtils.cpp
	src/CbcGenSolution.cpp
	src/CbcGenSolvers.cpp
	src/CbcHeuristic.cpp
	src/CbcHeuristicDINS.cpp
	src/CbcHeuristicDiveCoefficient.cpp
	src/CbcHeuristicDive.cpp
	src/CbcHeuristicDiveFractional.cpp
	src/CbcHeuristicDiveGuided.cpp
	src/CbcHeuristicDiveLineSearch.cpp
	src/CbcHeuristicDivePseudoCost.cpp
	src/CbcHeuristicDiveVectorLength.cpp
	src/CbcHeuristicDW.cpp
	src/CbcHeuristicFPump.cpp
	src/CbcHeuristicGreedy.cpp
	src/CbcHeuristicLocal.cpp
	src/CbcHeuristicPivotAndFix.cpp
	src/CbcHeuristicRandRound.cpp
	src/CbcHeuristicRENS.cpp
	src/CbcHeuristicRINS.cpp
	src/CbcHeuristicVND.cpp
	src/CbcMessage.cpp
	src/CbcModel.cpp
	src/CbcNode.cpp
	src/CbcNodeInfo.cpp
	src/CbcNWay.cpp
	src/CbcObject.cpp
	src/CbcObjectUpdateData.cpp
	src/CbcParam.cpp
	src/CbcPartialNodeInfo.cpp
	src/CbcSimpleInteger.cpp
	src/CbcSimpleIntegerDynamicPseudoCost.cpp
	src/CbcSimpleIntegerPseudoCost.cpp
	src/CbcSOS.cpp
	src/CbcStatistics.cpp
	src/CbcStrategy.cpp
	src/CbcSubProblem.cpp
	src/CbcThread.cpp
	src/CbcTree.cpp
	src/CbcTreeLocal.cpp)

set(_HDRS
	src/CbcConfig.h
	src/CbcBranchActual.hpp
	src/CbcBranchAllDifferent.hpp
	src/CbcBranchBase.hpp
	src/CbcBranchDynamic.hpp
	src/CbcBranchCut.hpp
	src/CbcBranchDecision.hpp
	src/CbcBranchDefaultDecision.hpp
	src/CbcBranchingObject.hpp
	src/CbcBranchLotsize.hpp
	src/CbcBranchToFixLots.hpp
	src/CbcCompareActual.hpp
	src/CbcCompareBase.hpp
	src/CbcCompareDefault.hpp
	src/CbcCompareDepth.hpp
	src/CbcCompareEstimate.hpp
	src/CbcCompareObjective.hpp
	src/CbcConsequence.hpp
	src/CbcClique.hpp
	src/CbcCompare.hpp
	src/CbcCountRowCut.hpp
	src/CbcCutGenerator.hpp
	src/CbcCutModifier.hpp
	src/CbcCutSubsetModifier.hpp
	src/CbcDummyBranchingObject.hpp
	src/CbcFathom.hpp
	src/CbcEventHandler.hpp
	src/CbcFathomDynamicProgramming.hpp
	src/CbcFeasibilityBase.hpp
	src/CbcFixVariable.hpp
	src/CbcFollowOn.hpp
	src/CbcFullNodeInfo.hpp
	src/CbcGeneral.hpp
	src/CbcGeneralDepth.hpp
	src/CbcHeuristic.hpp
	src/CbcHeuristicDINS.hpp
	src/CbcHeuristicDive.hpp
	src/CbcHeuristicDiveCoefficient.hpp
	src/CbcHeuristicDiveFractional.hpp
	src/CbcHeuristicDiveGuided.hpp
	src/CbcHeuristicDiveLineSearch.hpp
	src/CbcHeuristicDivePseudoCost.hpp
	src/CbcHeuristicDiveVectorLength.hpp
	src/CbcHeuristicDW.hpp
	src/CbcHeuristicFPump.hpp
	src/CbcHeuristicGreedy.hpp
	src/CbcHeuristicLocal.hpp
	src/CbcHeuristicPivotAndFix.hpp
	src/CbcHeuristicRandRound.hpp
	src/CbcHeuristicRENS.hpp
	src/CbcHeuristicRINS.hpp
	src/CbcHeuristicVND.hpp
	src/CbcMessage.hpp
	src/CbcModel.hpp
	src/CbcNode.hpp
	src/CbcNodeInfo.hpp
	src/CbcNWay.hpp
	src/CbcObject.hpp
	src/CbcObjectUpdateData.hpp
	src/CbcPartialNodeInfo.hpp
	src/CbcSimpleInteger.hpp
	src/CbcSimpleIntegerDynamicPseudoCost.hpp
	src/CbcSimpleIntegerPseudoCost.hpp
	src/CbcStrategy.hpp
	src/CbcSOS.hpp
	src/CbcSubProblem.hpp
	src/CbcTree.hpp
	src/CbcTreeLocal.hpp
	src/ClpConstraintAmpl.hpp
	src/ClpAmplObjective.hpp)

add_library(Cbc ${_SRCS} ${_HDRS})
target_include_directories(Cbc PUBLIC
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
	$<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}>)
target_compile_definitions(Cbc
	PUBLIC HAVE_CONFIG_H
	PRIVATE CBC_BUILD)
if(WIN32)
	target_compile_definitions(Cbc PRIVATE _CRT_SECURE_NO_WARNINGS _CRT_SECURE_NO_DEPRECATE)
endif()
target_compile_features(Cbc PUBLIC cxx_std_11)
target_link_libraries(Cbc PUBLIC
	CoinUtils::CoinUtils
	Osi::Osi
	Clp::Clp
	Cgl::Cgl)
set_target_properties(Cbc PROPERTIES
	PUBLIC_HEADER "${_HDRS};${CMAKE_CURRENT_BINARY_DIR}/config_cbc.h"
	VERSION ${VERSION})
add_library(Cbc::Cbc ALIAS Cbc)

# Install
include(GNUInstallDirs)
install(TARGETS Cbc
	INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
	PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/coin/
	ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
	)

##############
##  OsiCbc  ##
##############
set(_SRCS src/OsiCbc/OsiCbcSolverInterface.cpp)
set(_HDRS	src/OsiCbc/OsiCbcSolverInterface.hpp)

add_library(OsiCbc ${_SRCS} ${_HDRS})
target_include_directories(OsiCbc PUBLIC
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src/OsiCbc>)
#target_compile_definitions(OsiCbc PRIVATE ...)
if(WIN32)
	target_compile_definitions(OsiCbc PRIVATE _CRT_SECURE_NO_WARNINGS _CRT_SECURE_NO_DEPRECATE)
endif()
target_compile_features(OsiCbc PUBLIC cxx_std_11)
target_link_libraries(OsiCbc PUBLIC
	CoinUtils::CoinUtils
	Osi::Osi
	Cbc::Cbc)
set_target_properties(OsiCbc PROPERTIES
	PUBLIC_HEADER "${_HDRS}"
	VERSION ${VERSION})
add_library(OsiCbc::OsiCbc ALIAS OsiCbc)

# Install
include(GNUInstallDirs)
install(TARGETS OsiCbc
	INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
	PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/coin/
	ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
	)

#################
##  CbcSolver  ##
#################
set(_SRCS
	src/Cbc_C_Interface.cpp
	src/CbcCbcParam.cpp
	src/Cbc_ampl.cpp
	src/CbcLinked.cpp
	src/CbcLinkedUtils.cpp
	src/unitTestClp.cpp
	src/CbcSolver.cpp
	src/CbcSolverHeuristics.cpp
	src/CbcSolverAnalyze.cpp
	src/CbcMipStartIO.cpp
	src/CbcSolverExpandKnapsack.cpp)

set(_HDRS
	src/Cbc_C_Interface.h
	src/Cbc_ampl.h
	src/CbcLinked.hpp
	src/CbcSolver.hpp
	src/CbcMipStartIO.hpp)

add_library(CbcSolver ${_SRCS} ${_HDRS})
target_include_directories(CbcSolver PUBLIC
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>)
target_compile_definitions(CbcSolver
	PRIVATE CBC_THREAD_SAFE CBC_NO_INTERRUPT)
if(WIN32)
	target_compile_definitions(CbcSolver PRIVATE _CRT_SECURE_NO_WARNINGS _CRT_SECURE_NO_DEPRECATE)
endif()
target_compile_features(CbcSolver PUBLIC cxx_std_11)
target_link_libraries(CbcSolver PUBLIC
	Cbc::Cbc)
set_target_properties(CbcSolver PROPERTIES
	PUBLIC_HEADER "${_HDRS}"
	VERSION ${VERSION})
add_library(CbcSolver::CbcSolver ALIAS CbcSolver)

# Install
include(GNUInstallDirs)
install(TARGETS CbcSolver
	INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
	PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/coin/
	ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
	)
