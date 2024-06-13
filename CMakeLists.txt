cmake_minimum_required(VERSION 3.16)

project(ExaMiniMD LANGUAGES CXX)

include(GNUInstallDirs)

find_package(Kokkos REQUIRED)

option(ENABLE_MPI "Whether to build with MPI" OFF)
option(ENABLE_KOKKOS_REMOTE_SPACES "Whether to build with Kokkos Remote Spaces" OFF)

if (ENABLE_MPI)
  find_package(MPI REQUIRED CXX)
  message(STATUS "Building with MPI support")
  set(BACKEND_NAME MPI)
  list(APPEND BACKENDS ${BACKEND_NAME})
endif()

if (ENABLE_KOKKOS_REMOTE_SPACES)
  find_package(MPI REQUIRED CXX)
  find_package(KokkosRemoteSpaces REQUIRED)
  message(STATUS "Building with Kokkos Remote Spaces support")
  set(BACKEND_NAME KokkosRemoteSpaces)
  list(APPEND BACKENDS ${BACKEND_NAME})
endif()

list(LENGTH BACKENDS N_BACKENDS)
if (${N_BACKENDS} GREATER_EQUAL "2")
  message(FATAL_ERROR "Must specify a single valid communiction implementation. ${N_BACKENDS} given")
endif()

if (${N_BACKENDS} EQUAL "0")
  message(STATUS "Building with serial comm type")
endif()


add_subdirectory(src)
add_subdirectory(input)
