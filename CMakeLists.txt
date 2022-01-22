cmake_minimum_required (VERSION 3.5)
project(imguizmo VERSION 0.1.34 LANGUAGES CXX C)

find_package(stb REQUIRED)
find_package(imgui REQUIRED)

add_library(imguizmo STATIC "")
### Add includes
add_subdirectory(include/ImGuizmo)
### Add src files
target_include_directories(imguizmo PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/include>
    $<INSTALL_INTERFACE:include>)
target_link_libraries(imguizmo PUBLIC imgui::imgui stb::stb)
set_target_properties(imguizmo PROPERTIES POSITION_INDEPENDENT_CODE ON)
add_library(imguizmo::imguizmo ALIAS imguizmo)

# Installation stuff (for Hydra)
include(CMakePackageConfigHelpers)
write_basic_package_version_file(
  "${PROJECT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake"
  VERSION ${PROJECT_VERSION}
  COMPATIBILITY AnyNewerVersion
)
configure_package_config_file(
  "${PROJECT_SOURCE_DIR}/${PROJECT_NAME}Config.cmake.in"
  "${PROJECT_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
  INSTALL_DESTINATION "${CMAKE_INSTALL_DATADIR}/cmake/${PROJECT_NAME}"
  )

if(EXPORT_hydra_engine)
  export(TARGETS ${PROJECT_NAME} FILE ${PROJECT_NAME}Targets.cmake)
  export(PACKAGE ${PROJECT_NAME})
else(EXPORT_hydra_engine)
  install(TARGETS ${PROJECT_NAME} EXPORT hydra_engine-export
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib
        COMPONENT library
  )
endif(EXPORT_hydra_engine)
