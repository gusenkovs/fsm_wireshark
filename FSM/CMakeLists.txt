set(DISSECTOR_SRC
        packet-fsm.c
)

set(PLUGIN_FILES
        plugin.c
        ${DISSECTOR_SRC}
)

set(CLEAN_FILES
        ${PLUGIN_FILES}
)

if (WERROR)
        set_source_files_properties(
                ${CLEAN_FILES}
                PROPERTIES
                COMPILE_FLAGS -Werror
        )
endif()

include_directories(${CMAKE_CURRENT_SOURCE_DIR})

register_dissector_files(plugin.c
        plugin
        ${DISSECTOR_SRC}
)

add_library(fsm ${LINK_MODE_MODULE}
        ${PLUGIN_FILES}
)
set_target_properties(fsm PROPERTIES PREFIX "")
set_target_properties(fsm PROPERTIES LINK_FLAGS "${WS_LINK_FLAGS}")

target_link_libraries(fsm epan)

install(TARGETS fsm
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}/@CPACK_PACKAGE_NAME@/plugins/${CPACK_PACKAGE_VERSION} NAMELINK_SKIP
        RUNTIME DESTINATION ${CMAKE_INSTALL_LIBDIR}/@CPACK_PACKAGE_NAME@/plugins/${CPACK_PACKAGE_VERSION}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}/@CPACK_PACKAGE_NAME@/plugins/${CPACK_PACKAGE_VERSION}
)
