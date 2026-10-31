cmake_minimum_required(VERSION 3.14)
project(MyCppProject CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 1. Подключаем GoogleTest (скачается автоматически при сборке)
include(FetchContent)
FetchContent_Declare(
  googletest
  URL https://github.com/google/googletest/archive/refs/tags/v1.14.0.zip
)
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)

# 2. Включаем тестирование
enable_testing()

# 3. Функция-помощник, чтобы не дублировать код для каждого файла
# Она создает executable из файла тестов и линкует его с gtest
function(add_my_test test_name test_file)
    add_executable(${test_name} ${test_file})
    target_link_libraries(${test_name} PRIVATE GTest::gtest_main)
    target_include_directories(${test_name} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})
    add_test(NAME ${test_name} COMMAND ${test_name})
endfunction()

# 4. Добавляем твои тесты
add_my_test(array_tests array_tests.cpp)
add_my_test(optional_tests optional_tests.cpp)
add_my_test(unique_ptr_tests unique_ptr_tests.cpp)
