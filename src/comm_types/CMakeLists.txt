FILE(GLOB SRCS *.cpp)
target_sources(ExaMiniMD PRIVATE ${SRCS})

if (!ENABLE_MPI AND !ENABLE_KOKKOS_REMOTE_SPACES) 
    # Skip MPI module
    list(FILTER SRCS EXCLUDE REGEX ".*comm_mpi\\.cpp$")
endif()

target_sources(ExaMiniMD PRIVATE ${SRCS})
