# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/project/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.4.1)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O3 -DNDEBUG -fstrict-aliasing")

include_directories(feature_stab/db_vlvm)
include_directories(feature_stab/src)
include_directories(feature_stab/src/dbreg)
include_directories(feature_mos/src)
include_directories(feature_mos/src/mosaic)

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.

# 配置so库信息
add_library( # Sets the name of the library.
        # 生成的so库名称，此处生成的so文件名称是libfilter-lib.so
        jni_mosaic

        # Sets the library as a shared library.
        # STATIC：静态库，是目标文件的归档文件，在链接其它目标的时候使用
        # SHARED：动态库，会被动态链接，在运行时被加载
        # MODULE：模块库，是不会被链接到其它目标中的插件，但是可能会在运行时使用dlopen-系列的函数动态链接
        SHARED

        # Provides a relative path to your source file(s).
        # 资源文件，可以多个，
        # 资源路径是相对路径，相对于本CMakeLists.txt所在目录
        feature_mos_jni.cpp
        mosaic_renderer_jni.cpp
        feature_mos/src/mosaic/trsMatrix.cpp
        feature_mos/src/mosaic/AlignFeatures.cpp
        feature_mos/src/mosaic/Blend.cpp
        feature_mos/src/mosaic/Delaunay.cpp
        feature_mos/src/mosaic/ImageUtils.cpp
        feature_mos/src/mosaic/Mosaic.cpp
        feature_mos/src/mosaic/Pyramid.cpp
        feature_mos/src/mosaic_renderer/Renderer.cpp
        feature_mos/src/mosaic_renderer/WarpRenderer.cpp
        feature_mos/src/mosaic_renderer/SurfaceTextureRenderer.cpp
        feature_mos/src/mosaic_renderer/YVURenderer.cpp
        feature_mos/src/mosaic_renderer/FrameBuffer.cpp
        feature_stab/db_vlvm/db_feature_detection.cpp
        feature_stab/db_vlvm/db_feature_matching.cpp
        feature_stab/db_vlvm/db_framestitching.cpp
        feature_stab/db_vlvm/db_image_homography.cpp
        feature_stab/db_vlvm/db_rob_image_homography.cpp
        feature_stab/db_vlvm/db_utilities.cpp
        feature_stab/db_vlvm/db_utilities_camera.cpp
        feature_stab/db_vlvm/db_utilities_indexing.cpp
        feature_stab/db_vlvm/db_utilities_linalg.cpp
        feature_stab/db_vlvm/db_utilities_poly.cpp
        feature_stab/src/dbreg/dbreg.cpp
        feature_stab/src/dbreg/dbstabsmooth.cpp
        feature_stab/src/dbreg/vp_motionmodel.c)

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.

# 配置库的链接（依赖关系）(NDK > 19)
target_link_libraries( # Specifies the target library.
        # 目标库
        jni_mosaic

        EGL
        GLESv2
        android
        log

        # 导入ndk(android/bitmap.h)
        jnigraphics)