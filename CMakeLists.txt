cmake_minimum_required(VERSION 3.1 FATAL_ERROR)

project(NostraGLAD 
    VERSION 1.0.0
    DESCRIPTION "A Nostra-compatible GLAD/OpenGL version."
    LANGUAGES C)

add_library(GLAD SHARED "src/glad.c")

target_include_directories(GLAD 
    PUBLIC 
        "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
        "$<INSTALL_INTERFACE:include>")

target_compile_definitions(GLAD 
    PRIVATE 
        GLAD_GLAPI_EXPORT
        GLAD_GLAPI_EXPORT_BUILD)

install(TARGETS GLAD EXPORT NostraGLADTargets
    RUNTIME  DESTINATION "bin"
    LIBRARY  DESTINATION "lib"
    ARCHIVE  DESTINATION "lib"
    INCLUDES DESTINATION "include")

install(DIRECTORY "include" 
    DESTINATION 
        "include" 
    COMPONENT 
        Develop)

# Have the README.md in the install directory
install(FILES "README.md" DESTINATION ".")
install(FILES "CHANGELOG.md" DESTINATION ".")
install(FILES "LICENSE.md" DESTINATION ".")



include(CMakePackageConfigHelpers)

write_basic_package_version_file("${CMAKE_CURRENT_BINARY_DIR}/NostraGLADConfigVersion.cmake"
	VERSION "${NostraGLAD_VERSION}"
	COMPATIBILITY AnyNewerVersion)

export(EXPORT NostraGLADTargets
	FILE "${CMAKE_CURRENT_BINARY_DIR}/NostraGLADConfig.cmake"
	NAMESPACE Nostra::)

configure_file("cmake/NostraGLADConfig.cmake" "${CMAKE_CURRENT_BINARY_DIR}/NostraGLADConfig.cmake" COPYONLY)

set(CONFIG_PACKAGE_LOCATION "lib/cmake/NostraGLAD")

install(EXPORT NostraGLADTargets
	FILE
		NostraGLADConfig.cmake
	NAMESPACE
		Nostra::
	DESTINATION
		${CONFIG_PACKAGE_LOCATION})

install(
  FILES
    cmake/NostraGLADConfig.cmake
    "${CMAKE_CURRENT_BINARY_DIR}/NostraGLADConfig.cmake"
  DESTINATION
    ${CONFIG_PACKAGE_LOCATION}
  COMPONENT
    Develop
)


configure_file("cmake/CPackConfig.cmake" "cmake/CPackConfig.cmake" @ONLY)

include("${CMAKE_BINARY_DIR}/cmake/CPackConfig.cmake")

include(CPack)