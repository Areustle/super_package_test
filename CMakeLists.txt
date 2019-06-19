cmake_minimum_required(VERSION 3.12)

set(as_subproject dep pkg0)

macro(find_package)
  if(NOT "${ARGV0}" IN_LIST as_subproject)
    _find_package(${ARGV})
  endif()
endmacro()

add_subdirectory(pkg0)
add_subdirectory(dep)
