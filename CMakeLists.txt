cmake_minimum_required(VERSION 2.8)
project(CeresSim3Optimize)

add_compile_options(-std=c++11)

#########################################################################
if (NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE "Release")
  message(STATUS "No build type selected, default to ${CMAKE_BUILD_TYPE}")
endif()

if(CMAKE_BUILD_TYPE STREQUAL "Release")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -O3 -fPIC -march=native")
else(CMAKE_BUILD_TYPE STREQUAL "Debug")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -O0 -fPIC")
endif(CMAKE_BUILD_TYPE STREQUAL "Release")
#########################################################################

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)

find_package(QGLViewer REQUIRED)
find_package(Ceres REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(OpenGL REQUIRED)

if(Qt5_FOUND)
    QT5_WRAP_CPP(UI_SOURCES include/QtScene.h include/MainWidget.h)
    INCLUDE_DIRECTORIES(${Qt5Core_INCLUDE_DIRS} ${Qt5Xml_INCLUDE_DIRS} ${Qt5Gui_INCLUDE_DIRS} ${Qt5Widgets_INCLUDE_DIRS} ${Qt5OpenGL_INCLUDE_DIRS})
    SET(MY_QT_LIBRARIES ${Qt5Widgets_LIBRARIES} ${Qt5Core_LIBRARIES} ${Qt5Gui_LIBRARIES} ${Qt5Xml_LIBRARIES} ${Qt5OpenGL_LIBRARIES})
endif()

include_directories(
  include
  ${GLOG_INCLUDE_DIR}
  ${EIGEN3_INCLUDE_DIRS}
  ${CERES_INCLUDE_DIR}
  ${QGLVIEWER_INCLUDE_DIR}
  ${FREEGLUT_INCLUDE_DIRS}
  ${GLEW_INCLUDE_DIR}
)

set(SOURCE_FILES
  src/QtScene.cc
  src/MainWidget.cc
  src/Sim3Optimizer.cc
  src/main.cc
)

add_executable(${PROJECT_NAME}
  ${SOURCE_FILES}
  ${UI_SOURCES}
)

target_link_libraries(${PROJECT_NAME}
  ${CERES_LIBRARIES}
  ${QGLVIEWER_LIBRARY_RELEASE}
  ${MY_QT_LIBRARIES}
  ${OPENGL_gl_LIBRARY}
  ${OPENGL_glu_LIBRARY}
  glog
  gflags
  pthread
)
