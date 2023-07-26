project(apr-util)

cmake_minimum_required(VERSION 3.13)

if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 NEW)
endif(COMMAND cmake_policy)

add_compile_definitions(
	HAVE_CONFIG_H
	DARWIN
	SIGPROCMASK_SETS_THREAD_MASK
	DARWIN_10
)

include_directories(
	${CMAKE_CURRENT_SOURCE_DIR}/../gen/include
	${CMAKE_CURRENT_SOURCE_DIR}/../gen/include/private
	${CMAKE_CURRENT_SOURCE_DIR}/include/private
	${CMAKE_CURRENT_SOURCE_DIR}/include 
	${CMAKE_CURRENT_SOURCE_DIR}/../../apr/gen/include
	${CMAKE_CURRENT_SOURCE_DIR}/../../apr/apr/include
)

add_compile_options(
	-w
	-nostdinc
	-fblocks

	#-U_POSIX_C_SOURCE
	#-D__DARWIN_UNIX03
)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include/private)

set(aprutil_sources
	buckets/apr_brigade.c
	buckets/apr_buckets_alloc.c
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
	buckets/apr_buckets.c

	crypto/apr_crypto.c
	crypto/apr_md4.c
	crypto/apr_md5.c
	crypto/apr_passwd.c
	crypto/apr_sha1.c
	crypto/crypt_blowfish.c
	crypto/getuuid.c
	crypto/uuid.c

	dbd/apr_dbd_sqlite3.c
	dbd/apr_dbd.c

	dbm/apr_dbm_sdbm.c
	dbm/apr_dbm.c
	dbm/sdbm/sdbm_hash.c
	dbm/sdbm/sdbm_lock.c
	dbm/sdbm/sdbm_pair.c
	dbm/sdbm/sdbm.c

	encoding/apr_base64.c

	hooks/apr_hooks.c

	ldap/apr_ldap_init.c
	ldap/apr_ldap_option.c
	ldap/apr_ldap_rebind.c
	ldap/apr_ldap_stub.c
	ldap/apr_ldap_url.c

	memcache/apr_memcache.c

	misc/apr_date.c
	misc/apr_queue.c
	misc/apr_reslist.c
	misc/apr_rmm.c
	misc/apr_thread_pool.c
	misc/apu_dso.c
	misc/apu_version.c

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
target_link_libraries(aprutil system apr sqlite3 expat iconv LDAP)
set_target_properties(aprutil PROPERTIES OUTPUT_NAME "aprutil-1.0")
InstallSymlink("libaprutil-1.0.dylib" "${CMAKE_INSTALL_PREFIX}/libexec/darling/usr/lib/libaprutil-1.dylib")

install(TARGETS aprutil
	DESTINATION libexec/darling/usr/lib)

