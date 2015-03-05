#
# project: fips-vld
#

option(FIPS_USE_VLD "Enable Visual Leak Detector (Windows only)" OFF)
if (FIPS_WINDOWS AND FIPS_MSVC AND FIPS_USE_VLD)
    # create a custom target which copies the precompiled
    # DLLs in lib/[os] to the executable output path
    set(vld_src_dir ${CMAKE_CURRENT_SOURCE_DIR}/lib/${FIPS_PLATFORM_NAME})
    set(vld_dst_dir ${FIPS_DEPLOY_DIR}/${CMAKE_PROJECT_NAME}/${FIPS_CONFIG})
    add_custom_target(vld_copy_dlls ALL
                      COMMAND cmake -E copy_directory ${vld_src_dir} ${vld_dst_dir}
                      COMMENT "Copy VLD DLLs...")
endif()


