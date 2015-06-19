CMAKE_MINIMUM_REQUIRED(VERSION 3.2)
PROJECT(jx)

#Deps
#sqlite3
INCLUDE_DIRECTORIES(/usr/local/Cellar/sqlite/3.8.9/include)
SET(DEPS ${DEPS} /usr/local/Cellar/sqlite/3.8.9/lib/libsqlite3.dylib)

#zlib
SET(DEPS ${DEPS} z)

#libuv0
INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR}/deps/uv/include)
ADD_SUBDIRECTORY(deps/uv)
SET(DEPS ${DEPS} uv)

#v8
INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR}/deps/v8/include)
ADD_SUBDIRECTORY(deps/v8)
SET(DEPS ${DEPS} v8_base v8_nosnapshot)

#Cares
INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR}/deps/cares/include)
ADD_SUBDIRECTORY(deps/cares)
SET(DEPS ${DEPS} cares)

#OpenSSL
FIND_PACKAGE(OpenSSL 1 REQUIRED)
INCLUDE_DIRECTORIES(${OPENSSL_INCLUDE_DIR})
SET(DEPS ${DEPS} ${OPENSSL_LIBRARIES})

#http-parser
INCLUDE_DIRECTORIES(/usr/local/Cellar/http-parser/2.5.0/include)
SET(DEPS ${DEPS} /usr/local/Cellar/http-parser/2.5.0/lib/libhttp_parser.dylib)

#Main sources
INCLUDE_DIRECTORIES(
${CMAKE_CURRENT_SOURCE_DIR}/src
${CMAKE_CURRENT_SOURCE_DIR}/src/wrappers
${CMAKE_CURRENT_SOURCE_DIR}/tools/msvs/genfiles
${CMAKE_CURRENT_SOURCE_DIR}/deps/uv/src/ares
${CMAKE_CURRENT_BINARY_DIR}
)

ADD_DEFINITIONS(-DNODE_WANT_INTERNALS=1)
ADD_DEFINITIONS(-D__POSIX__)
ADD_DEFINITIONS(-DARCH="x64")
ADD_DEFINITIONS(-DPLATFORM="darwin")

SET(jx_SOURCES
src/jx/Proxy/EngineLogger.cc
src/jx/extend.cc
src/jx/commons.cc
src/jx/job.cc
src/jx/jx_instance.cc
src/jx/job_store.cc
src/jx/memory_store.cc
src/jx/jxp_compress.cc
src/jx/error_definition.cc

src/wrappers/handle_wrap.cc
src/wrappers/thread_wrap.cc
src/wrappers/memory_wrap.cc
src/wrappers/jxtimers_wrap.cc
src/wrappers/timer_wrap.cc
src/wrappers/node_os.cc
src/wrappers/cares_wrap.cc
src/wrappers/fs_event_wrap.cc
src/wrappers/node_buffer.cc
src/wrappers/tcp_wrap.cc
src/wrappers/udp_wrap.cc
src/wrappers/jxutils_wrap.cc
src/wrappers/process_wrap.cc
src/wrappers/signal_wrap.cc
src/wrappers/node_file.cc
src/wrappers/stream_wrap.cc
src/wrappers/tty_wrap.cc
src/wrappers/pipe_wrap.cc
src/wrappers/node_http_parser.cc
src/wrappers/node_zlib.cc

src/external/module_wrap.cc
src/external/sqlite3/database.cc
src/external/sqlite3/statement.cc
src/external/sqlite3/node_sqlite3.cc

src/node_constants.cc
src/node_extensions.cc
src/node_javascript.cc

src/public/jx_result.cc
src/public/jx.cc

src/jxcore.cc
src/node.cc
src/node_script.cc
src/string_bytes.cc
src/slab_allocator.cc
src/node_stat_watcher.cc
)

#V8
SET(jx_SOURCES ${jx_SOURCES}
src/jx/Proxy/V8/JXString.cc
src/jx/Proxy/V8/v8_typed_array.cc
)
ADD_DEFINITIONS(-DJS_ENGINE_V8=1)

#OpenSSL
SET(jx_SOURCES ${jx_SOURCES}
src/wrappers/node_crypto.cc
)
ADD_DEFINITIONS(-DHAVE_OPENSSL=1)

SET(jx_LIBRARIES
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/jx/_jx_argv.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/jx/_jx_subs.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/jx/_jx_memStore.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/jx/_jx_tasks.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/jx/_jx_utils.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/jx/_jx_multiplier.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/jx/_jx_monitor.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/jx/_jx_monitorHelper.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/jx/_jx_loadEmbedded.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/jx/_jx_http_helper.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/jx/_jx_config.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/jx/_jx_incoming.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/jx/_jx_parsers.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/jx/_jx_timers.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/jx/_jx_source.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/jx/_jx_marker.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/external/sqlite3.js
    ${CMAKE_CURRENT_SOURCE_DIR}/src/node.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/_debugger.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/_linklist.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/assert.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/buffer.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/child_process.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/console.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/constants.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/crypto.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/cluster.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/dgram.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/dns.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/domain.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/events.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/freelist.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/fs.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/http.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/https.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/module.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/net.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/os.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/path.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/punycode.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/querystring.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/readline.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/repl.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/stream.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/_stream_readable.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/_stream_writable.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/_stream_duplex.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/_stream_transform.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/_stream_passthrough.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/string_decoder.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/sys.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/timers.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/tls.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/tty.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/url.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/util.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/vm.js
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/zlib.js
    ${CMAKE_CURRENT_SOURCE_DIR}/src/macros.py
)

ADD_CUSTOM_COMMAND(OUTPUT node_natives.h COMMAND python ${CMAKE_CURRENT_SOURCE_DIR}/tools/js2c.py node_natives.h ${jx_LIBRARIES} ${CMAKE_CURRENT_SOURCE_DIR}/config.gypi DEPENDS src/macros.py)

##MOZJS
#SET(jx_SOURCES ${jx_SOURCES}
#src/jx/Proxy/Mozilla/MozJS/Isolate.cc
#src/jx/Proxy/Mozilla/MozJS/MozValue.cc
#src/jx/Proxy/Mozilla/MozJS/Exception.cc
#src/jx/Proxy/Mozilla/MozJS/utf_man.cc
#src/jx/Proxy/Mozilla/EngineHelper.cc
#src/jx/Proxy/Mozilla/JXString.cc
#src/jx/Proxy/Mozilla/PArguments.cc
#src/jx/Proxy/Mozilla/SpiderHelper.cc
#)
#ADD_DEFINITIONS(-DJS_ENGINE_MOZJS=1)

#Creating library
#ADD_LIBRARY(jx STATIC ${jx_SOURCES} ${CMAKE_CURRENT_BINARY_DIR}/node_natives.h)
ADD_EXECUTABLE(jx ${jx_SOURCES} ${CMAKE_CURRENT_BINARY_DIR}/node_natives.h main.cpp)
TARGET_LINK_LIBRARIES(jx ${DEPS})
