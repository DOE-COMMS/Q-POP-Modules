find_package(DOLFIN REQUIRED)
find_package(Boost REQUIRED COMPONENTS timer)
add_executable(qpop-imt)
target_sources(
  qpop-imt PRIVATE main.cpp ${PROJECT_SOURCE_DIR}/external/pugixml/pugixml.cpp
                   imt_2tdevice.h)
target_include_directories(
  qpop-imt PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}
                  ${PROJECT_SOURCE_DIR}/external/pugixml ${DOLFIN_INCLUDE_DIRS})
target_link_libraries(qpop-imt ${DOLFIN_LIBRARIES} Boost::timer Boost::boost)

add_custom_target(
  compile_ufl ALL
  DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/imt_2tdevice.h
  COMMENT "Compile imt_2tdevice.ufl file")

add_custom_command(
  OUTPUT ${CMAKE_CURRENT_SOURCE_DIR}/imt_2tdevice.h
  DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/imt_2tdevice.ufl
  WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
  COMMAND ffc -l dolfin imt_2tdevice.ufl
  COMMENT "Create imt_2tdevice.h by, ffc -l dolfin ${CMAKE_CURRENT_SOURCE_DIR}/imt_2tdevice.ufl")

add_dependencies(qpop-imt compile_ufl)
