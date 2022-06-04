
#
# libdbc.so
#
add_library(dbc SHARED dbc.cc parser.cc packer.cc common.cc)
target_compile_definitions(dbc PRIVATE DBC_FILE_PATH="${CMAKE_CURRENT_SOURCE_DIR}/..")
target_link_libraries(dbc PRIVATE common CapnProto::capnp CapnProto::kj ${ZMQ_LINK_LIBRARIES})

#
# parser_pyx
#
add_cython_target(opendbc.can.parser_pyx parser_pyx.pyx CXX PY3)
add_library(opendbc.can.parser_pyx MODULE ${opendbc.can.parser_pyx})
set_target_properties(opendbc.can.parser_pyx PROPERTIES PREFIX "" OUTPUT_NAME "parser_pyx")
target_include_directories(opendbc.can.parser_pyx PRIVATE ${Python3_INCLUDE_DIRS} ${CMAKE_CURRENT_SOURCE_DIR})
target_link_libraries(opendbc.can.parser_pyx PRIVATE ${Python3_LIBRARIES} dbc)

#
# packer_pyx
#
add_cython_target(opendbc.can.packer_pyx packer_pyx.pyx CXX PY3)
add_library(opendbc.can.packer_pyx MODULE ${opendbc.can.packer_pyx})
set_target_properties(opendbc.can.packer_pyx PROPERTIES PREFIX "" OUTPUT_NAME "packer_pyx")
target_include_directories(opendbc.can.packer_pyx PRIVATE ${Python3_INCLUDE_DIRS} ${CMAKE_CURRENT_SOURCE_DIR})
target_link_libraries(opendbc.can.packer_pyx PRIVATE ${Python3_LIBRARIES} dbc)

#
# installation
#
if (PYTHON_SITE_INSTALL)
    install(TARGETS dbc LIBRARY DESTINATION ${PYTHON_SITE}/opendbc/can)
    install(TARGETS opendbc.can.parser_pyx LIBRARY DESTINATION DESTINATION ${PYTHON_SITE}/opendbc/can)
    install(TARGETS opendbc.can.packer_pyx LIBRARY DESTINATION ${PYTHON_SITE}/opendbc/can)

    file(GLOB PYTHON_FILES ${CMAKE_CURRENT_SOURCE_DIR}/*.py)
    install(FILES ${PYTHON_FILES} DESTINATION ${PYTHON_SITE}/opendbc/can)

    if (BUILD_TESTING)
        file(GLOB PYTHON_TEST_FILES ${CMAKE_CURRENT_SOURCE_DIR}/tests/*.py)
        install(FILES ${PYTHON_TEST_FILES} DESTINATION ${PYTHON_SITE}/opendbc/can/tests)
    endif ()
endif()
