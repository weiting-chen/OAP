cmake_minimum_required (VERSION 2.8)
project (HPNL)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_COMPILER /usr/bin/g++)
set(CMAKE_CXX_FLAGS  " -std=c++11 -g -pthread -fPIC -fstack-protector-strong -I$ENV{PREFIX}/include -I$ENV{PREFIX}/include/linux " ${CMAKE_CXX_FLAGS})
message(${CMAKE_CXX_FLAGS})
include_directories(${PROJECT_SOURCE_DIR}/src)
include_directories(${PROJECT_SOURCE_DIR}/include)
include_directories(/usr/local/include)

if (JNI_FOUND)
  message (STATUS "JNI_INCLUDE_DIRS=${JNI_INCLUDE_DIRS}")
  message (STATUS "JNI_LIBRARIES=${JNI_LIBRARIES}")
endif()

set(UTIL ${PROJECT_SOURCE_DIR}/src/util)
set(DEMULTIPLEXER ${PROJECT_SOURCE_DIR}/src/demultiplexer)
set(CORE ${PROJECT_SOURCE_DIR}/src/core)
set(SERVICE ${PROJECT_SOURCE_DIR}/src/service)
set(EXTERNAL_SERVICE ${PROJECT_SOURCE_DIR}/src/external_service)
set(EXTERNAL_DEMULTIPLEXER ${PROJECT_SOURCE_DIR}/src/external_demultiplexer)

set(DEMULTIPLEXER_FILE ${DEMULTIPLEXER}/Proactor.cc ${DEMULTIPLEXER}/EqDemultiplexer.cc ${DEMULTIPLEXER}/EqHandler.cc ${DEMULTIPLEXER}/CqDemultiplexer.cc ${DEMULTIPLEXER}/RdmCqDemultiplexer.cc)
set(EXTERNAL_DEMULTIPLEXER_FILE ${EXTERNAL_DEMULTIPLEXER}/ExternalEqDemultiplexer.cc ${EXTERNAL_DEMULTIPLEXER}/ExternalCqDemultiplexer.cc ${EXTERNAL_DEMULTIPLEXER}/ExternalRdmCqDemultiplexer.cc)
set(CORE_FILE ${CORE}/MsgConnection.cc ${CORE}/MsgStack.cc ${CORE}/RdmConnection.cc ${CORE}/RdmStack.cc)
set(SERVICE_FILE ${SERVICE}/Service.cc ${SERVICE}/Server.cc ${SERVICE}/Client.cc)
set(EXTERNAL_SERVICE_FILE ${EXTERNAL_SERVICE}/ExternalEqService.cc)

option(WITH_PSM2 "enable PSM2 provider" OFF)
if(WITH_PSM2)
  add_definitions(-DPSM2)
endif(WITH_PSM2)

option(WITH_VERBS "enable VERBS provider" OFF)
if(WITH_VERBS)
  add_definitions(-DVERBS)
endif(WITH_VERBS)

add_subdirectory(test)
add_subdirectory(java)
