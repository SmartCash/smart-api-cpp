FIND_PACKAGE(JSONCPP REQUIRED)
FIND_PACKAGE(JSONRPCCPP REQUIRED)
FIND_PACKAGE(CURL REQUIRED)

# Find header and source files
FILE(GLOB smartcashapi_header ${CMAKE_CURRENT_SOURCE_DIR}/*.h)
FILE(GLOB smartcashapi_source ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp)

# Set target libraries
ADD_LIBRARY(smartcashapi SHARED ${smartcashapi_source})
ADD_LIBRARY(smartcashapi_static STATIC ${smartcashapi_source})

# Set output name of static library
SET_TARGET_PROPERTIES(smartcashapi_static PROPERTIES OUTPUT_NAME smartcashapi)

# Link the library with json-rpc-cpp libs
TARGET_LINK_LIBRARIES(smartcashapi
                        ${CURL_LIBRARY}
                        ${JSONCPP_LIBRARY}
                        jsonrpccpp-common
                        jsonrpccpp-client)

TARGET_LINK_LIBRARIES(smartcashapi_static
                        ${CURL_LIBRARY}
                        ${JSONCPP_LIBRARY}
                        jsonrpccpp-common
                        jsonrpccpp-client)

# Set version settings
SET(VERSION_STRING ${MAJOR_VERSION}.${MINOR_VERSION}.${PATCH_VERSION})
SET_TARGET_PROPERTIES(smartcashapi smartcashapi_static PROPERTIES
    VERSION "${VERSION_STRING}"
    SOVERSION "${SO_VERSION}")

# Install to appropriate destinations
INSTALL(FILES ${smartcashapi_header} DESTINATION include/smartcashapi)

INSTALL(TARGETS smartcashapi smartcashapi_static
            LIBRARY DESTINATION lib
            ARCHIVE DESTINATION lib)
