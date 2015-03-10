#OGRE
#########################################################
#find Ogre
if(EXISTS "/usr/local/lib/OGRE/cmake")
	set(OGRE_PLUGIN_DIR  "/usr/local/lib/OGRE" )
	set(CMAKE_MODULE_PATH "${OGRE_PLUGIN_DIR}/cmake/;${CMAKE_MODULE_PATH}")
elseif(EXISTS "/usr/lib/OGRE/cmake")
	set(OGRE_PLUGIN_DIR  "/usr/lib/OGRE" )
	set(CMAKE_MODULE_PATH "${OGRE_PLUGIN_DIR}/cmake/;${CMAKE_MODULE_PATH}")
elseif(EXISTS "/usr/share/OGRE/cmake/modules")
    if (EXISTS "/usr/lib/x86_64-linux-gnu/OGRE-1.8.1" )
        set(OGRE_PLUGIN_DIR  "/usr/lib/x86_64-linux-gnu/OGRE-1.8.1" )
    else()
        set(OGRE_PLUGIN_DIR  "/usr/lib/i386-linux-gnu/OGRE-1.8.1" )
    endif()
set(CMAKE_MODULE_PATH "/usr/share/OGRE/cmake/modules;${CMAKE_MODULE_PATH}")
elseif(EXISTS "/usr/lib64/OGRE/cmake") #für openSuse. Bitte drin lassen
    if (EXISTS "/usr/lib64" )
        set(OGRE_PLUGIN_DIR  "/usr/lib64" )
    endif()
    set(CMAKE_MODULE_PATH "/usr/lib64/OGRE/cmake;${CMAKE_MODULE_PATH}")
else ()
	set(SKIP_OGRE 1)
endif()

if (NOT SKIP_OGRE)
	find_package(OGRE)
#DO we really need that?
#	configure_file(config/plugins.cfg.in plugins.cfg )

	# OIS
	find_package(OIS)

	# OpenGL
	find_package(OpenGL)
endif()

# Setup GFX Libraries
#TODO remove unused libraries
if (NOT (OGRE_FOUND AND OIS_FOUND))
	add_definitions("-DNO_GRAPHICS")
        message ( WARNING "No Graphics Enabled")
else()
	add_definitions("-DGRAPHICS")
	add_definitions(-DOGRE_PLUGIN_DIR_REL="${OGRE_PLUGIN_DIR_REL}")
	add_definitions(-DOGRE_PLUGIN_DIR_DBG="${OGRE_PLUGIN_DIR_DBG}")
	message ( "Graphics Enabled: Ogre Libs: ${OGRE_LIBRARIES} path [${OGRE_LIBRARY_PATH}] plugin Dir: ${OGRE_PLUGIN_DIR}")

#TODO link them manually using target_link_libraries
        set(OGRE_GL
                ${OGRE_LIBRARIES}
                ${OPENGL_LIBRARY}
                ${OIS_LIBRARIES}
                Xxf86vm
                pthread
                Xcursor
                X11
                boost_system
    )
	link_libraries (
		${OGRE_LIBRARIES}
		${OPENGL_LIBRARY}
		${OIS_LIBRARIES}
		Xxf86vm
		pthread
		Xcursor
		X11
		boost_system
	)
        include_directories(${OGRE_INCLUDE_DIRS} )
endif()
