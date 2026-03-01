# template-cpp

## Setup
Protobuf:
```
sudo apt update
sudo apt install -y protobuf-compiler libprotobuf-dev
protoc --version  # Verify installation
protoc --cpp_out=. person.proto
```

Git submodules
```bash
git clone --recurse-submodules https://github.com/user/imgui-vulkan-example.git
```