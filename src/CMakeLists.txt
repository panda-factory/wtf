
SET(TARGET_NAME lib_sm)

ADD_SUBDIRECTORY(css)
ADD_SUBDIRECTORY(engine)
ADD_SUBDIRECTORY(graphics)
ADD_SUBDIRECTORY(render)
ADD_SUBDIRECTORY(semantics)
ADD_SUBDIRECTORY(shell)
ADD_SUBDIRECTORY(window)
ADD_SUBDIRECTORY(wtf)

SET(WINDOW_SRC
    ${CMAKE_CURRENT_SOURCE_DIR}/node/abstract_node.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/render/compositing/scene.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/render/compositing/scene_builder.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/render/canvas_scope.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/render/elements/view.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/render/layout/column.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/render/drawable.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/render/elements/element.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/render/layout.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/render/shape.cpp
    )

IF (WIN32)
    SET(WINDOW_SRC ${WINDOW_SRC}
        ${CMAKE_CURRENT_SOURCE_DIR}/render/elements/editable_text.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/render/elements/label.cpp)
ENDIF()

IF(BUILDING_SHARED)
    ADD_LIBRARY(${TARGET_NAME} SHARED ${WINDOW_SRC})
    TARGET_COMPILE_DEFINITIONS(${TARGET_NAME} PRIVATE BUILDING_SHARED=1)
ELSE()
    ADD_LIBRARY(${TARGET_NAME} STATIC ${WINDOW_SRC})
ENDIF()

IF (WIN32)
    TARGET_COMPILE_DEFINITIONS(${TARGET_NAME} PRIVATE OS_WIN=1)
ENDIF()

TARGET_INCLUDE_DIRECTORIES(${TARGET_NAME} PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${PROJECT_ROOT_PATH}
    ${PROJECT_ROOT_PATH}/third_party/angle/include
    ${PROJECT_ROOT_PATH}/third_party/skia
    ${PROJECT_ROOT_PATH}/third_party)

TARGET_LINK_DIRECTORIES(${TARGET_NAME} PUBLIC ${PROJECT_ROOT_PATH}/third_party/angle/libs)
TARGET_LINK_LIBRARIES(${TARGET_NAME} PUBLIC
    ext_convertUTF engine lib_txt shape graphics diagnostic layer painting pipeline render_object
    semantics foundation meta window)
IF (WIN32)
    TARGET_LINK_DIRECTORIES(${TARGET_NAME} PUBLIC ${PROJECT_ROOT_PATH}/third_party/skia/libs/win32)
    TARGET_LINK_LIBRARIES(${TARGET_NAME} PUBLIC libEGL.dll.lib libGLESv2.dll.lib skia.dll.lib skshaper.dll.lib)
ELSEIF(ANDROID)
    TARGET_LINK_DIRECTORIES(${TARGET_NAME} PUBLIC ${PROJECT_ROOT_PATH}/third_party/skia/libs/arm64-v8a)
    TARGET_LINK_LIBRARIES(${TARGET_NAME} PUBLIC shell skia skshaper)
ENDIF()