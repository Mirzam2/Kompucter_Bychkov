cmake_minimum_required(VERSION 3.11)

set(CMAKE_CXX_FLAGS "-std=c++11 -g")

project(vtkdemo)

add_executable(Rotate Rotate.cpp)
add_executable(Collapce Collapce.cpp)
# Найдём VTK
find_package(VTK REQUIRED)
# ... и подключим её со всеми её компонентами её нативными средствами
include(${VTK_USE_FILE})

# gmsh укажем явно по кусочкам (потому что он был просто распакован руками)
add_library(gmsh SHARED IMPORTED)
set_target_properties(gmsh PROPERTIES
    IMPORTED_LOCATION "/usr/lib/x86_64-linux-gnu/libgmsh.so"
    INTERFACE_INCLUDE_DIRECTORIES "/usr/include"
)
set(GMSH_LIBRARY gmsh)

# если бы gmsh был поставлен прилично, то можно было бы примерно вот так
#find_package(gmsh REQUIRED)
#include_directories(${GMSH_INCLUDE_DIR})
#target_link_libraries(tetr3d ${GMSH_LIBRARY})

# Укажем библиотеки для линковки
# ... тут только VTK

# ... а тут ещё и gmsh

target_link_libraries(Rotate ${VTK_LIBRARIES})
target_link_libraries(Rotate ${GMSH_LIBRARY})
target_link_libraries(Collapce ${VTK_LIBRARIES})
target_link_libraries(Collapce ${GMSH_LIBRARY})