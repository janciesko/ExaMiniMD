FILE(GLOB SRCS *.cpp)


#Skip lj_ideal, snap and cell
#TODO: SNAP is outdates and should likely be removed all together as it is
list(FILTER SRCS EXCLUDE REGEX ".*lj_cell\\.cpp$")
list(FILTER SRCS EXCLUDE REGEX ".*lj_idial_neigh\\.cpp$")
list(FILTER SRCS EXCLUDE REGEX ".*snap_neigh\\.cpp$")

target_sources(ExaMiniMD PRIVATE ${SRCS})

