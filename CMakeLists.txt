project (runxdg)

cmake_minimum_required(VERSION 3.0)

set(PROJECT_NAME "runxdg")
set(PROJECT_PRETTY_NAME "Run XDG Application")
set(PROJECT_MAJOR_VERSION "1.0")
set(PROJECT_MINOR_VERSION "1")
set(PROJECT_VERSION "${PROJECT_MAJOR_VERSION}.${PROJECT_MINOR_VERSION}")

add_definitions(-DTARGET_APP_ID=${TARGET_APP_ID})
add_definitions(-DRUNXDG_NAME=${RUNXDG_NAME})

set(CMAKE_CXX_FLAGS "-Wall -fpermissive")

find_package(PkgConfig REQUIRED)
pkg_check_modules(GLIB REQUIRED glib-2.0)
pkg_check_modules(GIO REQUIRED gio-2.0)
pkg_check_modules(GMODULE REQUIRED gmodule-2.0)

INCLUDE(FindPkgConfig)

INCLUDE_DIRECTORIES(
  include
  ${GLIB_INCLUDE_DIRS}
  ${GIO_INCLUDE_DIRS}
  )

LINK_DIRECTORIES(
  ${GLIB_LIBRARY_DIRS}
  ${GIO_LIBRARY_DIRS}
  )

SET(LIBRARIES
  libwindowmanager.so
  libhomescreen.so
  libilmControl.so
  libilmCommon.so
  libafbwsc.so
  libjson-c.so
  libEGL.so
  libGLESv2.so
  libwayland-egl.so
  libwayland-client.so
  libpthread.so
  ${GLIB_LIBRARIES}
  ${GIO_LIBRARIES}
)

SET(SRC_FILES
    src/runxdg.cpp
)

add_executable(${PROJECT_NAME} ${SRC_FILES})

TARGET_LINK_LIBRARIES(${PROJECT_NAME} ${LIBRARIES})

add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
  COMMAND cp -rf ${CMAKE_CURRENT_SOURCE_DIR}/package ${PROJECT_BINARY_DIR})

add_custom_target(widget

  COMMAND wgtpkg-pack -f -o ${PROJECT_BINARY_DIR}/package/simple-egl.wgt ${PROJECT_BINARY_DIR}/package/simple-egl

  # override hvac to test runxdg
  COMMAND wgtpkg-pack -f -o ${PROJECT_BINARY_DIR}/package/hvac.wgt ${PROJECT_BINARY_DIR}/package/hvac

  # override navigation to test runxdg
  COMMAND wgtpkg-pack -f -o ${PROJECT_BINARY_DIR}/package/navi.wgt ${PROJECT_BINARY_DIR}/package/navi
)

install (TARGETS ${PROJECT_NAME} DESTINATION bin)
