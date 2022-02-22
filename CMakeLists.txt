cmake_minimum_required(VERSION 3.1)
project(mc_1dof_robot_sample)

find_package(mc_rtc REQUIRED)

include(CMakePackageConfigHelpers)
set(VERSION_CONFIG "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake")
set(PROJECT_CONFIG "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake")
set(CONFIG_INSTALL_DIR "lib/cmake/${PROJECT_NAME}")
write_basic_package_version_file(
  ${VERSION_CONFIG}
  VERSION 1.0.0
  COMPATIBILITY SameMajorVersion
)
configure_package_config_file("${CMAKE_CURRENT_SOURCE_DIR}/cmake/Config.cmake.in" "${PROJECT_CONFIG}" INSTALL_DESTINATION "${CONFIG_INSTALL_DIR}")
install(FILES "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake" "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake" DESTINATION "${CONFIG_INSTALL_DIR}")

set(CATKIN_PACKAGE_SHARE_DESTINATION "${CMAKE_INSTALL_PREFIX}/share/mc_rtc/${PROJECT_NAME}")

install(DIRECTORY convex/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/convex)
install(DIRECTORY meshes/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/meshes)
install(DIRECTORY rsdf/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/rsdf)

macro(configure_and_install FILE_IN FILE_OUT DESTINATION)
  configure_file(${FILE_IN} "${CMAKE_CURRENT_BINARY_DIR}/${FILE_OUT}" @ONLY)
  install(FILES "${CMAKE_CURRENT_BINARY_DIR}/${FILE_OUT}"
    DESTINATION "${DESTINATION}")
endmacro()

configure_and_install(urdf/1dof.in.urdf urdf/1dof.urdf "${CATKIN_PACKAGE_SHARE_DESTINATION}/urdf")

configure_and_install(module/alias.in.yaml module/1dof.yaml "${MC_ROBOTS_ALIASES_DIRECTORY}")
#configure_and_install(module/module.in.yaml module/module.yaml "${CATKIN_PACKAGE_SHARE_DESTINATION}/module")

