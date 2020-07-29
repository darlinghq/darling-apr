project(apr)

cmake_minimum_required(VERSION 2.4.0)

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
	${CMAKE_CURRENT_SOURCE_DIR}/include/arch/unix
	${CMAKE_CURRENT_SOURCE_DIR}/../gen/include/arch/unix
	${CMAKE_CURRENT_SOURCE_DIR}/include/arch/unix
	${CMAKE_CURRENT_SOURCE_DIR}/include
	${CMAKE_CURRENT_SOURCE_DIR}/include/private
	${CMAKE_CURRENT_SOURCE_DIR}/../gen/include/private
)

# Hide warnings
add_compile_options(
	-w
	-nostdinc
	-fblocks

	#-U_POSIX_C_SOURCE
	#-D__DARWIN_UNIX03
)

set(apr_sources
	atomic/unix/builtins.c
	atomic/unix/ia32.c
	atomic/unix/mutex.c
	atomic/unix/ppc.c
	atomic/unix/s390.c
	atomic/unix/solaris.c

	dso/unix/dso.c

	encoding/apr_escape.c

	file_io/unix/buffer.c
	file_io/unix/copy.c
	file_io/unix/dir-32-bit-inode.c
	file_io/unix/dir.c
	file_io/unix/fileacc.c
	file_io/unix/filedup.c
	file_io/unix/filepath_util.c
	file_io/unix/filepath.c
	file_io/unix/filestat-32-bit-inode.c
	file_io/unix/filestat.c
	file_io/unix/flock.c
	file_io/unix/fullrw.c
	file_io/unix/mktemp.c
	file_io/unix/open.c
	file_io/unix/pipe.c
	file_io/unix/readwrite.c
	file_io/unix/seek.c
	file_io/unix/tempdir.c

	locks/unix/global_mutex.c
	locks/unix/proc_mutex.c
	locks/unix/thread_cond.c
	locks/unix/thread_mutex.c
	locks/unix/thread_rwlock.c

	memory/unix/apr_pools.c

	misc/unix/charset.c
	misc/unix/env.c
	misc/unix/errorcodes.c
	misc/unix/getopt.c
	misc/unix/otherchild.c
	misc/unix/rand.c
	misc/unix/start.c
	misc/unix/version.c

	mmap/unix/common.c
	mmap/unix/mmap.c

	network_io/unix/inet_ntop.c
	network_io/unix/inet_pton.c
	network_io/unix/multicast.c
	network_io/unix/sendrecv.c
	network_io/unix/sockaddr.c
	network_io/unix/socket_util.c
	network_io/unix/sockets.c
	network_io/unix/sockopt.c

	passwd/apr_getpass.c

	poll/unix/epoll.c
	poll/unix/kqueue.c
	poll/unix/poll.c
	poll/unix/pollcb.c
	poll/unix/pollset.c
	poll/unix/port.c
	poll/unix/select.c
	poll/unix/z_asio.c

	random/unix/apr_random.c
	random/unix/sha2_glue.c
	random/unix/sha2.c

	shmem/unix/shm.c

	strings/apr_cpystrn.c
	strings/apr_fnmatch.c
	strings/apr_snprintf.c
	strings/apr_strings.c
	strings/apr_strnatcmp.c
	strings/apr_strtok.c

	support/unix/waitio.c

	tables/apr_hash.c
	tables/apr_skiplist.c
	tables/apr_tables.c

	threadproc/unix/proc.c
	threadproc/unix/procsup.c
	threadproc/unix/signals.c
	threadproc/unix/thread.c
	threadproc/unix/threadpriv.c

	time/unix/time.c
	time/unix/timestr.c

	user/unix/groupinfo.c
	user/unix/userinfo.c
)

set(DYLIB_INSTALL_NAME "/usr/lib/libapr-1.0.dylib")
set(DYLIB_COMPAT_VERSION "5.0.0")
set(DYLIB_CURRENT_VERSION "5.8.0")

add_darling_library(apr SHARED ${apr_sources})
make_fat(apr)
target_link_libraries(apr system)
set_target_properties(apr PROPERTIES OUTPUT_NAME "apr-1.0")

InstallSymlink("libapr-1.0.dylib" "${CMAKE_INSTALL_PREFIX}/libexec/darling/usr/lib/libapr-1.dylib")

install(TARGETS apr
	DESTINATION libexec/darling/usr/lib)

