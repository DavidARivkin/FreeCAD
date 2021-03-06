IF (BUILD_GUI)
    PYSIDE_WRAP_RC(Material_QRC_SRCS Resources/Material.qrc)
ENDIF (BUILD_GUI)

SET(Material_SRCS
    Init.py
    InitGui.py
    Material.py
    importFCMat.py
    MaterialEditor.py
    materials-editor.ui
)
SOURCE_GROUP("Module" FILES ${Material_SRCS})

# collect all the material cards:
#FILE( GLOB MaterialLib_Files ./StandardMaterial/*.FCMat ./StandardMaterial/*.txt  )

SET (MaterialLib_Files
    StandardMaterial/None.FCMat
    StandardMaterial/ABS.FCMat
    StandardMaterial/CalculiX-Steel.FCMat
    StandardMaterial/Concrete-Generic.FCMat
    StandardMaterial/PLA.FCMat
    StandardMaterial/Steel-Generic.FCMat
    StandardMaterial/Wood-Generic.FCMat
    StandardMaterial/Readme.txt
)
SOURCE_GROUP("MatLib" FILES ${MaterialLib_Files})

ADD_CUSTOM_TARGET(Material ALL
    SOURCES ${Material_SRCS} ${Material_QRC_SRCS}
)

fc_target_copy_resource(Material 
    ${CMAKE_SOURCE_DIR}/src/Mod/Material
    ${CMAKE_BINARY_DIR}/Mod/Material
    ${Material_SRCS})

IF (BUILD_GUI)
    fc_target_copy_resource(Material
        ${CMAKE_CURRENT_BINARY_DIR}
        ${CMAKE_BINARY_DIR}/Mod/Material
        Material_rc.py)
ENDIF (BUILD_GUI)

ADD_CUSTOM_TARGET(MaterialLib ALL
    SOURCES ${MaterialLib_Files}
)

# When a target copies files to different output directories then apparently it always builds the project
# which is very annoying. So, the trick is to split this into two targets to avoid this behaviour.
fc_target_copy_resource(MaterialLib 
    ${CMAKE_SOURCE_DIR}/src/Mod/Material
    ${CMAKE_BINARY_DIR}/${CMAKE_INSTALL_DATADIR}/Mod/Material
    ${MaterialLib_Files})

INSTALL(
    FILES ${Material_SRCS} ${Material_QRC_SRCS}
    DESTINATION Mod/Material
)

INSTALL(
    DIRECTORY
        StandardMaterial
    DESTINATION
        ${CMAKE_INSTALL_DATADIR}/Mod/Material
    FILES_MATCHING PATTERN "*.FCMat*"
)
