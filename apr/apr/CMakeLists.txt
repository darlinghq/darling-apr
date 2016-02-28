project(apr)

cmake_minimum_required(VERSION 2.4.0)

if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 NEW)
endif(COMMAND cmake_policy)

add_definitions(-DTARGET_OS_MAC=1)
add_definitions(-D__APPLE__ -D__DYNAMIC__)
add_definitions(-DHAVE_CONFIG_H -U__linux__ -DDARWIN)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -U_POSIX_C_SOURCE -D__DARWIN_UNIX03 -nostdinc -fblocks -fPIC -ggdb -O0")
set(CMAKE_SHARED_LINKER_FLAGS "-nodefaultlibs -Wl,--version-script=${DARLING_TOP_DIRECTORY}/darwin.map")

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include/arch/unix)
include_directories(${DARLING_TOP_DIRECTORY}/src/libc/include/FreeBSD)
include_directories(${DARLING_TOP_DIRECTORY}/src/libc/include/NetBSD)
include_directories(${DARLING_TOP_DIRECTORY}/src/libc/gen)
include_directories(${DARLING_TOP_DIRECTORY}/src/libc/darwin)
include_directories(${DARLING_TOP_DIRECTORY}/src/libc/stdtime/FreeBSD)
include_directories(${DARLING_TOP_DIRECTORY}/src/launchd/liblaunch)
include_directories(${DARLING_TOP_DIRECTORY}/src/external/libdispatch)
include_directories(${DARLING_TOP_DIRECTORY}/src/external/zlib)
include_directories(${DARLING_TOP_DIRECTORY}/src/external/bzip2)

set(apr_sources
	atomic/unix/builtins.c
	atomic/unix/ia32.c
	atomic/unix/mutex.c
	dso/unix/dso.c

	file_io/unix/buffer.c
	file_io/unix/copy.c
	file_io/unix/dir-32-bit-inode.c
	file_io/unix/dir.c
	file_io/unix/fileacc.c
	file_io/unix/filedup.c
	file_io/unix/filepath.c
	file_io/unix/filepath_util.c
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

	mmap/unix/mmap.c
	mmap/unix/common.c

	network_io/unix/inet_ntop.c
	network_io/unix/inet_pton.c
	network_io/unix/multicast.c
	network_io/unix/sendrecv.c
	network_io/unix/sockaddr.c
	network_io/unix/sockets.c
	network_io/unix/socket_util.c
	network_io/unix/sockopt.c
	
	passwd/apr_getpass.c

	poll/unix/kqueue.c
	poll/unix/poll.c
	poll/unix/pollcb.c
	poll/unix/pollset.c
	poll/unix/port.c
	poll/unix/select.c

	random/unix/apr_random.c
	random/unix/sha2.c
	random/unix/sha2_glue.c

	shmem/unix/shm.c

	strings/apr_cpystrn.c
	strings/apr_fnmatch.c
	strings/apr_snprintf.c
	strings/apr_strings.c
	strings/apr_strnatcmp.c
	strings/apr_strtok.c

	support/unix/waitio.c

	tables/apr_hash.c
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

SET(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}/darling")
SET(CMAKE_BUILD_WITH_INSTALL_RPATH TRUE) 
SET(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)

add_library(apr SHARED ${apr_sources})
target_link_libraries(apr system)

install(TARGETS apr
	DESTINATION ${CMAKE_INSTALL_LIBDIR}/darling)
