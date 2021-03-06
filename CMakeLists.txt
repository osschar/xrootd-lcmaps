
cmake_minimum_required( VERSION 2.6 )
project( xrootd-lcmaps )

set( CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake )

find_package( Xrootd REQUIRED )
find_package( Lcmaps REQUIRED )

if( CMAKE_COMPILER_IS_GNUCXX )
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Werror -DLCMAPS_GSI_MODE=1" )
endif()

if( CMAKE_COMPILER_IS_GNUCC )
  set(CMAKE_CXX_FLAGS "${CMAKE_C_FLAGS} -Wall -Werror -DLCMAPS_GSI_MODE=1" )
endif()

include_directories( "${PROJECT_SOURCE_DIR}" "${XROOTD_INCLUDES}"
"${LCMAPS_INCLUDES}" )

add_library(XrdLcmaps MODULE src/XrdLcmaps.cc)
target_link_libraries(XrdLcmaps ${XROOTD_SECGSI} ${XROOTD_CRYPTOSSL}
${LCMAPS_LIB} ${DL_LIB})

install(
  FILES
  ${CMAKE_SOURCE_DIR}/configs/lcmaps.cfg
  DESTINATION ${SYSCONF_INSTALL_DIR}/xrootd
  PERMISSIONS
  OWNER_EXECUTE OWNER_WRITE OWNER_READ)

install(
  TARGETS XrdLcmaps
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR} )

set_target_properties(
  XrdLcmaps
  PROPERTIES
  VERSION   "0.0.1"
  SOVERSION "0"
  LINK_INTERFACE_LIBRARIES "" )

