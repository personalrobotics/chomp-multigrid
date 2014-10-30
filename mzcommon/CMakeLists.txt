cmake_minimum_required(VERSION 2.8.3)

include_directories(
  "${PROJECT_SOURCE_DIR}/include"
  "${PROJECT_SOURCE_DIR}/include/mzcommon"
)

set(mzcommon_srcs
  mersenne.c
  gauss.c
  GlCamera.cpp
  GlFrameGrabber.cpp
  GlTexture2D.cpp
  MzGlutApp.cpp
  TinyDom.cpp
  TriMesh3.cpp
  HeightMap.cpp
  DtGrid.cpp
  Geom2.cpp
  glstuff.cpp
  CCDWrapper.cpp
)
if(GLEW_FOUND)
  set(mzcommon_srcs ${mzcommon_srcs} GlFramebufferObject.cpp)
endif()

# mzcommon
add_library(mzcommon ${mzcommon_srcs})
target_link_libraries(mzcommon
  ${PNG_LIBRARIES}
  ${EXPAT_LIBRARIES}
  ${OPENGL_gl_LIBRARY}
  ${OPENGL_glu_LIBRARY}
  ${GLUT_LIBRARIES}
  ${CCD_LDFLAGS}
  ${CAIRO_LIBRARIES}
)
if (GLEW_FOUND)
  target_link_libraries(mzcommon ${GLEW_LIBRARIES})
endif()

# testwrl
add_executable(testwrl testwrl.cpp)
target_link_libraries(testwrl
  mzcommon
  ${OPENGL_gl_LIBRARY}
  ${OPENGL_glu_LIBRARY}
)

# testdt
add_gui_app(testdt testdt.cpp)
target_link_libraries(testdt
  mzcommon
  ${OPENGL_gl_LIBRARY}
  ${OPENGL_glu_LIBRARY}
  ${GLUT_LIBRARIES}
)

# testgeom2
add_executable(testgeom2 testgeom2.cpp)
if (${CAIRO_FOUND})
  target_link_libraries(testgeom2 mzcommon ${CAIRO_LIBRARIES})
else()
  target_link_libraries(testgeom2 mzcommon)
endif()

# ccddemo
add_gui_app(ccddemo ccddemo.cpp)
target_link_libraries(ccddemo
  mzcommon 
  ${EXPAT_LIBRARIES}
  ${CCD_LDFLAGS}
  ${OPENGL_gl_LIBRARY}
  ${OPENGL_glu_LIBRARY}
  ${GLUT_LIBRARIES}
)

install(TARGETS mzcommon testwrl testdt testgeom2 ccddemo
  ARCHIVE DESTINATION "${CATKIN_PACKAGE_LIB_DESTINATION}"
  LIBRARY DESTINATION "${CATKIN_PACKAGE_LIB_DESTINATION}"
  RUNTIME DESTINATION "${CATKIN_PACKAGE_BIN_DESTINATION}"
)
