project(obs-shaderfilter)

set(CMAKE_PREFIX_PATH "${QTDIR}")
set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)

#find_package(LibObs REQUIRED)
find_package(Qt5Core REQUIRED)
find_package(Qt5Widgets REQUIRED)

include_directories(
#	SYSTEM "${CMAKE_SOURCE_DIR}/libobs"
#	"${CMAKE_SOURCE_DIR}/UI/obs-frontend-api"
	${Qt5Core_INCLUDES}
	${Qt5Widgets_INCLUDES}
)

find_package(FFmpeg REQUIRED
	COMPONENTS avcodec)
include_directories(${FFMPEG_INCLUDE_DIRS})

if(MSVC)
	set(obs-shaderfilter_PLATFORM_DEPS
		w32-pthreads)
endif()

set(obs-shaderfilter_SOURCES
	src/obs-shaderfilter.c
)

add_library(obs-shaderfilter MODULE
	${obs-shaderfilter_SOURCES}
	${obs-shaderfilter_HEADERS}
)
	
target_link_libraries(obs-shaderfilter
	libobs
	Qt5::Core
	Qt5::Widgets
	${obs-shaderfilter_PLATFORM_DEPS}
	${FFMPEG_LIBRARIES}
#	obs-frontend-api
#	Qt5::Core
#	Qt5::Widgets
)

install_obs_plugin_with_data(obs-shaderfilter data)