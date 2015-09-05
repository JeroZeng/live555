cmake_minimum_required(VERSION 2.8)

project(live555)

include_directories(${CMAKE_SOURCE_DIR}/BasicUsageEnvironment/include)
include_directories(${CMAKE_SOURCE_DIR}/groupsock/include)
include_directories(${CMAKE_SOURCE_DIR}/liveMedia/include)
include_directories(${CMAKE_SOURCE_DIR}/UsageEnvironment/include)

if(MSVC)
add_definitions(-D_CRT_SECURE_NO_WARNINGS)
add_definitions(-D_WINSOCK_DEPRECATED_NO_WARNINGS)
endif()

file(GLOB BasicUsageEnvironmentFiles
	BasicUsageEnvironment/*.cpp)
add_library(BasicUsageEnvironment
			${BasicUsageEnvironmentFiles})
			
file(GLOB groupsockfiles
	groupsock/*.cpp groupsock/*.c)
add_library(groupsock ${groupsockfiles})

file(GLOB liveMediaFiles
		liveMedia/*.hh liveMedia/*.cpp liveMedia/*.c)	
add_library(liveMedia ${liveMediaFiles})

file(GLOB UsageEnvironmentFiles 
    UsageEnvironment/*.cpp)
add_library(UsageEnvironment ${UsageEnvironmentFiles})

file(GLOB mediaServerFiles
	mediaServer/*.hh mediaServer/*.cpp)
add_executable(mediaServer ${mediaServerFiles})
if(MSVC)
	target_link_libraries(mediaServer ws2_32.lib)
endif()
target_link_libraries(mediaServer  
				BasicUsageEnvironment groupsock
				UsageEnvironment liveMedia)