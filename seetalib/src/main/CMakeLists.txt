
cmake_minimum_required(VERSION 2.8.0)
set(CMAKE_VERBOSE_MAKEFILE on)

set(ANDROID_NDK_ABI_NAME ${CMAKE_ANDROID_ARCH_ABI})

add_definitions(-DOS_ANDROID)
add_definitions(-D__ANDROID__)
add_definitions(-D__ARM_NEON__)
add_definitions(-D__ARM_NEON)
add_definitions(-DSEETA_EXPORTS)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC -Wno-sign-compare -Wno-unused-parameter")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11 -Wall -O3 -frtti -fPIC -fpermissive -Wno-sign-compare -Wno-unused-parameter")


project(SeetaFace)

include_directories(
    cpp
    cpp/SeetaFace2
    cpp/SeetaFace2/FaceDetector
    cpp/SeetaFace2/FaceLandmarker
    cpp/SeetaFace2/FaceRecognizer
    cpp/SeetaFace2/SeetaNet/src
    cpp/SeetaFace2/SeetaNet/src/include_inner
    cpp/SeetaFace2/SeetaNet/include
)

file(GLOB_RECURSE src_files
    ${CMAKE_CURRENT_SOURCE_DIR}/cpp/jni/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/cpp/SeetaFace2/Seeta/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/cpp/SeetaFace2/FaceDetector/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/cpp/SeetaFace2/FaceLandmarker/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/cpp/SeetaFace2/FaceRecognizer/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/cpp/SeetaFace2/SeetaNet/src/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/cpp/SeetaFace2/SeetaNet/src/orz/mem/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/cpp/SeetaFace2/SeetaNet/src/orz/sync/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/cpp/SeetaFace2/SeetaNet/src/orz/tools/*.cpp
)

add_library(${PROJECT_NAME} SHARED ${src_files})

set(opencv_libs ${CMAKE_CURRENT_SOURCE_DIR}/jniLibs/${ANDROID_ABI}/libopencv_java3.so)

target_link_libraries(SeetaFace android jnigraphics z m dl log atomic ${opencv_libs})
