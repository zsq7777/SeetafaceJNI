cmake_minimum_required(VERSION 3.18.1)

project("seetafacejni")

# Opencv 配置
## 添加opencv
set(OpenCV_DIR ${CMAKE_SOURCE_DIR}/opencv-mobile-4.5.4-android/sdk/native/jni)

## 该目录下包含OpenCVConfig.cmake文件
find_package(OpenCV REQUIRED)

# 弃用 添加头文件搜索路径
#include_directories(${OpenCV_INCLUDE_DIRS})

# ncnn

## 添加ncnn
set(ncnn_DIR ${CMAKE_SOURCE_DIR}/ncnn-20220420-android-vulkan/${ANDROID_ABI}/lib/cmake/ncnn)
## 该目录下包含ncnnConfig.cmake文件
find_package(ncnn REQUIRED)




# seetaface6

## 添加头文件搜索路径
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/seetaface6/include)
## 添加动态库搜索文件
link_directories(${CMAKE_CURRENT_SOURCE_DIR}/seetaface6/jniLibs/${ANDROID_ABI})


add_library(
        seetafacejni # 生成SO库名 libseetafacejni.so

        SHARED # shard 共享库/动态函数库/windws上叫动态加载函数库dll  库的类型

        native-lib.cpp # 也可以添加其他文件进入动态库
        ${DIR_SRCS}  # seetaface6
)



# 日志库
find_library(
        log-lib # 别名
        log # 实际的
)




#add_library( seetaface SHARED ${DIR_SRCS})




#OpenGL库
#EGL
#GLESv2
#android

target_link_libraries(
        seetafacejni # 导入的C++库
        ${OpenCV_LIBS}  # opencv
        ncnn #ncnn
        SeetaFaceDetector600 # seetaface6
        TenniS
        SeetaAuthorize

        ${log-lib} # 引入别名
)