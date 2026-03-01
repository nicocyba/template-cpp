cmake_minimum_required(VERSION 3.20)
project(MyApp
    VERSION 0.1.0
    LANGUAGES CXX
    DESCRIPTION "My Description"
)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# ---------------------------------------------------------------------------
# Build options
# ---------------------------------------------------------------------------

option(BUILD_BACKEND "Build the backend"   ON)
option(BUILD_FRONTEND "Build the frontend" ON)

# ---------------------------------------------------------------------------
# Sub-projects
# ---------------------------------------------------------------------------

if(BUILD_BACKEND)
    message(STATUS "[MyApp] configuring backend")
    add_subdirectory(backend)
endif()

if(BUILD_FRONTEND)
    message(STATUS "[MyApp] configuring frontend")
    add_subdirectory(frontend)
endif()

# ---------------------------------------------------------------------------
# Developer notes
# ---------------------------------------------------------------------------
# Quick build (both):
#   cmake -B build -DCMAKE_BUILD_TYPE=RelWithDebInfo
#   cmake --build build -j$(nproc)
#
# Backend only:
#   cmake -B build -DBUILD_GUI=OFF
#   cmake --build build -j$(nproc)
#
# ---------------------------------------------------------------------------
