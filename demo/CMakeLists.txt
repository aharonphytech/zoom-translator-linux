cmake_minimum_required(VERSION 3.16)

project(meetingSDKDemo CXX)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
add_definitions(-std=c++11)
set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)

find_package(PkgConfig REQUIRED)
find_package(ZLIB REQUIRED)

# Find GLib using pkg-config
pkg_check_modules(GLIB REQUIRED glib-2.0)
pkg_check_modules(GIO REQUIRED gio-2.0)

include_directories(${CMAKE_SOURCE_DIR}/include)
include_directories(${CMAKE_SOURCE_DIR}/include/h)


# Include GLib directories
include_directories(${GLIB_INCLUDE_DIRS} ${GIO_INCLUDE_DIRS})
link_directories(${GLIB_LIBRARY_DIRS} ${GIO_LIBRARY_DIRS})
add_definitions(${GLIB_CFLAGS_OTHER} ${GIO_CFLAGS_OTHER})


include_directories(/usr/include/glib-2.0/)
include_directories(/usr/include/glib-2.0/glib)
include_directories(/usr/lib/x86_64-linux-gnu/glib-2.0/include/)
include_directories(/usr/lib64/glib-2.0/include/)


link_directories(${CMAKE_SOURCE_DIR}/lib/zoom_meeting_sdk)
link_directories(${CMAKE_SOURCE_DIR}/lib/zoom_meeting_sdk/qt_libs)
link_directories(${CMAKE_SOURCE_DIR}/lib/zoom_meeting_sdk/qt_libs/Qt/lib)


add_executable(meetingSDKDemo 
              ${CMAKE_SOURCE_DIR}/meeting_sdk_demo.cpp
              ${CMAKE_SOURCE_DIR}/MeetingReminderEventListener.h
              ${CMAKE_SOURCE_DIR}/MeetingReminderEventListener.cpp
              ${CMAKE_SOURCE_DIR}/MeetingServiceEventListener.h
              ${CMAKE_SOURCE_DIR}/MeetingServiceEventListener.cpp
              ${CMAKE_SOURCE_DIR}/NetworkConnectionHandler.h
              ${CMAKE_SOURCE_DIR}/NetworkConnectionHandler.cpp
              ${CMAKE_SOURCE_DIR}/AuthServiceEventListener.h
              ${CMAKE_SOURCE_DIR}/AuthServiceEventListener.cpp
              ${CMAKE_SOURCE_DIR}/MeetingParticipantsCtrlEventListener.h
              ${CMAKE_SOURCE_DIR}/MeetingParticipantsCtrlEventListener.cpp
              ${CMAKE_SOURCE_DIR}/MeetingRecordingCtrlEventListener.h
              ${CMAKE_SOURCE_DIR}/MeetingRecordingCtrlEventListener.cpp
              ${CMAKE_SOURCE_DIR}/ZoomSDKRenderer.h
              ${CMAKE_SOURCE_DIR}/ZoomSDKRenderer.cpp
              ${CMAKE_SOURCE_DIR}/ZoomSDKAudioRawData.h
              ${CMAKE_SOURCE_DIR}/ZoomSDKAudioRawData.cpp
              ${CMAKE_SOURCE_DIR}/ZoomSDKVideoSource.h
              ${CMAKE_SOURCE_DIR}/ZoomSDKVideoSource.cpp
              ${CMAKE_SOURCE_DIR}/ZoomSDKVirtualAudioMicEvent.h
              ${CMAKE_SOURCE_DIR}/ZoomSDKVirtualAudioMicEvent.cpp
              )

# Link GLib libraries
target_link_libraries(meetingSDKDemo ${GLIB_LIBRARIES} ${GIO_LIBRARIES})

target_link_libraries(meetingSDKDemo gcc_s gcc)
target_link_libraries(meetingSDKDemo meetingsdk)
target_link_libraries(meetingSDKDemo glib-2.0)
target_link_libraries(meetingSDKDemo curl)
target_link_libraries(meetingSDKDemo pthread)


# Create a symbolic link
# execute_process(COMMAND ln -s libmeetingsdk.so libmeetingsdk.so.1
#     WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}/lib/zoom_meeting_sdk
# )

configure_file(${CMAKE_SOURCE_DIR}/config.txt ${CMAKE_SOURCE_DIR}/bin/config.txt COPYONLY)

file(COPY ${CMAKE_SOURCE_DIR}/lib/zoom_meeting_sdk/ DESTINATION ${CMAKE_SOURCE_DIR}/bin)
