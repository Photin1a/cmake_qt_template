cmake_minimum_required(VERSION 3.5)
project(calibration  VERSION 0.1 LANGUAGES CXX)
set(CMAKE_BUILD_TYPE Debug)

#打开UIC，自动将ui文件转换为.h文件
set(CMAKE_AUTOUIC ON)  
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include_directories(include)
include_directories(plugin)
# FILE(GLOB PROJECT_HEADERS "include/*.h")

####################################TODO  package#######################################
#3rd package
list(APPEND CMAKE_PREFIX_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

#指明Qt库的位置   如果环境变量有Qt，就可以不用
list(APPEND CMAKE_PREFIX_PATH "/opt/Qt/5.15.2/gcc_64")
find_package(Qt5 REQUIRED COMPONENTS Widgets Core Charts Network)
#include_directories

####################################TODO project files#######################################
#headers 
list(APPEND PROJECT_HEADERS
        # include/mainwindow.h
        # plugin/plugin_A/plugin_A.h 
)
#sources 
list(APPEND PROJECT_SOURCES
        src/main.cpp
)
#ui_dir
list(APPEND UI_FILES_PATHS
ui/
)
# resource files qrc 
list(APPEND RESOURCE_QRC
    resources/res.qrc
)

####################################TODO plugin files#######################################
# #libplugin_A.so
# set(PLUGIN_NAME plugin_A)#setup.1
# set(PLUGIN_DIR ${CMAKE_CURRENT_SOURCE_DIR}/plugin/plugin_A)   #设置plugin_A目录  setup.2
# set(PLUGIN_OUTPUT_DIR ${CMAKE_CURRENT_BINARY_DIR}/plugin/plugin_A)   #设置plugin_A输出目录   setup.3
# configure_file(${PLUGIN_DIR}/plugin_A_base_class.json ${PLUGIN_OUTPUT_DIR}/plugin_A_base_class.json COPYONLY)# copy json file to OUTPUT_DIR   setup.4
# add_library(${PLUGIN_NAME} SHARED 
# ${PLUGIN_DIR}/plugin_A.cpp 
# ${PLUGIN_DIR}/plugin_A.h  #头文件也要放进来一起编译，不然运行时会报错
# )
# set_target_properties(${PLUGIN_NAME} PROPERTIES
#     LIBRARY_OUTPUT_DIRECTORY ${PLUGIN_DIR}
# )
# target_link_libraries(${PLUGIN_NAME} PRIVATE 
# Qt5::Core
# )
# #libplugin_B.so
# set(PLUGIN_NAME plugin_B)#setup.1
# set(PLUGIN_DIR ${CMAKE_CURRENT_SOURCE_DIR}/plugin/plugin_B)   #设置plugin_A目录  setup.2
# set(PLUGIN_OUTPUT_DIR ${CMAKE_CURRENT_BINARY_DIR}/plugin/plugin_B)   #设置plugin_A输出目录   setup.3
# configure_file(${PLUGIN_DIR}/plugin_B_base_class.json ${PLUGIN_OUTPUT_DIR}/plugin_B_base_class.json COPYONLY)# copy json file to OUTPUT_DIR   setup.4
# add_library(${PLUGIN_NAME} SHARED 
# ${PLUGIN_DIR}/plugin_B.cpp 
# ${PLUGIN_DIR}/plugin_B.h  #头文件也要放进来一起编译，不然运行时会报错
# )
# set_target_properties(${PLUGIN_NAME} PROPERTIES
#     LIBRARY_OUTPUT_DIRECTORY ${PLUGIN_DIR}
# )
# target_link_libraries(${PLUGIN_NAME} PRIVATE 
# Qt5::Core
# )

####################################TODO executable files, librares files#######################################
add_executable(calibration
    ${PROJECT_HEADERS}
    ${PROJECT_SOURCES}
    ${RESOURCE_QRC}
)
set_property(TARGET calibration PROPERTY AUTOUIC_SEARCH_PATHS ${CMAKE_CURRENT_SOURCE_DIR}/${UI_FILES_PATHS}) #根据CMAKE官方文档，CMAKE会去包含ui_.h的文件所在文件夹寻找.ui文件，或者去AUTOUIC_SEARCH_PATHS下寻找。
target_link_libraries(calibration PRIVATE 
Qt5::Core
Qt5::Network
)

