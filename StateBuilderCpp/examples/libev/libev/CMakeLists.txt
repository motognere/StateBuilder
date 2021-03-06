project(LIBEV)
set(LIBEV_LIBRARY ev CACHE INTERNAL "LIBEV_LIBRARY LIBRARY NAME")
set(LIBEV_INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR} CACHE INTERNAL "LIBEV_INCLUDE_DIR INCLUDE DIRECTORY")

include(CheckIncludeFiles)
include(CheckFunctionExists)
include(CheckLibraryExists)

#include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(${LIBEV_BINARY_DIR})
add_definitions(-DHAVE_CONFIG_H=1 -DEV_MULTIPLICITY=1)

check_include_files(sys/inotify.h HAVE_SYS_INOTIFY_H)
check_include_files(sys/epoll.h HAVE_SYS_EPOLL_H)
check_include_files(sys/event.h HAVE_SYS_EVENT_H)
check_include_files(sys/queue.h HAVE_SYS_QUEUE_H)
check_include_files(port.h HAVE_PORT_H)
check_include_files(poll.h HAVE_POLL_H)
check_include_files(sys/select.h HAVE_SYS_SELECT_H)
check_include_files(sys/eventfd.h HAVE_SYS_EVENTFD_H)

check_function_exists(inotify_init HAVE_INOTIFY_INIT)
check_function_exists(epoll_ctl HAVE_EPOLL_CTL)
check_function_exists(kqueue HAVE_KQUEUE)
check_function_exists(port_create HAVE_PORT_CREATE)
check_function_exists(poll HAVE_POLL)
check_function_exists(select HAVE_SELECT)
check_function_exists(eventfd HAVE_EVENTFD)
check_function_exists(nanosleep HAVE_NANOSLEEP)

# check first without rt
check_function_exists(clock_gettime HAVE_CLOCK_GETTIME)

check_library_exists(rt clock_gettime "" HAVE_LIBRT)
# then check with rt
check_library_exists(rt clock_gettime "" HAVE_CLOCK_GETTIME)

check_library_exists(m ceil "" HAVE_LIBM)

configure_file(config.h.cmake ${LIBEV_BINARY_DIR}/config.h)

set(LIBEV_SRCS
  ev.c
)

#include_directories()

#link_directories(${Boost_LIBRARY_DIRS} )

add_library(${LIBEV_LIBRARY} STATIC ${LIBEV_SRCS} ) 

#target_link_libraries(${LIBEV_LIBRARY} 
  
#)


# A warning is an error
if(MSVC) 
  add_definitions( /W4 ) 
else (MSVC)  
  add_definitions( -Wall)
endif (MSVC)

 
