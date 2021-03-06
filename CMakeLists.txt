project (dlfcn C)

cmake_minimum_required(VERSION 2.8)

option(BUILD_SHARED_LIBS "shared/static libs" ON) 
option(USE_WINE "use wine for test" ON)
option(BUILD_TESTS "tests?" OFF)

set(headers dlfcn.h)
set(sources dlfcn.c)

add_library(dl ${sources})

install (TARGETS dl RUNTIME DESTINATION bin
                    LIBRARY DESTINATION lib${LIB_SUFFIX}
                    ARCHIVE DESTINATION lib${LIB_SUFFIX})

install (FILES ${headers} DESTINATION include)

if (BUILD_TESTS)
  enable_testing()
  if (USE_WINE)
    find_program(WINE_EXECUTABLE NAMES wine)
  endif ()
  add_library(testdll SHARED testdll.c)
  set_target_properties(testdll PROPERTIES PREFIX "")
  add_executable(t_dlfcn test.c)
  target_link_libraries(t_dlfcn dl)
  add_test ( NAME t_dlfcn COMMAND ${WINE_EXECUTABLE} t_dlfcn${CMAKE_EXECUTABLE_SUFFIX} )
endif ()
