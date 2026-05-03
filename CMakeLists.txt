cmake_minimum_required(VERSION 3.20.0)

project(rpv-security-guardian
        LANGUAGES C
        DESCRIPTION "rpv lightweight security guardian")

# public security properties

# hash code sequence size of any security artifacts used
set(SECURITY_CONFIG_HASH_SIZE  32 CACHE STRING "Defines the expected size of the hash produced. This must match the output size of the selected hash algorithm (e.g., 32 bytes for SHA-256).")

# lifetime cyclic rotating guarded region 'signature' code sequence size
set(SECURITY_CONFIG_NONCE_SIZE 16 CACHE STRING "Defines the required size of nonces generated. The nonce size must meet the requirements of the cryptographic algorithms in use (e.g., 12 or 16 bytes depending on the scheme).")

# internal cmake properties

set(SECURITY_AUTOGEN_DIRECTORY "${CMAKE_BINARY_DIR}/autogen/security")

# security sources

file(GLOB SECURITY_C_SOURCES CONFIGURE_DEPENDS ./src/*.c)

# config generation

configure_file(./config/sec_conf.h.in ${SECURITY_AUTOGEN_DIRECTORY}/sec_conf.h)

# object library

add_library(rpv-security-guardian OBJECT)

target_compile_features(rpv-security-guardian PRIVATE c_std_23)
target_sources(rpv-security-guardian PRIVATE ${SECURITY_C_SOURCES})

target_include_directories(rpv-security-guardian INTERFACE ./include
                                                           ${SECURITY_AUTOGEN_DIRECTORY})
