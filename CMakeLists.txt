cmake_minimum_required(VERSION 4.0)
project(usdex 
    VERSION ${SKBUILD_PROJECT_VERSION}
    LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)

find_package(Python COMPONENTS Interpreter Development)
find_package(pybind11 CONFIG)

find_package(cxxopts REQUIRED)
find_package(pxr REQUIRED)

add_library(usdex_core
    source/core/library/AssetStructure.cpp
    source/core/library/CameraAlgo.cpp
    source/core/library/Core.cpp
    source/core/library/CurvesAlgo.cpp
    source/core/library/Debug.cpp
    source/core/library/Diagnostics.cpp
    source/core/library/GprimAlgo.cpp
    source/core/library/LayerAlgo.cpp
    source/core/library/LightAlgo.cpp
    source/core/library/MaterialAlgo.cpp
    source/core/library/MeshAlgo.cpp
    source/core/library/NameAlgo.cpp
    source/core/library/PhysicsJointAlgo.cpp
    source/core/library/PhysicsMaterialAlgo.cpp
    source/core/library/PointsAlgo.cpp
    source/core/library/PrimvarData.cpp
    source/core/library/Settings.cpp
    source/core/library/StageAlgo.cpp
    source/core/library/TfUtils.cpp
    source/core/library/Transcoding.cpp
    source/core/library/XformAlgo.cpp
)
target_include_directories(usdex_core
    PUBLIC include)
target_link_libraries(usdex_core
    ar
    arch
    gf
    kind
    pcp
    plug
    sdf
    tf
    usd
    usdGeom
    usdLux
    usdPhysics
    usdShade
    usdUtils
    vt
    work
)

pybind11_add_module(_usdex_core
    source/core/python/bindings/Module.cpp
)
target_link_libraries(_usdex_core
    PRIVATE usdex_core
)
install(
    TARGETS _usdex_core
    DESTINATION ${SKBUILD_PROJECT_NAME}/core
    COMPONENT PythonModule
)

set(CORE_PYTHON_SOURCES
    source/core/python/__init__.py
    source/core/python/_AssetStructureBindings.py
    source/core/python/_StageAlgoBindings.py
)
install(
    FILES ${CORE_PYTHON_SOURCES}
    DESTINATION ${SKBUILD_PROJECT_NAME}/core
    COMPONENT PythonModule
)
