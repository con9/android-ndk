#
# Copyright (C) The Android Open Source Project
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

cmake_minimum_required(VERSION 3.4.1)
set(CMAKE_VERBOSE_MAKEFILE on)

# build native_app_glue as a static lib
include_directories(${ANDROID_NDK}/sources/android/native_app_glue)
add_library(app-glue STATIC
            ${ANDROID_NDK}/sources/android/native_app_glue/android_native_app_glue.c)

set(ndk_helper_dir ../../../../common/ndk_helper)
set(ndk_helper_SRCS ${ndk_helper_dir}/gl3stub.c
                    ${ndk_helper_dir}/GLContext.cpp
                    ${ndk_helper_dir}/shader.cpp
                    ${ndk_helper_dir}/vecmath.cpp
                    ${ndk_helper_dir}/gestureDetector.cpp
                    ${ndk_helper_dir}/interpolator.cpp
                    ${ndk_helper_dir}/tapCamera.cpp
                    ${ndk_helper_dir}/JNIHelper.cpp
                    ${ndk_helper_dir}/perfMonitor.cpp)
set(more_teapots_SRCS
                    MoreTeapotsNativeActivity.cpp
                    MoreTeapotsRenderer.cpp
                    ${ndk_helper_SRCS})

include_directories(${ndk_helper_dir}
                    ${ANDROID_NDK}/sources/android/cpufeatures)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11 -Wall -fno-exceptions -fno-rtti")

add_library(MoreTeapotsNativeActivity SHARED ${more_teapots_SRCS})

# add lib dependencies
target_link_libraries(MoreTeapotsNativeActivity android log EGL GLESv2 atomic app-glue)

