cmake_minimum_required(VERSION 3.7)

project(kvm-orig)

# Make Targets
add_definitions(-std=c11)
add_custom_target(kvm-orig COMMAND $(MAKE) -C ${kvm-orig_SOURCE_DIR} PWD=${kvm-orig_SOURCE_DIR})

#
# KVM Original Kernel Module (Dec 2006 - 6aa8b732ca01c3d7a54e93f4d701b8aabbe60fb7)
#

file(GLOB MOD_SOURCE_FILES "drivers/kvm/*.c" "drivers/kvm/*.h" "include/linux/*.h")
add_library(dummy_mod ${MOD_SOURCE_FILES})

#set(LINUX_HEADERS_VERSION 4.4.0-83)
#set(LINUX_HEADERS_PATH /usr/src/linux-headers-${LINUX_HEADERS_VERSION})
set(LINUX_DEV_PATH /home/Storage/SSD/Development/Software/eliaskousk/linux)
set(LINUX_PATH ${LINUX_DEV_PATH})
set(LINUX_HEADERS
        ${LINUX_PATH}/arch/x86/include
        ${LINUX_PATH}/arch/x86/include/generated
        ${LINUX_PATH}/arch/x86/include/generated/uapi
        ${LINUX_PATH}/arch/x86/include/uapi
        ${LINUX_PATH}/include
        ${LINUX_PATH}/include/generated
        ${LINUX_PATH}/include/generated/uapi
        ${LINUX_PATH}/include/uapi)
TARGET_INCLUDE_DIRECTORIES(dummy_mod PUBLIC ${LINUX_HEADERS})
TARGET_INCLUDE_DIRECTORIES(dummy_mod PUBLIC include/linux)

TARGET_COMPILE_DEFINITIONS(dummy_mod PUBLIC __KERNEL__)
TARGET_COMPILE_DEFINITIONS(dummy_mod PUBLIC MODULE)
TARGET_COMPILE_DEFINITIONS(dummy_mod PUBLIC CONFIG_X86_64)

TARGET_COMPILE_OPTIONS(dummy_mod PUBLIC -nostdinc)
TARGET_COMPILE_OPTIONS(dummy_mod PUBLIC -isystem /usr/lib/gcc/x86_64-linux-gnu/5/include)
TARGET_COMPILE_OPTIONS(dummy_mod PUBLIC -include ${LINUX_PATH}/include/linux/kconfig.h)
