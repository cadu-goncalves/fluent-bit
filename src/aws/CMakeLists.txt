include_directories(
  ${CMAKE_CURRENT_SOURCE_DIR}
  ${CMAKE_CURRENT_BINARY_DIR}
  )

set(src
  "flb_aws_util.c"
  "flb_aws_imds.c"
  )

add_library(flb-aws STATIC ${src})

if(FLB_JEMALLOC)
  target_link_libraries(flb-aws libjemalloc)
endif()
