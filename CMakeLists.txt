cmake_minimum_required(VERSION 3.10.0)

project(test_vst
    VERSION 0.0.1
    LANGUAGES C CXX)

add_subdirectory(vst3sdk)

set(plug_sources
    include/plugcontroller.h
    include/plugids.h
    include/plugprocessor.h
    include/version.h
    source/plugfactory.cpp
    source/plugcontroller.cpp
    source/plugprocessor.cpp
)

#--- HERE change the target Name for your plug-in (for ex. set(target myDelay))-------
set(target test_plugin)

# These variables are declared in the vst3sdk CMakeLists.txt, but they are
# not propogated up to this scope. But some of them are used by
# smtg_add_vst3plugin, so we have to redefine them here
set(ROOT "${CMAKE_CURRENT_SOURCE_DIR}/vst3sdk")
set(SDK_ROOT "${ROOT}")
set(public_sdk_SOURCE_DIR ${SDK_ROOT}/public.sdk)
set(pluginterfaces_SOURCE_DIR ${SDK_ROOT}/pluginterfaces)

smtg_add_vst3plugin(${target} ${plug_sources})

target_link_libraries(${target}
    PRIVATE
        sdk
)

if(SMTG_MAC)
    smtg_target_set_bundle(${target} INFOPLIST "${CMAKE_CURRENT_LIST_DIR}/resource/Info.plist" PREPROCESS)
elseif(SMTG_WIN)
    target_sources(${target}
        PRIVATE
            resource/plug.rc
    )
endif(SMTG_MAC)
