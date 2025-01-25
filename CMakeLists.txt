cmake_minimum_required(VERSION 3.30)

project(terathon VERSION 1.0)

set(TerathonContainers_SOURCE_DIR "${CMAKE_SOURCE_DIR}/Terathon-Container-Library")
set(TerathonMath_SOURCE_DIR "${CMAKE_SOURCE_DIR}/Terathon-Math-Library")
set(OpenDDL_SOURCE_DIR "${CMAKE_SOURCE_DIR}/OpenDDL")

set(HEADERS
        "${TerathonContainers_SOURCE_DIR}/TSArray.h"
        "${TerathonContainers_SOURCE_DIR}/TSBasic.h"
        "${TerathonContainers_SOURCE_DIR}/TSGraph.h"
        "${TerathonContainers_SOURCE_DIR}/TSHash.h"
        "${TerathonContainers_SOURCE_DIR}/TSList.h"
        "${TerathonContainers_SOURCE_DIR}/TSMap.h"
        "${TerathonContainers_SOURCE_DIR}/TSPlatform.h"
        "${TerathonContainers_SOURCE_DIR}/TSTree.h"

        "${TerathonMath_SOURCE_DIR}/TSAlgebra.h"
        "${TerathonMath_SOURCE_DIR}/TSBivector3D.h"
        "${TerathonMath_SOURCE_DIR}/TSConformal2D.h"
        "${TerathonMath_SOURCE_DIR}/TSConformal3D.h"
        "${TerathonMath_SOURCE_DIR}/TSFlector3D.h"
        "${TerathonMath_SOURCE_DIR}/TSMath.h"
        "${TerathonMath_SOURCE_DIR}/TSMatrix2D.h"
        "${TerathonMath_SOURCE_DIR}/TSMatrix3D.h"
        "${TerathonMath_SOURCE_DIR}/TSMatrix4D.h"
        "${TerathonMath_SOURCE_DIR}/TSMotor3D.h"
        "${TerathonMath_SOURCE_DIR}/TSQuaternion.h"
        "${TerathonMath_SOURCE_DIR}/TSRigid2D.h"
        "${TerathonMath_SOURCE_DIR}/TSRigid3D.h"
        "${TerathonMath_SOURCE_DIR}/TSSimd.h"
        "${TerathonMath_SOURCE_DIR}/TSVector2D.h"
        "${TerathonMath_SOURCE_DIR}/TSVector3D.h"
        "${TerathonMath_SOURCE_DIR}/TSVector4D.h"

        "${TerathonOpenDDL_SOURCE_DIR}/OpenDDL/Code/TSBivector4D.h"
        "${TerathonOpenDDL_SOURCE_DIR}/OpenDDL/Code/TSColor.h"
        "${TerathonOpenDDL_SOURCE_DIR}/OpenDDL/Code/TSData.h"
        "${TerathonOpenDDL_SOURCE_DIR}/OpenDDL/Code/TSHalf.h"
        "${TerathonOpenDDL_SOURCE_DIR}/OpenDDL/Code/TSMotor4D.h"
        "${TerathonOpenDDL_SOURCE_DIR}/OpenDDL/Code/TSOpenDDL.h"
        "${TerathonOpenDDL_SOURCE_DIR}/OpenDDL/Code/TSString.h"
        "${TerathonOpenDDL_SOURCE_DIR}/OpenDDL/Code/TSText.h"
        "${TerathonOpenDDL_SOURCE_DIR}/OpenDDL/Code/TSTools.h"
        "${TerathonOpenDDL_SOURCE_DIR}/OpenDDL/Code/TSTrivector4D.h"
)

set(SOURCES
        "${TerathonContainers_SOURCE_DIR}/TSGraph.cpp"
        "${TerathonContainers_SOURCE_DIR}/TSHash.cpp"
        "${TerathonContainers_SOURCE_DIR}/TSList.cpp"
        "${TerathonContainers_SOURCE_DIR}/TSMap.cpp"
        "${TerathonContainers_SOURCE_DIR}/TSTree.cpp"

        "${TerathonMath_SOURCE_DIR}/TSAlgebra.cpp"
        "${TerathonMath_SOURCE_DIR}/TSBivector3D.cpp"
        "${TerathonMath_SOURCE_DIR}/TSConformal2D.cpp"
        "${TerathonMath_SOURCE_DIR}/TSConformal3D.cpp"
        "${TerathonMath_SOURCE_DIR}/TSFlector3D.cpp"
        "${TerathonMath_SOURCE_DIR}/TSMath.cpp"
        "${TerathonMath_SOURCE_DIR}/TSMatrix2D.cpp"
        "${TerathonMath_SOURCE_DIR}/TSMatrix3D.cpp"
        "${TerathonMath_SOURCE_DIR}/TSMatrix4D.cpp"
        "${TerathonMath_SOURCE_DIR}/TSMotor3D.cpp"
        "${TerathonMath_SOURCE_DIR}/TSQuaternion.cpp"
        "${TerathonMath_SOURCE_DIR}/TSRigid2D.cpp"
        "${TerathonMath_SOURCE_DIR}/TSRigid3D.cpp"
        "${TerathonMath_SOURCE_DIR}/TSVector2D.cpp"
        "${TerathonMath_SOURCE_DIR}/TSVector3D.cpp"
        "${TerathonMath_SOURCE_DIR}/TSVector4D.cpp"

        "${TerathonOpenDDL_SOURCE_DIR}/OpenDDL/Code/TSBivector4D.cpp"
        "${TerathonOpenDDL_SOURCE_DIR}/OpenDDL/Code/TSColor.cpp"
        "${TerathonOpenDDL_SOURCE_DIR}/OpenDDL/Code/TSData.cpp"
        "${TerathonOpenDDL_SOURCE_DIR}/OpenDDL/Code/TSHalf.cpp"
        "${TerathonOpenDDL_SOURCE_DIR}/OpenDDL/Code/TSMotor4D.cpp"
        "${TerathonOpenDDL_SOURCE_DIR}/OpenDDL/Code/TSOpenDDL.cpp"
        "${TerathonOpenDDL_SOURCE_DIR}/OpenDDL/Code/TSString.cpp"
        "${TerathonOpenDDL_SOURCE_DIR}/OpenDDL/Code/TSText.cpp"
        "${TerathonOpenDDL_SOURCE_DIR}/OpenDDL/Code/TSTools.cpp"
        "${TerathonOpenDDL_SOURCE_DIR}/OpenDDL/Code/TSTrivector4D.cpp"
)

add_library(${PROJECT_NAME} STATIC ${HEADERS} ${SOURCES})

set_target_properties(${PROJECT_NAME} PROPERTIES
        PUBLIC_HEADER "${HEADERS}"
)

# Define the namespace
set(PROJECT_NAMESPACE ${PROJECT_NAME}::)

# Create an alias target in the namespace
add_library(${PROJECT_NAMESPACE}${PROJECT_NAME} ALIAS ${PROJECT_NAME})

include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

target_include_directories(${PROJECT_NAME}
        PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME}>
)

install(TARGETS ${PROJECT_NAME}
        EXPORT ${PROJECT_NAME}-targets
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME}
)

install(EXPORT ${PROJECT_NAME}-targets
        NAMESPACE ${PROJECT_NAMESPACE}
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}
        FILE ${PROJECT_NAME}-config.cmake
)

write_basic_package_version_file(
        ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}-config-version.cmake
        VERSION ${PROJECT_VERSION}
        COMPATIBILITY SameMajorVersion
)

install(FILES
        ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}-config-version.cmake
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}
)

export(EXPORT ${PROJECT_NAME}-targets
        NAMESPACE ${PROJECT_NAMESPACE}
        FILE ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}-config.cmake
)
