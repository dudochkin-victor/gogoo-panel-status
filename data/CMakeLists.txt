ADD_SUBDIRECTORY(theme)

########### Add .desktop target ###############
CONFIGURE_FILE(
  "${CMAKE_CURRENT_SOURCE_DIR}/gogoo-panel-status.desktop.in"
  "${CMAKE_CURRENT_BINARY_DIR}/gogoo-panel-status.desktop"
  IMMEDIATE @ONLY)
INSTALL(FILES ${CMAKE_CURRENT_BINARY_DIR}/gogoo-panel-status.desktop DESTINATION ${DATA_DIR}/panels)


