CMAKE_MINIMUM_REQUIRED(VERSION 3.0)
PROJECT(jx)

SET(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR})

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
ADD_LIBRARY(jx STATIC ${jx_SOURCES} ${CMAKE_CURRENT_BINARY_DIR}/node_natives.h)

#Deps
#sqlite3
IF(NOT SQLITE3_INCLUDE_DIR OR SQLITE3_LIBRARY)
    FIND_PACKAGE(SQLite3 REQUIRED)
ENDIF()

TARGET_INCLUDE_DIRECTORIES(jx PRIVATE ${SQLITE3_INCLUDE_DIR})
TARGET_LINK_LIBRARIES(jx ${SQLITE3_LIBRARY})

#zlib
IF(NOT ZLIB_INCLUDE_DIRS OR NOT ZLIB_LIBRARIES)
    FIND_PACKAGE(ZLIB REQUIRED)
ENDIF()

TARGET_INCLUDE_DIRECTORIES(jx PRIVATE ${ZLIB_INCLUDE_DIRS})
TARGET_LINK_LIBRARIES(jx ${ZLIB_LIBRARIES})

#libuv0
TARGET_INCLUDE_DIRECTORIES(jx PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/deps/uv/include)
ADD_SUBDIRECTORY(deps/uv)
TARGET_LINK_LIBRARIES(jx uv)

#v8
TARGET_INCLUDE_DIRECTORIES(jx PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/deps/v8/include)
ADD_SUBDIRECTORY(deps/v8)
TARGET_LINK_LIBRARIES(jx v8_base v8_nosnapshot)

#Cares
TARGET_INCLUDE_DIRECTORIES(jx PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/deps/cares/include)
ADD_SUBDIRECTORY(deps/cares)
TARGET_LINK_LIBRARIES(jx cares)

#OpenSSL
IF(NOT OPENSSL_INCLUDE_DIR OR NOT OPENSSL_LIBRARIES)
    FIND_PACKAGE(OpenSSL 1 REQUIRED)
ENDIF()

TARGET_INCLUDE_DIRECTORIES(jx PRIVATE ${OPENSSL_INCLUDE_DIR})
TARGET_LINK_LIBRARIES(jx ${OPENSSL_LIBRARIES})

#http-parser
TARGET_INCLUDE_DIRECTORIES(jx PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/deps/http_parser/)
ADD_DEFINITIONS(-DHTTP_PARSER_STRICT=0)
ADD_LIBRARY(http_parser STATIC ${CMAKE_CURRENT_SOURCE_DIR}/deps/http_parser/http_parser.c)
TARGET_LINK_LIBRARIES(jx http_parser)

ADD_EXECUTABLE(jx_sample main.cpp)
TARGET_LINK_LIBRARIES(jx_sample jx)
