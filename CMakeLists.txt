cmake_minimum_required(VERSION 3.0.0)

set(PATCH_VERSION 1 CACHE INTERNAL "Patch version")
set(PROJECT_VERSION 0.0.${PATCH_VERSION})

project(task01_hello_world VERSION ${PROJECT_VERSION})

find_package(Boost COMPONENTS unit_test_framework REQUIRED)
configure_file(version.h.in version.h)

set(PROJECT_NAME_TEST ${PROJECT_NAME}_test)
set(PROJECT_LIB_NAME ${PROJECT_NAME}_lib)
set(SRC_H   lib.h)
set(SRC_CPP lib.cpp)

# cli
add_executable(${PROJECT_NAME} ${SRC_H} ${SRC_CPP} main.cpp)
target_include_directories(${PROJECT_NAME} PRIVATE ${CMAKE_BINARY_DIR})

# test
add_executable(${PROJECT_NAME_TEST} ${SRC_H} ${SRC_CPP} test_version.cpp)
set_target_properties(${PROJECT_NAME_TEST} PROPERTIES COMPILE_DEFINITIONS BOOST_TEST_DYN_LINK INCLUDE_DIRECTORIES ${Boost_INCLUDE_DIR})
target_link_libraries(${PROJECT_NAME_TEST} PUBLIC ${Boost_LIBRARIES})
target_include_directories(${PROJECT_NAME_TEST} PRIVATE ${CMAKE_BINARY_DIR})

enable_testing()
add_test(${PROJECT_NAME_TEST} ${PROJECT_NAME_TEST})

# deb
install(TARGETS ${PROJECT_NAME} RUNTIME DESTINATION bin)
set(CPACK_GENERATOR DEB)

set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")

set(CPACK_PACKAGE_CONTACT example@example.com)
include (CPack)


