cmake_minimum_required(VERSION 2.8)
project(VideoStab)

set( CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH}
    ${CMAKE_CURRENT_SOURCE_DIR}/cmake_modules 
    )

include_directories( ${CMAKE_SOURCE_DIR}/external/tclap-1.2.1/include/ )
include_directories( ${CMAKE_SOURCE_DIR}/external/easylogging/ )

add_definitions( -std=c++11 -Wall )

set(CMAKE_BUILD_TYPE Release )

find_package( OpenCV REQUIRED )

include_directories( ${OpenCV_INCLUDE_DIR} )
if(OpenCV_VERSION VERSION_GREATER 3.0.0 )
    add_definitions( -DUSE_OPENCV3 )
else()
    add_definitions( -DUSE_OPENCV2 )
endif()

# Either use LIBTIFF or TINYTIFF.
find_package( TIFF REQUIRED )
add_definitions( -DUSE_LIBTIFF )


add_executable(videostab 
    src/main.cpp
    src/videoio.cpp
    src/globals.cpp
    src/stablizer.cpp
    )

#message( STATUS "Found following libraries ${OpenCV_LIBRARIES}" )

target_link_libraries( videostab  
    ${TIFF_LIBRARIES}
    ${OpenCV_LIBRARIES}
    )


install( TARGETS videostab 
    RUNTIME
    DESTINATION bin
    )

