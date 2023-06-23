add_executable(qpop-imt)
target_sources(
  qpop-imt PRIVATE main.cpp ${PROJECT_SOURCE_DIR}/external/pugixml/pugixml.cpp)
target_include_directories(
  qpop-imt PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}
                  ${PROJECT_SOURCE_DIR}/external/pugixml/)
