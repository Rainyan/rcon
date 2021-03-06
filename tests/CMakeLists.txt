CMAKE_MINIMUM_REQUIRED(VERSION 3.0)

PKG_CHECK_MODULES(CHECK REQUIRED check)

INCLUDE_DIRECTORIES(${CMAKE_SOURCE_DIR} ${CHECK_INCLUDE_DIRS})
ADD_DEFINITIONS(${CHECK_CFLAGS})

SET(TESTS "srcrcontest")

FOREACH(TEST ${TESTS})
  SET(SOURCES "../srcrcon.c" "../memstream.c" "../fmemopen.c")
  ADD_EXECUTABLE(${TEST} "${TEST}.c" ${SOURCES})
  ADD_TEST(NAME ${TEST} COMMAND ${CMAKE_CURRENT_BINARY_DIR}/${TEST})
  TARGET_LINK_LIBRARIES("${TEST}" ${CHECK_LIBRARIES} ${CHECK_LDFLAGS})
  IF (NOT HAVE_ARC4RANDOM_UNIFORM)
    INCLUDE_DIRECTORIES(${BSD_INCLUDE_DIRS})
    TARGET_LINK_LIBRARIES(${TEST} ${BSD_LIBRARIES})
  ENDIF()
ENDFOREACH()
