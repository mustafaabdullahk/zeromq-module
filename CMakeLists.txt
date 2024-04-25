# Copyright (c) 2024 Sparse Technology, Mustafa Abdullah Kus
# SPDX-License-Identifier: Apache-2.0

if(CONFIG_ZEROMQ)
    # Enable packed filesystem
    # if(CONFIG_MONGOOSE_ENABLE_PACKED_FS)
    #     add_definitions(-DMG_ENABLE_PACKED_FS=1)
    # endif()

    # assign device type for flash section
    # if(CONFIG_SOC_SERIES_STM32H7X)
    #     add_definitions(-DMG_DEVICE=2)
    # elseif(CONFIG_SOC_SERIES_STM32H5X)
    #     add_definitions(-DMG_DEVICE=1)
    # else()
    #     add_definitions(-DMG_DEVICE=0)
    # endif()

    # we'r set as zephyr from zeromq source
    # #define MG_ARCH_ZEPHYR 7
    add_definitions(-DMG_ARCH=7)

    add_library(ZEROMQ INTERFACE)

    add_compile_definitions(__ZEPHYR__)

    zephyr_include_directories(
        ${ZEPHYR_CURRENT_MODULE_DIR}/libzmq/include
    )

    # target_include_directories(ZEROMQ INTERFACE
    # ${ZEROMQ_DIR}
    # )
    target_link_libraries(ZEROMQ INTERFACE zephyr_interface)

    # target_compile_definitions(ZEROMQ INTERFACE LFS_CONFIG=zephyr_lfs_config.h)
    zephyr_library()

    zephyr_library_sources(
        ${ZEPHYR_CURRENT_MODULE_DIR}/libzmq/src/
    )

    zephyr_library_link_libraries(ZEROMQ)

    zephyr_library_compile_options(-std=gnu99)
endif()
