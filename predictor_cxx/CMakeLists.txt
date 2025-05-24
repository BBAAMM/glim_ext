cmake_minimum_required(VERSION 3.7)
project(predictor_cxx)

find_package(glim REQUIRED)

add_library(predictor_cxx
  src/glim_ext/predictor.cxx)
message(WARNING "This module is not maintained. You should expect that you need several patches to make it work.")

target_include_directories(predictor_cxx PRIVATE
  include
)
target_link_libraries(predictor_cxx
  glim_ext
)
target_compile_options(predictor_cxx PRIVATE
  -std=c++17
)
