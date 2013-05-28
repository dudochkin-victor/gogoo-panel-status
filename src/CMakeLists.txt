SET(VERSION_MAJOR 0)
SET(VERSION_MINOR 0)
SET(VERSION_PATCH 0)

INCLUDE_DIRECTORIES(
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_BINARY_DIR}
    ${MPL_INCLUDE_DIRS}
    ${WNCK_INCLUDE_DIRS}
    ${SW_INCLUDE_DIRS}
    ${CLUTTER_GTK_INCLUDE_DIRS}
    ${GCONF_INCLUDE_DIRS}
    ${CHAMPLAIN_INCLUDE_DIRS}
    ${GEOCLUE_INCLUDE_DIRS}
    ${NM_INCLUDE_DIRS}
    )

SET (LIBRARY oma-panel-status)

SET(LIBRARY_SRCS 
	penge-magic-texture.c 
	penge-clickable-label.c 
	mps-tweet-card.c 
	mps-view-bridge.c 
	mps-feed-pane.c 
	mps-feed-switcher.c 
	mps-geotag-pane.c 
	sw-online.c 
    )

SET(MARSHALS
        ${CMAKE_CURRENT_BINARY_DIR}/sw-marshals.h
        ${CMAKE_CURRENT_BINARY_DIR}/sw-marshals.c
)

########### Add marshal target ###############
ADD_CUSTOM_COMMAND(
   OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/sw-marshals.h
   COMMAND glib-genmarshal --prefix=sw_marshal --header ${CMAKE_SOURCE_DIR}/src/sw-marshals.list > ${CMAKE_CURRENT_BINARY_DIR}/sw-marshals.h
   )

ADD_CUSTOM_COMMAND(
   OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/sw-marshals.c
   COMMAND glib-genmarshal --prefix=sw_marshal --body ${CMAKE_SOURCE_DIR}/src/sw-marshals.list --body > ${CMAKE_CURRENT_BINARY_DIR}/sw-marshals.c
   DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/sw-marshals.h
   )


#ADD_DEFINITIONS(-DPKGDATADIR="$(pkgdatadir)")
#ADD_DEFINITIONS(-DMWB_VERSION_STRING="$(MWB_VERSION)")

ADD_LIBRARY(${LIBRARY} ${LIBRARY_SRCS} ${MARSHALS})

SET_TARGET_PROPERTIES( ${LIBRARY} PROPERTIES
     SOVERSION 0
     VERSION ${VERSION_MAJOR}.${VERSION_MINOR}.${VERSION_PATCH}
   )

TARGET_LINK_LIBRARIES(${LIBRARY}
    ${MPL_LIBRARIES}
    ${CHAMPLAIN_LIBRARIES}
    ${GCONF_LIBRARIES}
    ${GEOCLUE_LIBRARIES}
    ${SW_LIBRARIES}
    ${NM_LIBRARIES}
    ${CLUTTER_GTK_LIBRARIES}
    ${GCONF_LIBRARIES}
    )
INSTALL(TARGETS ${LIBRARY} DESTINATION ${PKG_LIBDIR})

SET (PROGRAMS gogoo-panel-status)

SET(SERVICES_DIR ${PKG_LIBDIR}/gogoo-panel-status/services)

ADD_DEFINITIONS(-DLOCALEDIR="${LOCALEDIR}")
ADD_DEFINITIONS(-DMX_CACHE="${DATADIR}/mx.cache")
ADD_DEFINITIONS(-DTHEMEDIR="${PKG_DATADIR}/${PROGRAMS}/theme")
ADD_DEFINITIONS(-DGETTEXT_PACKAGE="${GETTEXT_PACKAGE}")
ADD_DEFINITIONS(-DDATADIR="${PKG_DATADIR}/${PROGRAMS}")
ADD_DEFINITIONS(-DSERVICES_MODULES_DIR="${SERVICES_DIR}")
ADD_DEFINITIONS(-DWITH_ONLINE_NM)

SET(PROGRAM_SRCS
    meego-panel-status.c
    )

SET(PROGRAM_HEADERS
    )

ADD_EXECUTABLE(${PROGRAMS} ${PROGRAM_SRCS} ${PROGRAM_HEADERS})

   
TARGET_LINK_LIBRARIES(${PROGRAMS}
    oma-panel-status
    )

INSTALL(TARGETS ${PROGRAMS} RUNTIME DESTINATION ${PKG_LIBEXECDIR})

########### Add .service target ###############
CONFIGURE_FILE(
  "${CMAKE_CURRENT_SOURCE_DIR}/com.meego.UX.Shell.Panels.status.service.in"
  "${CMAKE_CURRENT_BINARY_DIR}/com.meego.UX.Shell.Panels.status.service"
  IMMEDIATE @ONLY)
INSTALL(FILES ${CMAKE_CURRENT_BINARY_DIR}/com.meego.UX.Shell.Panels.zones.service DESTINATION share/dbus-1/services)

########### Add .pc target ###############
CONFIGURE_FILE(
  "${CMAKE_CURRENT_SOURCE_DIR}/gogoo-panel-status.pc.in"
  "${CMAKE_CURRENT_BINARY_DIR}/gogoo-panel-status.pc"
  IMMEDIATE @ONLY)
INSTALL(FILES ${CMAKE_CURRENT_BINARY_DIR}/gogoo-panel-status.pc DESTINATION ${PKG_LIBDIR}/pkgconfig)


