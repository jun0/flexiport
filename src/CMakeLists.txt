set(srcs flexiport.cpp
    port.cpp
    timeout.cpp
    ${config_h}
    )
if(FLEXIPORT_INCLUDE_SERIAL)
    set(srcs ${srcs} serialport.cpp)
endif(FLEXIPORT_INCLUDE_SERIAL)
if(FLEXIPORT_INCLUDE_TCP)
    set(srcs ${srcs} tcpport.cpp)
endif(FLEXIPORT_INCLUDE_TCP)
if(FLEXIPORT_INCLUDE_UDP)
    set(srcs ${srcs} udpport.cpp)
endif(FLEXIPORT_INCLUDE_UDP)
if(FLEXIPORT_INCLUDE_LOGGING)
    set(srcs ${srcs} logwriterport.cpp logreaderport.cpp logfile.cpp)
endif(FLEXIPORT_INCLUDE_LOGGING)

include_directories(${PROJECT_SOURCE_DIR}/include)
include_directories(${PROJECT_BINARY_DIR}/include)
add_library(${PROJECT_NAME_LOWER} ${LIB_TYPE} ${srcs})
set_target_properties(${PROJECT_NAME_LOWER} PROPERTIES
    VERSION ${PROJECT_VERSION}
    SOVERSION ${PROJECT_VERSION_MAJOR})
install(TARGETS ${PROJECT_NAME_LOWER}
    EXPORT ${PROJECT_NAME_LOWER}
    LIBRARY DESTINATION ${LIB_INSTALL_DIR} COMPONENT library
    RUNTIME DESTINATION ${BIN_INSTALL_DIR} COMPONENT library
    ARCHIVE DESTINATION ${LIB_INSTALL_DIR} COMPONENT library)
install(EXPORT ${PROJECT_NAME_LOWER}
    DESTINATION ${LIB_INSTALL_DIR}/${PROJECT_NAME_LOWER}
    FILE ${PROJECT_NAME_LOWER}Depends.cmake)
if(WIN32)
    target_link_libraries(${PROJECT_NAME_LOWER} Ws2_32)
endif(WIN32)

