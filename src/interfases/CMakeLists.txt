cmake_minimum_required(VERSION 3.5)
project(interfases)

# Default to C99
if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(ament_cmake_python REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclpy REQUIRED)
#include(CSharpUtilities)
#add_executable(grpc_client scripts/client.cs)



find_package(Protobuf REQUIRED)

find_package(PkgConfig REQUIRED)
pkg_search_module(GRPC REQUIRED grpc)

# uncomment the following section in order to fill in
# further dependencies manually.
# find_package(<dependency> REQUIRED)\



include_directories(include/interfases)
add_library(multiplicador SHARED src/multiplicador.cpp)

ament_export_targets(multiplicador HAS_LIBRARY_TARGET)

install(
  DIRECTORY include/interfases
  DESTINATION include
)

install(TARGETS multiplicador
    EXPORT multiplicador
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION include
)


ament_python_install_package(${PROJECT_NAME})
install(
   PROGRAMS  scripts/image.py  protobuff/grpc_server.py
   DESTINATION lib/${PROJECT_NAME}
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # uncomment the line when a copyright and license is not present in all source files
  #set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # uncomment the line when this package is not in a git repo
  #set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
