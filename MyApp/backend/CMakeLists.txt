

# ---------------------------------------------------------------------------
# Dependencies
# ---------------------------------------------------------------------------

find_package(Protobuf REQUIRED)


# ---------------------------------------------------------------------------
# Protobuf / gRPC code generation
# ---------------------------------------------------------------------------

# Locate the protoc and grpc_cpp_plugin executables supplied by gRPC
# get_target_property(GRPC_CPP_PLUGIN_EXECUTABLE gRPC::grpc_cpp_plugin IMPORTED_LOCATION)

set(PROTO_FILE "${CMAKE_SOURCE_DIR}/../proto/bot_service.proto")
set(PROTO_OUT  "${CMAKE_CURRENT_BINARY_DIR}/proto")
file(MAKE_DIRECTORY "${PROTO_OUT}")

# Run protoc to generate .pb.cc / .grpc.pb.cc
add_custom_command(
    OUTPUT
        "${PROTO_OUT}/service.pb.cc"
        "${PROTO_OUT}/service.pb.h"
        # "${PROTO_OUT}/service.grpc.pb.cc"
        # "${PROTO_OUT}/service.grpc.pb.h"
    COMMAND protobuf::protoc
        --cpp_out="${PROTO_OUT}"
        --grpc_out="${PROTO_OUT}"
        # --plugin=protoc-gen-grpc="${GRPC_CPP_PLUGIN_EXECUTABLE}"
        -I "${CMAKE_SOURCE_DIR}/../proto"
        "${PROTO_FILE}"
    DEPENDS "${PROTO_FILE}"
    COMMENT "Generating protobuf/gRPC sources"
)

add_library(backend_proto STATIC
    "${PROTO_OUT}/bot_service.pb.cc"
    # "${PROTO_OUT}/bot_service.grpc.pb.cc"
)
target_include_directories(backend_proto PUBLIC "${PROTO_OUT}")
target_link_libraries(backend_proto
    PUBLIC
        # gRPC::grpc++
        protobuf::libprotobuf
)

# ---------------------------------------------------------------------------
# executable
# ---------------------------------------------------------------------------

add_executable(MyAppTarget
    src/main.cpp
)

target_include_directories(MyAppTarget PRIVATE src)

target_link_libraries(MyAppTarget
    PRIVATE
        backend_proto
        # gRPC::grpc++
        # gRPC::grpc++_reflection
        # spdlog::spdlog
        # Threads::Threads
)

# Compiler warnings
target_compile_options(MyAppTarget PRIVATE
    $<$<CXX_COMPILER_ID:GNU,Clang>:-Wall -Wextra -Wpedantic>
)
