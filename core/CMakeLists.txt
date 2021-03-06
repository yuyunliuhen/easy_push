CMAKE_MINIMUM_REQUIRED(VERSION 2.8)
AUX_SOURCE_DIRECTORY( . SOURCE_DIRECTORY)	
ADD_EXECUTABLE(core ${SOURCE_DIRECTORY})
INCLUDE_DIRECTORIES( ../common ../dep/easy/src/base ../dep/tinynet ../dep/easy/dep/protobuf/src)

IF(UNIX)
	OPTION(USE_32BITS "Compile for 32bits processors(linux only)" OFF)
	#	32 bit
	IF(USE_32BITS)  
		message(STATUS "using 32bits")  
		SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -m32")  
		SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -m32")   
	#	64 bit
	ELSE()  
		message(STATUS "using 64bits")  
		SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -m64")  
		SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -m64")    
	ENDIF(USE_32BITS)  
	SET(CMAKE_CXX_FLAGS "$ENV{CXXFLAGS} -O0 -Wall -g -ggdb -fpermissive -L../dep/protobuf/src/.libs")
ELSE(WIN32)

ENDIF(UNIX)

IF(UNIX)
	TARGET_LINK_LIBRARIES(core tinynet easy protobuf proto pthread)
ELSEIF(WIN32)
	#	64 bit
	TARGET_LINK_LIBRARIES(core ws2_32 tinynet easy ../dep/protobuf/src/.libs/libprotobuf proto
	#	absolute path should add .lib, just like as follows:
	c:/local/boost_1_53_0/lib64-msvc-11.0/libboost_thread-vc110-mt-gd-1_53.lib 
	c:/local/boost_1_53_0/lib64-msvc-11.0/libboost_system-vc110-mt-gd-1_53.lib  
	c:/local/boost_1_53_0/lib64-msvc-11.0/libboost_date_time-vc110-mt-gd-1_53.lib 
	c:/local/boost_1_53_0/lib64-msvc-11.0/libboost_chrono-vc110-mt-gd-1_53.lib)
	#	32 bit
	#TARGET_LINK_LIBRARIES(core tinynet ../../easy/dep/protobuf/vsprojects/Debug/libprotobuf proto ws2_32)
ENDIF(UNIX)
SET(EXECUTABLE_OUTPUT_PATH ../bin)


