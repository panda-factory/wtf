
SET(APP_NAME example_editor)

SET(EXAMPLE_EDITOR_SRC
    ${CMAKE_CURRENT_SOURCE_DIR}/editor_app.cpp
    )

ADD_EXECUTABLE(${APP_NAME} ${EXAMPLE_EDITOR_SRC})

IF(BUILDING_SHARED)
    TARGET_COMPILE_DEFINITIONS(${APP_NAME} PRIVATE BUILDING_SHARED=1)
ENDIF()

TARGET_INCLUDE_DIRECTORIES(${APP_NAME} PRIVATE
    ${PROJECT_ROOT_PATH}/include
    ${PROJECT_ROOT_PATH}/src
    ${PROJECT_ROOT_PATH}/third_party/skia
    )

TARGET_LINK_LIBRARIES(${APP_NAME} PRIVATE lib_sm)

TARGET_COMPILE_DEFINITIONS(${APP_NAME} PUBLIC NOMINMAX)