add_library(rocks_kvstore STATIC rocks_kvstore.cpp rocks_kvttlcompactfilter.cpp)
target_link_libraries(rocks_kvstore utils_common kvstore rocksdb record glog ${SYS_LIBS} snappy lz4_static)

add_library(rocks_kvstore_for_test STATIC rocks_kvstore.cpp rocks_kvttlcompactfilter.cpp)
target_compile_definitions(rocks_kvstore_for_test PRIVATE -DNO_VERSIONEP)
target_link_libraries(rocks_kvstore_for_test utils_common kvstore rocksdb record glog ${SYS_LIBS} snappy lz4_static)

add_executable(rocks_kvstore_test rocks_kvstore_test.cpp)

target_link_libraries(rocks_kvstore_test utils_common kvstore rocksdb
	session_ctx session lock server -Wl,--whole-archive commands -Wl,--no-whole-archive
    rocks_kvstore server_params record gtest_main ${STDFS_LIB} ${SYS_LIBS} snappy lz4_static)
