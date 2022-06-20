cmake_minimum_required(VERSION 3.6)

set(lib_name luasocket)
set(target_name ext_${lib_name})

project(${lib_name})

# set(${target_name}_src
#   buffer.c
#   io.c
#   timeout.c
#   select.c
#   udp.c
#   mime.c
#   tcp.c
#   auxiliar.c
#   inet.c
#   luasocket.c
#   luasocket_scripts.c
#   except.c
#   options.c
# )

# if(WINDOWS)
#   list(APPEND ${target_name}_src 
#     wsocket.c
#   )
# else()
#   list(APPEND ${target_name}_src 
#     unix.c
#     usocket.c
#     serial.c
#   )
# endif()


# add_library(${target_name} STATIC
#   ${${target_name}_src}
# )

# if(LINUX)
#   target_compile_definitions(${target_name} PRIVATE _GNU_SOURCE)
# endif()

# target_include_directories(${target_name} 
#   INTERFACE ..
#   PRIVATE ../luajit/include
# )

# set_target_properties(${target_name}
#     PROPERTIES
#     ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
#     LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
#     FOLDER "External"
# )

# # TODO CHECK "lua/prebuilt/ios/liblua.a" IS NECESSARY?
include(../../cmake/CocosExternalConfig.cmake)

add_library(${target_name} STATIC IMPORTED GLOBAL)

set_target_properties(${target_name} PROPERTIES
  INTERFACE_INCLUDE_DIRECTORIES "${CMAKE_CURRENT_SOURCE_DIR}/include"
)

if(WINDOWS)
  set_target_properties(${target_name} PROPERTIES
    # need use absolutely path
    IMPORTED_LOCATION_DEBUG "${platform_spec_path}/debug/lib${lib_name}.lib"
    IMPORTED_LOCATION_RELEASE "${platform_spec_path}/release/lib${lib_name}.lib"
  )
  set_target_properties(${target_name} PROPERTIES
    MAP_IMPORTED_CONFIG_MINSIZEREL Release
    MAP_IMPORTED_CONFIG_RELWITHDEBINFO Release
  )
else()
  set_target_properties(${target_name} PROPERTIES
    IMPORTED_LOCATION "${platform_spec_path}/lib${lib_name}.a"
  )
endif()