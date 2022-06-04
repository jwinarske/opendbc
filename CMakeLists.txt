
if(PYTHON_SITE_INSTALL)
    # Get Python Site Path
    message(STATUS "PYTHON_EXECUTABLE=${PYTHON_EXECUTABLE}")
    execute_process(
      COMMAND "${PYTHON_EXECUTABLE}" -c "if True:
        from distutils import sysconfig as sc
        print(sc.get_python_lib(prefix='', plat_specific=True))"
      OUTPUT_VARIABLE PYTHON_SITE
      OUTPUT_STRIP_TRAILING_WHITESPACE)

    file(GLOB DBC_FILES ${CMAKE_CURRENT_SOURCE_DIR}/*.dbc)
    install(FILES ${DBC_FILES} __init__.py DESTINATION ${PYTHON_SITE}/opendbc)
endif()

add_subdirectory(can)
