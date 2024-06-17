FILE(GLOB SRCS *.cpp)

#Skip snap and cell
#TODO: SNAP is outdates and should likely be removed all together as it is
list(FILTER SRCS EXCLUDE REGEX ".*lj_cell\\.cpp$")
list(FILTER SRCS EXCLUDE REGEX ".*snap_neigh\\.cpp$")

# Skip force-type module if Kokkos Remote Spaces is not enabled
if (ENABLE_KOKKOS_REMOTE_SPACES)
    message(STATUS "Building with support for force_lj_neigh_distrib")
    list(FILTER SRCS EXCLUDE REGEX ".*lj_neigh\\.cpp$")
    target_compile_definitions(ExaMiniMD PRIVATE EXAMINIMD_ENABLE_KOKKOS_REMOTE_SPACES)
    target_compile_definitions(ExaMiniMD PRIVATE SHMEMTESTS_USE_SCALAR)
    #target_compile_definitions(ExaMiniMD PRIVATE SHMEMTESTS_USE_HALO)
    #target_compile_definitions(ExaMiniMD PRIVATE SHMEMTESTS_USE_HALO_LOCAL)
    #target_compile_definitions(ExaMiniMD PRIVATE SHMEMTESTS_USE_LOCAL_GLOBAL)
    target_compile_definitions(ExaMiniMD PRIVATE SHMEMTESTS_USE_GLOBAL)
else()
    #Otherwise exclude
    list(FILTER SRCS EXCLUDE REGEX ".*distrib\\.cpp$")
endif()



target_sources(ExaMiniMD PRIVATE ${SRCS})
