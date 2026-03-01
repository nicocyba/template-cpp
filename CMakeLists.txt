cmake_minimum_required(VERSION 3.20)
project(gamebot_wow_wotlk
    VERSION 0.1.0
    LANGUAGES CXX
    DESCRIPTION "WOW WOTLK game-bot with gRPC GUI visualizer"
)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# ---------------------------------------------------------------------------
# Build options
# ---------------------------------------------------------------------------

option(BUILD_BACKEND "Build the bot backend process"   ON)
option(BUILD_FRONTEND "Build the bot frontend process" ON)

# ---------------------------------------------------------------------------
# Sub-projects
# ---------------------------------------------------------------------------

if(BUILD_BACKEND)
    message(STATUS "[gamebot] configuring bot backend")
    add_subdirectory(backend)
endif()

if(BUILD_FRONTEND)
    message(STATUS "[gamebot] configuring bot frontend")
    add_subdirectory(frontend)
endif()

# ---------------------------------------------------------------------------
# Developer notes
# ---------------------------------------------------------------------------
# Quick build (both):
#   cmake -B build -DCMAKE_BUILD_TYPE=RelWithDebInfo
#   cmake --build build -j$(nproc)
#
# Bot only:
#   cmake -B build -DBUILD_GUI=OFF
#   cmake --build build -j$(nproc)
#
# Run:
#   ./build/bot/darkorbit_bot [address]    # default: localhost:50051
#   ./build/gui/darkorbit_gui [--address=host:port] [--headless]
# ---------------------------------------------------------------------------
