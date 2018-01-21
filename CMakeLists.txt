cmake_minimum_required(VERSION 3.9)
project(blockparser)



set(CMAKE_CXX_STANDARD 11)

set(DESTINATION ${PROJECT_NAME}/bin)





INCLUDE_DIRECTORIES(${blockparser_SOURCE_DIR})







set(CB_SOURCE_FILES cb/allBalances.cpp
                    cb/closure.cpp
                    cb/dumpTX.cpp
                    cb/headers.cpp
                    cb/help.cpp
                    cb/pristine.cpp
                    cb/rawdump.cpp
                    cb/rewards.cpp
                    cb/simpleStats.cpp
                    cb/sql.cpp
                    cb/taint.cpp
                    cb/transactions.cpp
                    cb/txoTypes.cpp)





set(CRYPTO_SOURCE_FILES crypto/common_btc.h
                        crypto/endian_btc.h
                        crypto/ripemd160_btc.h
                        crypto/ripemd160_btc.cpp
                        crypto/sha256_btc.h
                        crypto/sha256_btc.cpp)




set(H9_SOURCE_FILES h9/aes_helper.c
                    h9/blake.c
                    h9/bmw.c
                    h9/cubehash.c
                    h9/echo.c
                    h9/fugue.c
                    h9/groestl.c
                    h9/h9.h
                    h9/h13.h
                    h9/hamsi.c
                    h9/hamsi_helper.h
                    h9/jh.c
                    h9/keccak.c
                    h9/luffa.c
                    h9/qubit.h
                    h9/shavite.c
                    h9/simd.c
                    h9/skein.c
                    h9/sph_blake.h
                    h9/sph_bmw.h
                    h9/sph_cubehash.h
                    h9/sph_echo.h
                    h9/sph_fugue.h
                    h9/sph_groestl.h
                    h9/sph_hamsi.h
                    h9/sph_jh.h
                    h9/sph_keccak.h
                    h9/sph_luffa.h
                    h9/sph_shavite.h
                    h9/sph_simd.h
                    h9/sph_skein.h
                    h9/sph_types.h
                    h9/sph_types.h
                    h9/uint256.h)


set(SCRYPT_SOURCE_FILES scrypt/pbkdf2.cpp
                        scrypt/pbkdf2.h
                        scrypt/scrypt.h
                        scrypt/scrypt.cpp)





set(TYPE_SOURCE_FILES t/base58_t.cpp
                      t/ripemd-160_t.cpp
                      t/secp256k1_t.cpp
                      t/sha256_t.cpp)








set(GEOCOIN_SOURCE_FILES
                        ${PROJECT_SOURCE_DIR}/option.cpp
                        ${PROJECT_SOURCE_DIR}/option.h
                        ${PROJECT_SOURCE_DIR}/opcodes.cpp
                        ${PROJECT_SOURCE_DIR}/opcodes.h
                        ${PROJECT_SOURCE_DIR}/errlog.h
                        ${PROJECT_SOURCE_DIR}/common.h
                        ${PROJECT_SOURCE_DIR}/callback.h
                        ${PROJECT_SOURCE_DIR}/callback.cpp
                        ${PROJECT_SOURCE_DIR}/rmd160.cpp
                        ${PROJECT_SOURCE_DIR}/rmd160.h
                        ${PROJECT_SOURCE_DIR}/sha256.cpp
                        ${PROJECT_SOURCE_DIR}/sha256.h
                        ${PROJECT_SOURCE_DIR}/timer.cpp
                        ${PROJECT_SOURCE_DIR}/timer.h
                        ${PROJECT_SOURCE_DIR}/test.cpp
                        ${PROJECT_SOURCE_DIR}/test.h
                        ${PROJECT_SOURCE_DIR}/util.cpp
                        ${PROJECT_SOURCE_DIR}/util.h)




add_definitions(-DGEOCOIN)

add_compile_options(
        -fno-stack-protector
        -O6
        -g0
        -m64
        -Wall
        -flto
        -msse3
        -Wextra
        -Wformat
        -pedantic
        -std=c++11
        -ffast-math
        -march=native
        -fno-check-new
        -funroll-loops
        -Wno-deprecated
        -falign-loops=16
        -fstrict-aliasing
        -Wstrict-aliasing=2
        -Wno-strict-overflow
        -Wformat-security
        -Wno-unused-const-variable
        -Wno-variadic-macros
        -fomit-frame-pointer
        -Wno-unused-parameter
        -finline-limit=1000000
        -Wno-unused-variable
        -Wno-unused-local-typedefs)





add_executable(blockparser parser.cpp ${CB_SOURCE_FILES}
                                  ${CRYPTO_SOURCE_FILES}
                                  ${H9_SOURCE_FILES}
                                  ${SCRYPT_SOURCE_FILES}
                                  ${TYPE_SOURCE_FILES}
                                  ${GEOCOIN_SOURCE_FILES})


#[[
  Note:  Ubuntu 16.04 +
         cmake find scripts
         /usr/share/cmake-3.9/Modules
         look for FindOpenSSL.cmake
         it will show you the variables exposed when finding package
         if you want to query package manager - dpkg with package name it will
         also show what directories the libraries reside in
         $ dpkg -L libssl-dev
         using find_package helps to stay OS independent imo
]]


find_package(OpenSSL)
if(OPENSSL_FOUND)
    target_link_libraries(${PROJECT_NAME} PUBLIC ${OPENSSL_LIBRARIES})
endif(OPENSSL_FOUND)

find_package(Boost)
if(Boost_FOUND)
    include_directories(${Boost_INCLUDE_DIRS})
    target_link_libraries(${PROJECT_NAME} PUBLIC ${Boost_LIBRARY})
endif(Boost_FOUND)