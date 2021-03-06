# @@@LICENSE
#
#      Copyright (c) 2008-2013 LG Electronics, Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
# LICENSE@@@

set(SOURCE
    base.c
    callmap.c
    clock.c
    debug_methods.c
    mainloop.c
    message.c
    subscription.c
    timersource.c
    transport.c
    transport_channel.c
    transport_client.c
    transport_incoming.c
    transport_message.c
    transport_outgoing.c
    transport_security.c
    transport_serial.c
    transport_shm.c
    transport_signal.c
    transport_utils.c
    utils.c
    )

add_definitions(-DG_LOG_DOMAIN="LunaService")

set(LIBRARIES
    ${GLIB2_LDFLAGS}
    ${GTHREAD2_LDFLAGS}
    ${CJSON_LDFLAGS}
    dl
    pthread
    rt
    )

set(TESTLIB_PREFIX "_test")
set(TESTLIBNAME ${CMAKE_PROJECT_NAME}${TESTLIB_PREFIX})
add_library(${CMAKE_PROJECT_NAME} SHARED ${SOURCE})
add_library(${TESTLIBNAME} SHARED ${SOURCE})
target_link_libraries(${CMAKE_PROJECT_NAME} ${LIBRARIES})
target_link_libraries(${TESTLIBNAME} ${LIBRARIES})
webos_build_library()

SET_TARGET_PROPERTIES(${TESTLIBNAME} PROPERTIES COMPILE_FLAGS "-DLUNA_SERVICE_UNIT_TEST")

# Unit tests are compiled every time, to ensure that errors are thrown
# if a functions API is changed, but the unit tests are not updated
add_subdirectory(test)
