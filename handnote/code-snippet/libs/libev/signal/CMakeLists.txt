cmake_minimum_required(VERSION 3.13)
#
PROJECT(test)
#ADD_SUBDIRECTORY(src)

INCLUDE_DIRECTORIES(/home/local/include)
LINK_DIRECTORIES(/home/local/lib)

#AUX_SOURCE_DIRECTORY(. DIR_SRC)
FILE(GLOB HDRS "*.h")
FILE(GLOB SRCS "*.c")

#
ADD_DEFINITIONS(
	-O3 -g -W -Wall
	-Wunused-variable -Wunused-parameter -Wunused-function -Wunused
	-Wno-deprecated -Wwrite-strings
	-D__WUR= -D__REENTRANT -D_FILE_OFFSET_BITS=64 -DTIXML_USE_STL)
#-Woverloaded-virtual


#
ADD_EXECUTABLE(${PROJECT_NAME} ${SRCS})
#ADD_LIBRARY(hiwork SHARED ${DIR_SRC})
#ADD_LIBRARY(hiwork_static STATIC ${DIR_SRC})


#SET_TARGET_PROPERTIES(${PROJECT_NAME} PROPERTIES VERSION 1.2 SOVERSION 1)
#
TARGET_LINK_LIBRARIES(${PROJECT_NAME} ev)

#安装
#INSTALL(TARGETS hiwork hiwork_static 
#	LIBRARY DESTINATION lib
#	ARCHIVE DESTINATION lib)
#INSTALL(FILES ${HEADERS}
#	DESTINATION include/hiwork)
