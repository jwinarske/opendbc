
add_library(dbc SHARED dbc.cc parser.cc packer.cc common.cc)
target_compile_definitions(dbc PRIVATE DBC_FILE_PATH="${CMAKE_CURRENT_SOURCE_DIR}/..")
target_link_libraries(dbc PRIVATE common CapnProto::capnp CapnProto::kj ${ZMQ_LINK_LIBRARIES})

# Build packer and parser
add_cython_target(parser_pyx parser_pyx.pyx CXX)
add_library(parser_pyx MODULE ${parser_pyx})
target_include_directories(parser_pyx PRIVATE ${Python3_INCLUDE_DIRS} ${CMAKE_CURRENT_SOURCE_DIR})
target_link_libraries(parser_pyx PRIVATE ${Python3_LIBRARIES} dbc)

add_cython_target(packer_pyx packer_pyx.pyx CXX)
add_library(packer_pyx MODULE ${parser_pyx})
target_include_directories(packer_pyx PRIVATE ${Python3_INCLUDE_DIRS} ${CMAKE_CURRENT_SOURCE_DIR})
target_link_libraries(packer_pyx PRIVATE ${Python3_LIBRARIES} dbc)
