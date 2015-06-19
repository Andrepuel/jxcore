CMAKE_MINIMUM_REQUIRED(VERSION 3.2)
PROJECT(uv)

SET(LIBUVDIR ${CMAKE_CURRENT_SOURCE_DIR})

include_directories(${LIBUVDIR}/include ${LIBUVDIR}/src ${LIBUVDIR}/include/uv-private)
set(SOURCES
  ${LIBUVDIR}/src/fs-poll.c
  ${LIBUVDIR}/src/inet.c
  ${LIBUVDIR}/src/uv-common.c
  ${LIBUVDIR}/src/version.c)

if(WIN32)
  add_definitions(-DWIN32_LEAN_AND_MEAN -D_WIN32_WINNT=0x0600 -D_CRT_SECURE_NO_WARNINGS)
  include_directories(${LIBUVDIR}/src/win)
  set(SOURCES ${SOURCES}
    ${LIBUVDIR}/src/async.c
    ${LIBUVDIR}/src/atomicops-inl.h
    ${LIBUVDIR}/src/core.c
    ${LIBUVDIR}/src/dl.c
    ${LIBUVDIR}/src/error.c
    ${LIBUVDIR}/src/fs-event.c
    ${LIBUVDIR}/src/fs.c
    ${LIBUVDIR}/src/getaddrinfo.c
    ${LIBUVDIR}/src/handle-inl.h
    ${LIBUVDIR}/src/handle.c
    ${LIBUVDIR}/src/internal.h
    ${LIBUVDIR}/src/loop-watcher.c
    ${LIBUVDIR}/src/pipe.c
    ${LIBUVDIR}/src/poll.c
    ${LIBUVDIR}/src/process-stdio.c
    ${LIBUVDIR}/src/process.c
    ${LIBUVDIR}/src/req-inl.h
    ${LIBUVDIR}/src/req.c
    ${LIBUVDIR}/src/signal.c
    ${LIBUVDIR}/src/stream-inl.h
    ${LIBUVDIR}/src/stream.c
    ${LIBUVDIR}/src/tcp.c
    ${LIBUVDIR}/src/thread.c
    ${LIBUVDIR}/src/threadpool.c
    ${LIBUVDIR}/src/timer.c
    ${LIBUVDIR}/src/tty.c
    ${LIBUVDIR}/src/udp.c
    ${LIBUVDIR}/src/util.c
    ${LIBUVDIR}/src/winapi.c
    ${LIBUVDIR}/src/winapi.h
    ${LIBUVDIR}/src/winsock.c
    ${LIBUVDIR}/src/winsock.h
    )

else()
  include_directories(${LIBUVDIR}/src/unix)
  set(SOURCES ${SOURCES}

      #      ${LIBUVDIR}/src/unix/aix.c
      #      ${LIBUVDIR}/src/unix/android-ifaddrs.c
      ${LIBUVDIR}/src/unix/async.c
      ${LIBUVDIR}/src/unix/atomic-ops.h
      ${LIBUVDIR}/src/unix/core.c
      #      ${LIBUVDIR}/src/unix/cygwin.c
      ${LIBUVDIR}/src/unix/darwin-proctitle.c
      ${LIBUVDIR}/src/unix/darwin.c
      ${LIBUVDIR}/src/unix/dl.c
      ${LIBUVDIR}/src/unix/error.c
      #      ${LIBUVDIR}/src/unix/freebsd.c
      ${LIBUVDIR}/src/unix/fs.c
      ${LIBUVDIR}/src/unix/fsevents.c
      ${LIBUVDIR}/src/unix/getaddrinfo.c
      ${LIBUVDIR}/src/unix/internal.h
      ${LIBUVDIR}/src/unix/kqueue.c
      #      ${LIBUVDIR}/src/unix/linux-core.c
      #      ${LIBUVDIR}/src/unix/linux-inotify.c
      #      ${LIBUVDIR}/src/unix/linux-syscalls.c
      #      ${LIBUVDIR}/src/unix/linux-syscalls.h
      ${LIBUVDIR}/src/unix/loop-watcher.c
      ${LIBUVDIR}/src/unix/loop.c
      #      ${LIBUVDIR}/src/unix/netbsd.c
      #      ${LIBUVDIR}/src/unix/openbsd.c
      ${LIBUVDIR}/src/unix/pipe.c
      ${LIBUVDIR}/src/unix/poll.c
      ${LIBUVDIR}/src/unix/process.c
      ${LIBUVDIR}/src/unix/proctitle.c
      #      ${LIBUVDIR}/src/unix/pthread-fixes.c
      ${LIBUVDIR}/src/unix/signal.c
      ${LIBUVDIR}/src/unix/stream.c
      #      ${LIBUVDIR}/src/unix/sunos.c
      ${LIBUVDIR}/src/unix/tcp.c
      ${LIBUVDIR}/src/unix/thread.c
      ${LIBUVDIR}/src/unix/threadpool.c
      ${LIBUVDIR}/src/unix/timer.c
      ${LIBUVDIR}/src/unix/tty.c
      ${LIBUVDIR}/src/unix/udp.c
      ${LIBUVDIR}/src/unix/uv-dtrace.d
    )
endif()

add_library(uv STATIC ${SOURCES})
TARGET_LINK_LIBRARIES(uv 
"-framework Foundation"
"-framework CoreServices"
"-framework ApplicationServices"
)

