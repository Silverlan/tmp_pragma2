include(${CMAKE_SOURCE_DIR}/cmake/pr_common.cmake)

set(PROJ_NAME pragma)
pr_add_executable(${PROJ_NAME} APP_ICON_WIN "${CMAKE_CURRENT_SOURCE_DIR}/appicon.rc" DEBUGGER_LAUNCH_ARGS "-console -luaext")

pr_add_headers(${PROJ_NAME} "include/")
pr_add_sources(${PROJ_NAME} "src/")

if(UNIX)
    target_link_libraries(${PROJ_NAME} PRIVATE "dl")
    target_link_libraries(${PROJ_NAME} PRIVATE "pthread")
endif()

pr_finalize(${PROJ_NAME})

if(WIN32)
    # On Windows we need a separate version of pragma to allow launching it from within a console. This version will use the console subsystem and has the
    # extension ".com" instead of ".exe".

    set(PROJ_NAME pragma_console)
    pr_add_executable(${PROJ_NAME} CONSOLE APP_ICON_WIN "${CMAKE_CURRENT_SOURCE_DIR}/appicon.rc" DEBUGGER_LAUNCH_ARGS "-console -luaext")

    pr_add_headers(${PROJ_NAME} "include/")
    pr_add_sources(${PROJ_NAME} "src/")

    if(UNIX)
        target_link_libraries(${PROJ_NAME} PRIVATE "dl")
        target_link_libraries(${PROJ_NAME} PRIVATE "pthread")
    endif()

    set_target_properties(${PROJ_NAME} PROPERTIES OUTPUT_NAME "pragma" SUFFIX ".com")

    pr_finalize(${PROJ_NAME})
endif()
