#************************************************************
#* Author       : ZhuHaiWen                                 *
#* Email        : zhuhw@ihep.ac.cn/zhwren0211@whu.edu.cn    *
#* Last modified: 2015-10-27 16:41:1445935314               *
#* Filename     : CMakeLists.txt                            *
#* Phone Number : 18625272373                               *
#* Discription  :                                           *
#************************************************************
#------------------------------------------------------------
# Setup the project
CMAKE_MINIMUM_REQUIRED(VERSION 2.6 FATAL_ERROR)
PROJECT(main.exe)
#----------------------------------------------------------------------------
# Find Geant4 package, activating all available UI and Vis drivers by default
# You can set WITH_GEANT4_UIVIS to OFF via the command line or ccmake/cmake-gui
# to build a batch mode only executable
#
OPTION(WITH_GEANT4_UIVIS "Build example with Geant4 UI and Vis drivers" ON)
IF(WITH_GEANT4_UIVIS)
  FIND_PACKAGE(Geant4 REQUIRED ui_all vis_all)
ELSE()
  FIND_PACKAGE(Geant4 REQUIRED)
ENDIF()
#---------------------------------------------------------
# Setup Geant4 include directories and compile definitions
#
INCLUDE(${Geant4_USE_FILE})
#---------------------------------------------------------
# Find ROOT (required package)
#
FIND_PACKAGE(ROOT REQUIRED)
#---------------------------------------------------------
# Locate sources and headers for this project
#
INCLUDE_DIRECTORIES(${PROJECT_SOURCE_DIR}/include
                    ${Geant4_INCLUDE_DIR}
                    ${ROOT_INCLUDE_DIR})
FILE(GLOB sources ${PROJECT_SOURCE_DIR}/src/*.cc)
FILE(GLOB headers ${PROJECT_SOURCE_DIR}/include/*.hh)
#---------------------------------------------------------
# Add the executable, and link it to the Geant4 libraries
#
ADD_EXECUTABLE(main.exe main.cc ${sources} ${headers})
TARGET_LINK_LIBRARIES(main.exe ${Geant4_LIBRARIES} ${ROOT_LIBRARIES} m stdc++ fftw3)
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-shadow -std=c++11")
