
cmake_minimum_required( VERSION 2.8.8 )

project(freetype)

option(FREETYPE_BUILD_SHARED "Build shared library." OFF)
option(FREETYPE_BUILD_STATIC "Build static library." ON)
option(FREETYPE_USE_ZLIB "Use external zlib library for decompression." ON)
option(FREETYPE_USE_BZIP2 "Use external bzip2 library for decompression." ON)
option(FREETYPE_USE_CID "Use CID Font." OFF)

if(NOT FREETYPE_BUILD_STATIC AND NOT FREETYPE_BUILD_SHARED)
  message(FATAL_ERROR "C'mon you must build *one* of the libraries.")
endif()

if(FREETYPE_USE_ZLIB)
  find_package(ZLIB REQUIRED)
  set(FT_CONFIG_OPTION_USE_ZLIB TRUE)
  set(FT_CONFIG_OPTION_SYSTEM_ZLIB TRUE)
  set(FT_CONFIG_OPTION_USE_LZW TRUE)
endif()

if(FREETYPE_USE_BZIP2)
  find_package(BZip2 REQUIRED)
  set(FT_CONFIG_OPTION_USE_BZIP2 TRUE)
endif()

set(FTOPTION_HDR ${CMAKE_CURRENT_BINARY_DIR}/include/freetype/config/ftoption.h)
configure_file(include/freetype/config/ftoption.h.in
  ${FTOPTION_HDR}
)

set(LIB_SRCS
  src/autofit/autofit.c
  src/bdf/bdf.c
  src/cff/cff.c
  src/base/ftbase.c
  src/base/ftbitmap.c
  src/cache/ftcache.c
  src/base/ftfstype.c
  src/base/ftgasp.c
  src/base/ftglyph.c
  src/gzip/ftgzip.c
  src/base/ftinit.c
  src/lzw/ftlzw.c
  src/base/ftstroke.c
  src/base/ftsystem.c
  src/smooth/smooth.c
  src/base/ftbbox.c
  src/base/ftmm.c
  src/base/ftpfr.c
  src/base/ftsynth.c
  src/base/fttype1.c
  src/base/ftwinfnt.c
  src/base/ftxf86.c
  src/base/ftlcdfil.c
  src/base/ftgxval.c
  src/base/ftotval.c
  src/base/ftpatent.c
  src/pcf/pcf.c
  src/pfr/pfr.c
  src/psaux/psaux.c
  src/pshinter/pshinter.c
  src/psnames/psmodule.c
  src/raster/raster.c
  src/sfnt/sfnt.c
  src/truetype/truetype.c
  src/type1/type1.c
  src/cid/type1cid.c
  src/type42/type42.c
  src/winfonts/winfnt.c
)

set(LIB_BUILD_HDRS
  include/ft2build.h
)
  
set(LIB_HDRS
  include/freetype/freetype.h
  include/freetype/ftadvanc.h
  include/freetype/ftbbox.h
  include/freetype/ftbitmap.h
  include/freetype/ftcache.h
  include/freetype/ftchapters.h
  include/freetype/fterrdef.h
  include/freetype/fterrors.h
  include/freetype/ftgasp.h
  include/freetype/ftglyph.h
  include/freetype/ftgxval.h
  include/freetype/ftimage.h
  include/freetype/ftincrem.h
  include/freetype/ftlcdfil.h
  include/freetype/ftlist.h
  include/freetype/ftlzw.h
  include/freetype/ftmm.h
  include/freetype/ftmodapi.h
  include/freetype/ftmoderr.h
  include/freetype/ftotval.h
  include/freetype/ftoutln.h
  include/freetype/ftpfr.h
  include/freetype/ftrender.h
  include/freetype/ftsizes.h
  include/freetype/ftsnames.h
  include/freetype/ftstroke.h
  include/freetype/ftsynth.h
  include/freetype/ftsystem.h
  include/freetype/fttrigon.h
  include/freetype/fttypes.h
  include/freetype/ftwinfnt.h
  include/freetype/ftxf86.h
  include/freetype/t1tables.h
  include/freetype/ttnameid.h
  include/freetype/tttables.h
  include/freetype/tttags.h
  include/freetype/ttunpat.h
)

set(LIB_INTERNAL_HDRS
  include/freetype/internal/autohint.h
  include/freetype/internal/ftcalc.h
  include/freetype/internal/ftdebug.h
  include/freetype/internal/ftdriver.h
  include/freetype/internal/ftgloadr.h
  include/freetype/internal/ftmemory.h
  include/freetype/internal/ftobjs.h
  include/freetype/internal/ftpic.h
  include/freetype/internal/ftrfork.h
  include/freetype/internal/ftserv.h
  include/freetype/internal/ftstream.h
  include/freetype/internal/fttrace.h
  include/freetype/internal/ftvalid.h
  include/freetype/internal/internal.h
  include/freetype/internal/psaux.h
  include/freetype/internal/pshints.h
  include/freetype/internal/services
  include/freetype/internal/sfnt.h
  include/freetype/internal/t1types.h
  include/freetype/internal/tttypes.h
)

set(LIB_SERVICES_HDRS
  include/freetype/internal/services/svbdf.h
  include/freetype/internal/services/svcid.h
  include/freetype/internal/services/svgldict.h
  include/freetype/internal/services/svgxval.h
  include/freetype/internal/services/svkern.h
  include/freetype/internal/services/svmm.h
  include/freetype/internal/services/svotval.h
  include/freetype/internal/services/svpfr.h
  include/freetype/internal/services/svpostnm.h
  include/freetype/internal/services/svpscmap.h
  include/freetype/internal/services/svpsinfo.h
  include/freetype/internal/services/svsfnt.h
  include/freetype/internal/services/svttcmap.h
  include/freetype/internal/services/svtteng.h
  include/freetype/internal/services/svttglyf.h
  include/freetype/internal/services/svwinfnt.h
  include/freetype/internal/services/svxf86nm.h
)
set(LIB_CONFIG_HDRS
  include/freetype/config/ftconfig.h
  include/freetype/config/ftheader.h
  include/freetype/config/ftmodule.h
  include/freetype/config/ftstdlib.h
  ${FTOPTION_HDR}
)

if(FREETYPE_USE_ZLIB)
  set(LIB_SRCS ${LIB_SRCS} src/gzip/ftgzip.c)
  set(LIB_HDRS ${LIB_HDRS} include/freetype/ftgzip.h)
endif()

if(FREETYPE_USE_BZIP2)
  set(LIB_SRCS ${LIB_SRCS} src/bzip2/ftbzip2.c)
  set(LIB_HDRS ${LIB_HDRS} include/freetype/ftbzip2.h)
endif()

if(FREETYPE_USE_CID)
  set(LIB_HDRS ${LIB_HDRS} include/freetype/ftcid.h)
endif()

if(LINUX AND ${CMAKE_SYSTEM_PROCESSOR} STREQUAL "x86_64")
  set_source_files_properties(${LIB_SRCS}
    PROPERTIES COMPILE_FLAGS "-fPIC")
elseif(WIN32)
  set(LIB_SRCS ${LIB_SRCS} src/win32/ftdebug.c)
  add_definitions(-D_CRT_SECURE_NO_WARNINGS -D_CRT_SECURE_NO_DEPRECATE)
  add_subdirectory(src/tools)
  set(WINDOWS_DEF_FILE exports.def)
  get_property(APINAMES_LOCATION TARGET apinames PROPERTY LOCATION)
  set(EXPORTABLE_HDRS)
  foreach(LIB_HDR ${LIB_HDRS})
    list(APPEND EXPORTABLE_HDRS ${CMAKE_CURRENT_SOURCE_DIR}/${LIB_HDR})
  endforeach()
  add_custom_command(
    COMMENT "Generating exports file."
    OUTPUT ${WINDOWS_DEF_FILE}
    COMMAND ${APINAMES_LOCATION} -w -o${WINDOWS_DEF_FILE} ${EXPORTABLE_HDRS}
	DEPENDS apinames
  )
elseif(OSX)
  set(LIB_HDRS ${LIB_HDRS} include/freetype/ftmac.h)
elseif(BORLAND)
  set(LIB_HDRS ${LIB_HDRS} include/freetype/ftbdf.h)
endif()
add_definitions(-DFT2_BUILD_LIBRARY)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include ${CMAKE_CURRENT_BINARY_DIR}/include ${ZLIB_INCLUDE_DIRS} ${BZIP2_INCLUDE_DIRS})

set(TARGETS)
set(LIB_NAME freetype)
if(FREETYPE_BUILD_SHARED)
  add_library(${LIB_NAME} SHARED ${LIB_SRCS} ${LIB_BUILD_HDRS} ${LIB_HDRS} ${LIB_CONFIG_HDRS} ${LIB_SERVICES_HDRS} ${LIB_INTERNAL_HDRS} ${WINDOWS_DEF_FILE})
  list(APPEND TARGETS ${LIB_NAME})
endif()
if(FREETYPE_BUILD_STATIC)
  add_library(${LIB_NAME}-static STATIC ${LIB_SRCS} ${LIB_BUILD_HDRS} ${LIB_HDRS} ${LIB_CONFIG_HDRS} ${LIB_SERVICES_HDRS} ${LIB_INTERNAL_HDRS})
  set_target_properties(${LIB_NAME}-static PROPERTIES OUTPUT_NAME "${LIB_NAME}" PREFIX "lib")
  list(APPEND TARGETS ${LIB_NAME}-static)
endif()

message("${TARGETS}")
set_target_properties(${TARGETS} PROPERTIES COMPILE_DEFINITIONS_DEBUG "FT_DEBUG_LEVEL_ERROR;FT_DEBUG_LEVEL_TRACE")
target_link_libraries(${TARGETS} ${ZLIB_LIBRARIES} ${BZIP2_LIBRARIES})

install(TARGETS ${TARGETS}
  RUNTIME DESTINATION bin
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
)

install(FILES ${LIB_HDRS}
  DESTINATION include/freetype2/freetype
)

install(FILES ${LIB_CONFIG_HDRS}
  DESTINATION include/freetype2/freetype/config
)

install(FILES ${LIB_BUILD_HDRS}
  DESTINATION include
)
