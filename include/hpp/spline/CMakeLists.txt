set(HEADERS_INCLUDE_DIR
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>$<INSTALL_INTERFACE:include/hpp/spline>)

SET(${PROJECT_NAME}_HEADERS
  ${HEADERS_INCLUDE_DIR}/bernstein.h
  ${HEADERS_INCLUDE_DIR}/bezier_polynom_conversion.h
  ${HEADERS_INCLUDE_DIR}/curve_abc.h
  ${HEADERS_INCLUDE_DIR}/exact_cubic.h
  ${HEADERS_INCLUDE_DIR}/MathDefs.h
  ${HEADERS_INCLUDE_DIR}/polynom.h
  ${HEADERS_INCLUDE_DIR}/spline_deriv_constraint.h
  ${HEADERS_INCLUDE_DIR}/bezier_curve.h
  ${HEADERS_INCLUDE_DIR}/cubic_spline.h
  ${HEADERS_INCLUDE_DIR}/curve_constraint.h
  ${HEADERS_INCLUDE_DIR}/quintic_spline.h
)

set(${PROJECT_NAME}_HELPERS_HEADERS
  ${HEADERS_INCLUDE_DIR}/helpers/effector_spline.h
  ${HEADERS_INCLUDE_DIR}/helpers/effector_spline_rotation.h
)

add_library(hpp-spline INTERFACE)
if(COMMAND target_sources)
  target_sources(hpp-spline INTERFACE ${${PROJECT_NAME}_HEADERS})
endif()
if(TARGET Eigen3::Eigen)
  target_link_libraries(hpp-spline INTERFACE Eigen3::Eigen)
else()
  target_include_directories(hpp-spline SYSTEM INTERFACE "${EIGEN3_INCLUDE_DIR}")
endif()
target_include_directories(hpp-spline INTERFACE $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}> $<INSTALL_INTERFACE:include>)

install(
    TARGETS hpp-spline
    EXPORT "${TARGETS_EXPORT_NAME}"
)

install(FILES
  ${${PROJECT_NAME}_HEADERS}
  DESTINATION include/hpp/spline
)

install(DIRECTORY helpers DESTINATION include/hpp/spline FILES_MATCHING PATTERN "*.h")

