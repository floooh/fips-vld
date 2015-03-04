#
# project: fips-vld
#

if (NOT FIPS_IMPORT)
    cmake_minimum_required(VERSION 2.8)
    get_filename_component(FIPS_ROOT_DIR "../fips" ABSOLUTE)
    include("${FIPS_ROOT_DIR}/cmake/fips.cmake")
    fips_setup()
    fips_project(fips-vld)
    option(FIPS_USE_VLD "Enable Visual Leak Detector (Windows only)" ON)
else()
    option(FIPS_USE_VLD "Enable Visual Leak Detector (Windows only)" OFF)
endif()

if (FIPS_WINDOWS AND FIPS_MSVC AND FIPS_USE_VLD)
    fips_begin_sharedlib(vld)
        include_directories(vld vld/lib/dbghelp/include vld/setup)
        add_definitions(-D_UNICODE -DUNICODE)
        add_definitions(-D_WIN32_WINNT=0x0502)
        fips_dir(vld)
        fips_files(
            callstack.h
            criticalsection.h
            crtmfcpatch.h
            map.h
            ntapi.h
            resource.h
            set.h
            stdafx.h
            tree.h
            utility.h
            vld.h
            vld_def.h
            vldallocator.h
            vldheap.h
            vldint.h)
        fips_files(
            callstack.cpp
            dllspatches.cpp
            ntapi.cpp
            stdafx.cpp
            utility.cpp
            vld.cpp
            vld_hooks.cpp
            vldapi.cpp
            vldheap.cpp)
        fips_dir(vld/setup)
        fips_files(version.h)
        fips_deps(psapi.lib)
    fips_end_sharedlib()
    # disable some warnings
    set_target_properties(vld PROPERTIES COMPILE_FLAGS "/wd4201 /wd4229")
endif()

if (NOT FIPS_IMPORT)
    fips_finish()
endif()


