set(PROJECTNAME MyGUI.OpenGLESPlatform)

include_directories(
	include
	${MYGUI_SOURCE_DIR}/MyGUIEngine/include
	${OPENGL_INCLUDE_DIR}
	${MYGUI_SOURCE_DIR}/Common
)

include(${PROJECTNAME}.list)
add_definitions(-DGL_GLEXT_PROTOTYPES)
add_library(${PROJECTNAME} STATIC ${HEADER_FILES} ${SOURCE_FILES})

add_dependencies(${PROJECTNAME} MyGUIEngine)

target_link_libraries(${PROJECTNAME} OpenGL::GL MyGUIEngine)

link_directories(${OPENGL_LIB_DIR})

# installation rules
install(FILES ${HEADER_FILES}
	DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}/MYGUI"
)
mygui_install_target(${PROJECTNAME} "")
