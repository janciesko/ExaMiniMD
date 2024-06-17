FILE(GLOB SRCS *.cpp)
add_executable(ExaMiniMD ${SRCS})

set(SUBDIRECTORIES
  binning_types
  comm_types
  force_types
  neighbor_types)

foreach(SUBDIR ${SUBDIRECTORIES})
  add_subdirectory(${SUBDIR})
endforeach()

if(ENABLE_MPI)
   target_compile_definitions(ExaMiniMD PRIVATE EXAMINIMD_ENABLE_MPI)
endif()

target_include_directories(ExaMiniMD PRIVATE ${Kokkos_DIR} ${CMAKE_CURRENT_SOURCE_DIR} ${SUBDIRECTORIES})
target_link_libraries(ExaMiniMD PRIVATE $<$<BOOL:${ENABLE_MPI}>:MPI::MPI_CXX> Kokkos::kokkos $<$<BOOL:${ENABLE_KOKKOS_REMOTE_SPACES}>:Kokkos::kokkosremotespaces>)

install(TARGETS ExaMiniMD DESTINATION ${CMAKE_INSTALL_BINDIR})