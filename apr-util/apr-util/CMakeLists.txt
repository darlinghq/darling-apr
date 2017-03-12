project(apr-util)

cmake_minimum_required(VERSION 2.4.0)

if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 NEW)
endif(COMMAND cmake_policy)

add_definitions(-DTARGET_OS_MAC=1)
add_definitions(-D__APPLE__ -D__DYNAMIC__)
add_definitions(-DHAVE_CONFIG_H -U__linux__ -DDARWIN)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -U_POSIX_C_SOURCE -D__DARWIN_UNIX03 -nostdinc -fblocks -fPIC -ggdb -O0")

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include/private)
include_directories(${DARLING_TOP_DIRECTORY}/src/libc/include/FreeBSD)
include_directories(${DARLING_TOP_DIRECTORY}/src/libc/include/NetBSD)
include_directories(${DARLING_TOP_DIRECTORY}/src/libc/gen)
include_directories(${DARLING_TOP_DIRECTORY}/src/libc/darwin)
include_directories(${DARLING_TOP_DIRECTORY}/src/libc/stdtime/FreeBSD)
include_directories(${DARLING_TOP_DIRECTORY}/src/launchd/liblaunch)
include_directories(${DARLING_TOP_DIRECTORY}/src/external/libdispatch)
include_directories(${DARLING_TOP_DIRECTORY}/src/external/zlib)
include_directories(${DARLING_TOP_DIRECTORY}/src/external/bzip2)
include_directories(${DARLING_TOP_DIRECTORY}/src/external/apr/apr/apr/include)
include_directories(${DARLING_TOP_DIRECTORY}/src/external/sqlite)
include_directories(${DARLING_TOP_DIRECTORY}/src/external/expat/expat/lib)
include_directories(${DARLING_TOP_DIRECTORY}/src/libiconv/include)

set(aprutil_sources
	buckets/apr_brigade.c
	buckets/apr_buckets_alloc.c
	buckets/apr_buckets.c
	buckets/apr_buckets_eos.c
	buckets/apr_buckets_file.c
	buckets/apr_buckets_flush.c
	buckets/apr_buckets_heap.c
	buckets/apr_buckets_mmap.c
	buckets/apr_buckets_pipe.c
	buckets/apr_buckets_pool.c
	buckets/apr_buckets_refcount.c
	buckets/apr_buckets_simple.c
	buckets/apr_buckets_socket.c

	crypto/apr_crypto.c
	crypto/apr_crypto_nss.c
	crypto/apr_crypto_openssl.c
	crypto/apr_md4.c
	crypto/apr_md5.c
	crypto/apr_passwd.c
	crypto/apr_sha1.c
	crypto/crypt_blowfish.c
	crypto/getuuid.c
	crypto/uuid.c

	dbd/apr_dbd.c
	dbd/apr_dbd_sqlite3.c

	dbm/apr_dbm.c
	dbm/apr_dbm_ndbm.c
	dbm/apr_dbm_sdbm.c

	encoding/apr_base64.c
	hooks/apr_hooks.c
	memcache/apr_memcache.c

	misc/apr_date.c
	misc/apr_queue.c
	misc/apr_reslist.c
	misc/apr_rmm.c
	misc/apr_thread_pool.c
	misc/apu_dso.c
	misc/apu_version.c

	dbm/sdbm/sdbm.c
	dbm/sdbm/sdbm_hash.c
	dbm/sdbm/sdbm_lock.c
	dbm/sdbm/sdbm_pair.c

	strmatch/apr_strmatch.c
	uri/apr_uri.c
	xlate/xlate.c
	xml/apr_xml.c
)

set(DYLIB_INSTALL_NAME "/usr/lib/libaprutil-1.0.dylib")
set(DYLIB_COMPAT_VERSION "4.0.0")
set(DYLIB_CURRENT_VERSION "4.0.0")

add_darling_library(aprutil SHARED ${aprutil_sources})
make_fat(aprutil)
target_link_libraries(aprutil system apr sqlite3 expat iconv)
set_target_properties(aprutil PROPERTIES OUTPUT_NAME "aprutil-1.0")
InstallSymlink("libaprutil-1.0.dylib" "${CMAKE_INSTALL_PREFIX}/libexec/darling/usr/lib/libaprutil-1.dylib")

install(TARGETS aprutil
	DESTINATION libexec/darling/usr/lib)

