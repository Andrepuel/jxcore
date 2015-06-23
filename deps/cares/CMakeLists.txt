CMAKE_MINIMUM_REQUIRED(VERSION 3.0)
PROJECT(cares)

ADD_DEFINITIONS(-DHAVE_CONFIG_H)
ADD_LIBRARY(cares STATIC
include/ares.h
include/ares_version.h
include/nameser.h
src/ares_cancel.c
src/ares__close_sockets.c
src/ares_data.c
src/ares_data.h
src/ares_destroy.c
src/ares_dns.h
src/ares_expand_name.c
src/ares_expand_string.c
src/ares_fds.c
src/ares_free_hostent.c
src/ares_free_string.c
src/ares_getenv.h
src/ares_gethostbyaddr.c
src/ares_gethostbyname.c
src/ares__get_hostent.c
src/ares_getnameinfo.c
src/ares_getopt.c
src/ares_getopt.h
src/ares_getsock.c
src/ares_init.c
src/ares_ipv6.h
src/ares_library_init.c
src/ares_library_init.h
src/ares_llist.c
src/ares_llist.h
src/ares_mkquery.c
src/ares_nowarn.c
src/ares_nowarn.h
src/ares_options.c
src/ares_parse_aaaa_reply.c
src/ares_parse_a_reply.c
src/ares_parse_mx_reply.c
src/ares_parse_naptr_reply.c
src/ares_parse_ns_reply.c
src/ares_parse_ptr_reply.c
src/ares_parse_soa_reply.c
src/ares_parse_srv_reply.c
src/ares_parse_txt_reply.c
src/ares_platform.h
src/ares_private.h
src/ares_process.c
src/ares_query.c
src/ares__read_line.c
src/ares_rules.h
src/ares_search.c
src/ares_send.c
src/ares_setup.h
src/ares_strcasecmp.c
src/ares_strcasecmp.h
src/ares_strdup.c
src/ares_strdup.h
src/ares_strerror.c
src/ares_timeout.c
src/ares__timeval.c
src/ares_version.c
src/ares_writev.c
src/ares_writev.h
src/bitncmp.c
src/bitncmp.h
src/inet_net_pton.c
src/inet_net_pton.h
src/inet_ntop.c
src/inet_ntop.h
src/setup_once.h
src/windows_port.c
#Darwin
config/darwin/ares_config.h
)

TARGET_INCLUDE_DIRECTORIES(cares PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/include)
TARGET_INCLUDE_DIRECTORIES(cares PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/src)
TARGET_INCLUDE_DIRECTORIES(cares PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/config/darwin)

