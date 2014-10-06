PROJECT(PropertyBrowser)
SET(QT_MIN_VERSION "4.5.0")
find_package(qt5 COMPONENTS Qt5Widgets REQUIRED)


IF(NOT qt5_FOUND)
  MESSAGE(SEND_ERROR "Failed to find Qt 4.5 or greater.")
ELSE(NOT qt5_FOUND)

#  INCLUDE(${QT_USE_FILE})
  SET(CMAKE_PACKAGE_QTGUI TRUE)

  SET(qtpropertyeditor_SRCS
        src/qtpropertybrowser.cpp
        src/qtpropertymanager.cpp
        src/qteditorfactory.cpp
        src/qtvariantproperty.cpp
        src/qttreepropertybrowser.cpp
        src/qtbuttonpropertybrowser.cpp
        src/qtgroupboxpropertybrowser.cpp
        src/qtpropertybrowserutils.cpp
   )

set(QT_INCLUDE_DIR
    ${Qt5Gui_INCLUDE_DIRS}
    ${Qt5Widgets_INCLUDE_DIRS}
    ${Qt5PrintSupport_INCLUDE_DIRS}
    ${Qt5Svg_INCLUDE_DIRS}
)

  include_directories(${CMAKE_CURRENT_BINARY_DIR}
  ${CMAKE_CURRENT_BINARY_DIR}/src)
  include_directories(src)
  INCLUDE_DIRECTORIES(
   ${QT_INCLUDE_DIR}
  )

qt5_generate_moc(
  src/qtpropertybrowser.h
  ${CMAKE_CURRENT_BINARY_DIR}/moc_qtpropertybrowser.cpp
)
qt5_generate_moc(
  src/qtpropertymanager.h
  ${CMAKE_CURRENT_BINARY_DIR}/moc_qtpropertymanager.cpp
)
qt5_generate_moc(
  src/qteditorfactory.h
  ${CMAKE_CURRENT_BINARY_DIR}/moc_qteditorfactory.cpp
)
qt5_generate_moc(
  src/qtvariantproperty.h
  ${CMAKE_CURRENT_BINARY_DIR}/moc_qtvariantproperty.cpp
)
qt5_generate_moc(
  src/qttreepropertybrowser.h
  ${CMAKE_CURRENT_BINARY_DIR}/moc_qttreepropertybrowser.cpp
)
qt5_generate_moc(
  src/qtbuttonpropertybrowser.h
  ${CMAKE_CURRENT_BINARY_DIR}/moc_qtbuttonpropertybrowser.cpp
)
qt5_generate_moc(
  src/qtgroupboxpropertybrowser.h
  ${CMAKE_CURRENT_BINARY_DIR}/moc_qtgroupboxpropertybrowser.cpp
)
qt5_generate_moc(
  src/qtpropertymanager.cpp
  ${CMAKE_CURRENT_BINARY_DIR}/qtpropertymanager.moc
)
qt5_generate_moc(
  src/qteditorfactory.cpp
  ${CMAKE_CURRENT_BINARY_DIR}/qteditorfactory.moc
)
qt5_generate_moc(
  src/qttreepropertybrowser.cpp
  ${CMAKE_CURRENT_BINARY_DIR}/qttreepropertybrowser.moc
)

set(
  qtpropertyeditor_HEADERS_ONLY_MOC
  ${CMAKE_CURRENT_BINARY_DIR}/moc_qtpropertybrowser.cpp
  ${CMAKE_CURRENT_BINARY_DIR}/moc_qtpropertymanager.cpp
  ${CMAKE_CURRENT_BINARY_DIR}/moc_qteditorfactory.cpp
  ${CMAKE_CURRENT_BINARY_DIR}/moc_qtvariantproperty.cpp
  ${CMAKE_CURRENT_BINARY_DIR}/moc_qttreepropertybrowser.cpp
  ${CMAKE_CURRENT_BINARY_DIR}/moc_qtbuttonpropertybrowser.cpp
  ${CMAKE_CURRENT_BINARY_DIR}/moc_qtgroupboxpropertybrowser.cpp
  ${CMAKE_CURRENT_BINARY_DIR}/qtpropertymanager.moc
  ${CMAKE_CURRENT_BINARY_DIR}/qteditorfactory.moc
  ${CMAKE_CURRENT_BINARY_DIR}/qttreepropertybrowser.moc
)

set_source_files_properties(
  ${qtpropertyeditor_HEADERS_ONLY_MOC}
  PROPERTIES
    HEADER_FILE_ONLY true
)

qt5_generate_moc(
  src/qtpropertybrowserutils_p.h
  ${CMAKE_CURRENT_BINARY_DIR}/moc_qtpropertybrowserutils_p.cpp
)

set(
  qtpropertyeditor_HEADERS_MOC
  ${CMAKE_CURRENT_BINARY_DIR}/moc_qtpropertybrowserutils_p.cpp
)

set(
  qtpropertyeditor_MOC
  ${qtpropertyeditor_HEADERS_MOC}
  ${qtpropertyeditor_HEADERS_ONLY_MOC}
)

qt5_add_resources(
  qtpropertyeditor_RESOURCES
  src/qtpropertybrowser.qrc
)

#set(
#  extension_SRCS
#  main.cpp
#)

#qt5_generate_moc(
#  main.cpp
#  ${CMAKE_CURRENT_SOURCE_DIR}/main.moc
#)

#set(
#  extension_MOC
#  ${CMAKE_CURRENT_SOURCE_DIR}/main.moc
#)

#set_source_files_properties(
#  ${extension_MOC}
#  PROPERTIES
#    HEADER_FILE_ONLY true
#)

  qt5_ADD_RESOURCES(RESOURCE_SRCS src/qtpropertybrowser.qrc)

  #${extension_SRCS}

  SET(SRCS   ${extension_MOC}
  ${qtpropertyeditor_MOC}
  ${qtpropertyeditor_SRCS}
  ${qtpropertyeditor_RESOURCES}
)

  ADD_LIBRARY(PropertyBrowser SHARED ${SRCS})

set(QT_LIBRARIES
  ${Qt5Gui_LIBRARIES}
  ${Qt5Widgets_LIBRARIES}
  ${Qt5PrintSupport_LIBRARIES}
  ${Qt5Svg_LIBRARIES}
)


  TARGET_LINK_LIBRARIES(PropertyBrowser
  ${QT_LIBRARIES}
)

  ADD_DEFINITIONS(${QT_DEFINITIONS})
  ADD_DEFINITIONS(-DQT_DLL)
  ADD_DEFINITIONS(-DQT_QTPROPERTYBROWSER_EXPORT)
  ADD_DEFINITIONS(-DQT_PLUGIN)
  #ADD_DEFINITIONS(-DQT_NO_DEBUG)
  ADD_DEFINITIONS(-DQT_SHARED)

  INSTALL( TARGETS PropertyBrowser LIBRARY DESTINATION lib ${INSTALL_TARGETS_DEFAULT_ARGS} )

  ENDIF(NOT qt5_FOUND)
