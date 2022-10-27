set(COMMON_SOURCES 
	src/SerialPort.cpp
	src/SerialPortBase.cpp
	src/SerialPortInfo.cpp
	src/SerialPortInfoBase.cpp

	include/CSerialPort/ithread.hpp
	include/CSerialPort/osplatformutil.h
	include/CSerialPort/SerialPort.h
	include/CSerialPort/SerialPort_global.h
	include/CSerialPort/SerialPort_version.h
	include/CSerialPort/SerialPortBase.h
	include/CSerialPort/SerialPortInfo.h
	include/CSerialPort/SerialPortInfoBase.h
	include/CSerialPort/sigslot.h
	)
	
if (CMAKE_HOST_WIN32)
	set(OS_ABOUT_SOURCES src/SerialPortInfoWinBase.cpp src/SerialPortWinBase.cpp include/CSerialPort/SerialPortInfoWinBase.h include/CSerialPort/SerialPortWinBase.h )
elseif (CMAKE_HOST_UNIX)
	set(OS_ABOUT_SOURCES src/SerialPortInfoUnixBase.cpp src/SerialPortUnixBase.cpp include/CSerialPort/SerialPortInfoUnixBase.h include/CSerialPort/SerialPortUnixBase.h )
endif ()

if (WIN32)
	# for function availableFriendlyPorts
	set(LIBS setupapi)
elseif(APPLE)
    find_library(IOKIT_LIBRARY IOKit)
    find_library(FOUNDATION_LIBRARY Foundation)
	set(LIBS ${FOUNDATION_LIBRARY} ${IOKIT_LIBRARY})
elseif (UNIX)
	set(LIBS pthread)
endif()

set(SRCS ${COMMON_SOURCES} ${OS_ABOUT_SOURCES}
	)
set(INCS ${CMAKE_CURRENT_SOURCE_DIR}/include)
__add_real_target(cserialport lib SOURCE ${SRCS} LIB ${LIBS} INC ${INCS} INTERFACE ${INCS})