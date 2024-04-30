cmake_minimum_required(VERSION 3.28...3.29)
project(Example VERSION 0.0.1)

function(download_cmake_from_github)
    set(flags)
    set(args GITHUB_PREFIX LOCATION)
    set(listArgs FILES)
    
    cmake_parse_arguments(DOWNLOAD "${flags}" "${args}" "${listArgs}" ${ARGN})

    if(NOT DOWNLOAD_FILES)
        message(FATAL_ERROR "[download_cmake_from_repository] FILES where not set")
    endif()

    if(NOT DOWNLOAD_GITHUB_PREFIX)
        set(DOWNLOAD_GITHUB_PREFIX "https://raw.githubusercontent.com/soltys/CMakeFiles/main/src/")
    endif()

    if(NOT DOWNLOAD_LOCATION)
        set(DOWNLOAD_LOCATION "${PROJECT_SOURCE_DIR}/cmake/")
    endif()

    file(MAKE_DIRECTORY ${DOWNLOAD_LOCATION})

    foreach(FILE_NAME ${DOWNLOAD_FILES})
        set(_OUTPUT_FILE "${DOWNLOAD_LOCATION}${FILE_NAME}.cmake")
        set(_DOWNLOAD_URL "${DOWNLOAD_GITHUB_PREFIX}${FILE_NAME}.cmake")
        if(NOT EXISTS "${_OUTPUT_FILE}")
            message(NOTICE "Downloading ${_DOWNLOAD_URL} into ${_OUTPUT_FILE}")
            file(DOWNLOAD "${_DOWNLOAD_URL}" "${_OUTPUT_FILE}")
        endif()
    endforeach()
endfunction()

download_cmake_from_github(FILES "get_all_targets" "vcpkg_check")