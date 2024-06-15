enable_testing()
find_package(GTest REQUIRED)

add_executable(cps-tests loader.cpp utils.cpp version.cpp)
target_link_libraries(cps-tests PRIVATE libcps)
target_link_libraries(cps-tests PRIVATE GTest::gtest GTest::gtest_main)
add_test(CpsTests cps-tests)
