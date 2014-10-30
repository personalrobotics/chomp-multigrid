cmake_minimum_required(VERSION 2.8.3)

set(DEMO_LIBRARIES
  mzcommon
  chomp
  ${PNG_LIBRARIES}
  ${CAIRO_LIBRARIES}
)

include_directories(
  "${PROJECT_SOURCE_DIR}/include"
  "${PROJECT_SOURCE_DIR}/include/chomp-multigrid"
)

add_library(chomp
  Constraint.cpp
  ConstraintFactory.cpp
  Chomp.cpp
)

add_executable(testmatops testmatops.cpp)
target_link_libraries(testmatops chomp)

add_executable(circle_demo circle_demo.cpp)
target_link_libraries(circle_demo ${DEMO_LIBRARIES})

add_executable(map2d_demo map2d_demo.cpp Map2D.cpp)
target_link_libraries(map2d_demo ${DEMO_LIBRARIES})

install(TARGETS chomp testmatops circle_demo map2d_demo
  ARCHIVE DESTINATION "${CATKIN_PACKAGE_LIB_DESTINATION}"
  LIBRARY DESTINATION "${CATKIN_PACKAGE_LIB_DESTINATION}"
  RUNTIME DESTINATION "${CATKIN_PACKAGE_BIN_DESTINATION}"
)
