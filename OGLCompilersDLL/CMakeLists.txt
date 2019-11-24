set(SOURCES InitializeDll.cpp InitializeDll.h)

add_library(OGLCompiler STATIC ${SOURCES})
set_property(TARGET OGLCompiler PROPERTY FOLDER glslang)
set_property(TARGET OGLCompiler PROPERTY POSITION_INDEPENDENT_CODE ON)

if(WIN32)
    source_group("Source" FILES ${SOURCES})
endif(WIN32)

if(ENABLE_GLSLANG_INSTALL)
    install(TARGETS OGLCompiler EXPORT OGLCompilerTargets
            ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR})
	install(EXPORT OGLCompilerTargets DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake)
endif(ENABLE_GLSLANG_INSTALL)

#copying built lib to the output
add_custom_command(
TARGET OGLCompiler 
POST_BUILD
COMMAND ${CMAKE_COMMAND} -E copy
  $<TARGET_FILE:OGLCompiler>
  ${CMAKE_SOURCE_DIR}/../builtDependencies/windows/$<CONFIGURATION>/$<TARGET_FILE_NAME:OGLCompiler>)		

#copying the headers
file(COPY ${HEADERS} DESTINATION ${CMAKE_SOURCE_DIR}/../builtDependencies/includes/OGLCompilersDLL)
