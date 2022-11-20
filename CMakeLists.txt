cmake_minimum_required(VERSION 3.9)
project(McIniData VERSION 1.0)

list(APPEND CMAKE_PREFIX_PATH $ENV{ROOTSYS})
find_package(ROOT REQUIRED)

if (NOT ROOT_FOUND)
    message(WARNING "ROOT was not found with CMAKE_PREFIX_PATH...")
    list(APPEND CMAKE_MODULE_PATH $ENV{ROOTSYS}/etc/cmake)
    find_package(ROOT REQUIRED)
endif ()

if (ROOT_USE_FILE)
    message("-- ROOT_USE_FILE ${ROOT_USE_FILE}")
    include(${ROOT_USE_FILE})
endif ()

# if(CMAKE_CXX_COMPILER_VERSION VERSION_LESS 4.9.0)
message(WARNING "GCC compiler is older than 4.9.0. Boost library must be used!")
find_package(Boost REQUIRED COMPONENTS regex)
include_directories(${Boost_INCLUDE_DIRS})
# endif ()

include_directories(include ${CMAKE_SOURCE_DIR})

ROOT_GENERATE_DICTIONARY(
        G__McIniData include/Nucleon.h include/EventInitialState.h include/URun.h include/UEvent.h include/UParticle.h 
        LINKDEF include/MCIni-LinkDef.h
)

add_library(McIniData SHARED G__McIniData.cxx src/URun.cxx src/UEvent.cxx src/UParticle.cxx src/Nucleon.cxx src/EventInitialState.cxx)
target_link_libraries(McIniData PRIVATE ${ROOT_LIBRARIES})

# Find GNU installation directories
include(GNUInstallDirs)

# Set public headers
# cling tends to output weird messages if version isn't specialised
set_target_properties(McIniData PROPERTIES
        VERSION 1.0
        SOVERSION 1
        PUBLIC_HEADER "include/BasicTypes.h;include/EventInitialState.h;include/Nucleon.h;include/UEvent.h;include/UParticle.h;include/URun.h")

# Install the library and TObject-related files with it
install(TARGETS McIniData
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/libMcIniData.rootmap ${CMAKE_CURRENT_BINARY_DIR}/libMcIniData_rdict.pcm
        DESTINATION ${CMAKE_INSTALL_LIBDIR})

add_executable(JAMReader src/jam/JAMRunInfo.cxx src/jam/JAMReader.cxx)
# if(CMAKE_CXX_COMPILER_VERSION VERSION_LESS 4.9.0)
target_link_libraries(JAMReader PUBLIC McIniData ${ROOT_LIBRARIES} ${Boost_LIBRARIES})
# else()
# target_link_libraries(JAMReader PUBLIC McIniData ${ROOT_LIBRARIES})
# endif ()

