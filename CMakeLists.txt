cmake_minimum_required(VERSION 3.10 FATAL_ERROR)
project(QtSsh VERSION 0.1)

option(BUILD_STATIC  "Build static library"            OFF)

if(BUILD_STATIC)
    message(STATUS "Build QtSsh static")
else(BUILD_STATIC)
    message(STATUS "Build QtSsh shared")
endif(BUILD_STATIC)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/lib")
set(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)

set(CMAKE_AUTOMOC ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)

find_package(QT NAMES Qt6 Qt5 REQUIRED COMPONENTS Core Network)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Core Network)

set(QT_LIBRARIES Qt${QT_VERSION_MAJOR}::Core Qt${QT_VERSION_MAJOR}::Network)
set(QT_VERSION ${Qt${QT_VERSION_MAJOR}_VERSION})

add_subdirectory(qtssh)

if (WithExamples)
add_subdirectory(examples)
endif()

include(CMakePackageConfigHelpers)
# create Config.cmake
configure_package_config_file(
	config.cmake.in
	"${CMAKE_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
	INSTALL_DESTINATION lib/cmake
	PATH_VARS CMAKE_INSTALL_INCLUDEDIR CMAKE_INSTALL_LIBDIR
)

# create ConfigVersion.cmake
write_basic_package_version_file(
	"${CMAKE_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake"
	VERSION ${PROJECT_VERSION}
	COMPATIBILITY AnyNewerVersion)

# install the *Config.cmake and *ConfigVersion.cmake
install(FILES
	"${CMAKE_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
	"${CMAKE_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake"
	DESTINATION lib/cmake
)
