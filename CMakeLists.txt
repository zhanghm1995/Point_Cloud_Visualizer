cmake_minimum_required (VERSION 2.8 FATAL_ERROR)
project(pcl_visualizer)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

find_package(PCL 1.7 REQUIRED)
find_package (VTK REQUIRED)
add_definitions(${PCL_DEFINITIONS})

 find_package(Qt5 REQUIRED COMPONENTS Core Xml OpenGL Gui Widgets)
 include_directories(${Qt5Core_INCLUDE_DIRS}
                      ${Qt5Xml_INCLUDE_DIRS}
                      ${Qt5Gui_INCLUDE_DIRS}
                      ${Qt5Widgets_INCLUDE_DIRS}
                      ${Qt5OpenGL_INCLUDE_DIRS})
  set(MY_QT_LIBRARIES
      ${Qt5Widgets_LIBRARIES}
      ${Qt5Core_LIBRARIES}
      ${Qt5Gui_LIBRARIES}
      ${Qt5Xml_LIBRARIES}
      ${Qt5OpenGL_LIBRARIES})
      
# some status messages
message(STATUS "[STATUS] Linking against Qt libs: ${MY_QT_LIBRARIES}")


include_directories(${PCL_INCLUDE_DIRS})

set(QT_SOURCES
    main.cpp
    pclviewer.cpp
    )

set(QT_HEADERS
    pclviewer.h
    )

set(QT_UI_FILES
    pclviewer.ui
    )
    
if (Qt5_FOUND)
  QT5_WRAP_CPP(QT_HEADERS_MOC ${QT_HEADERS})
  QT5_WRAP_UI(QT_UI_FILES_MOC ${QT_UI_FILES})
else()
  message(FATAL_ERROR "Qt not found. So cannot moc.")
endif()


link_directories    (${PCL_LIBRARY_DIRS})


ADD_EXECUTABLE(pcl_visualizer 
            ${QT_SOURCES}
            ${QT_UI_FILES_MOC}
            ${QT_HEADERS_MOC})
TARGET_LINK_LIBRARIES(pcl_visualizer ${MY_QT_LIBRARIES} ${PCL_LIBRARIES} ${VTK_LIBRARIES})
