# Development Guide

This document provides guidelines for setting up a development environment, building, testing, and contributing to verifirewall.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Environment Setup](#environment-setup)
- [Building the Project](#building-the-project)
- [Running Tests](#running-tests)
- [Code Style](#code-style)
- [Static Analysis](#static-analysis)
- [Debugging](#debugging)
- [Creating Packages](#creating-packages)
- [Docker Development](#docker-development)

## Prerequisites

### Required Tools

- **CMake** 3.16 or higher
- **C++ Compiler** with C++17 support (GCC 8+, Clang 7+, MSVC 2019+)
- **Git** for version control

### Required Libraries (Development Headers)

| Library | Minimum Version | Purpose |
|---------|----------------|---------|
| Boost | 1.65+ | Core utilities, system, filesystem |
| OpenSSL | 1.1.1+ | Cryptography, TLS |
| PCRE2 | 10.30+ | Regular expressions |
| libxml2 | 2.9+ | XML parsing |
| GTest | 1.10+ | Unit testing |
| GMock | 1.10+ | Mocking framework |
| cURL | 7.60+ | HTTP client |
| Redis | 5.0+ | In-memory data store |
| Hiredis | 0.14+ | Redis C client |
| MaxmindDB | 1.5+ | GeoIP database |
| yq | 4.0+ | YAML processor |

### Installation by Platform

#### Ubuntu/Debian

```bash
sudo apt-get update
sudo apt-get install -y \
    cmake \
    g++ \
    libboost-all-dev \
    libssl-dev \
    libpcre2-dev \
    libxml2-dev \
    libgtest-dev \
    libgmock-dev \
    libcurl4-openssl-dev \
    libhiredis-dev \
    redis-server \
    libmaxminddb-dev \
    yq
```

#### Alpine Linux

```bash
apk add --no-cache \
    cmake \
    g++ \
    boost-dev \
    openssl-dev \
    pcre2-dev \
    libxml2-dev \
    gtest-dev \
    gmock-dev \
    curl-dev \
    hiredis-dev \
    redis \
    libmaxminddb-dev \
    yq
```

#### CentOS/RHEL/Fedora

```bash
sudo dnf install -y \
    cmake \
    gcc-c++ \
    boost-devel \
    openssl-devel \
    pcre2-devel \
    libxml2-devel \
    gtest-devel \
    gmock-devel \
    libcurl-devel \
    hiredis-devel \
    redis \
    libmaxminddb-devel \
    yq
```

#### macOS (Homebrew)

```bash
brew install \
    cmake \
    boost \
    openssl \
    pcre2 \
    libxml2 \
    googletest \
    curl \
    hiredis \
    redis \
    maxminddb \
    yq
```

## Environment Setup

### Clone the Repository

```bash
git clone https://github.com/verifirewall/verifirewall.git
cd verifirewall
```

### Configure Git Hooks (Optional)

```bash
# Install pre-commit hooks for code formatting
pip install pre-commit
pre-commit install
```

## Building the Project

### Standard Build

```bash
# Create build directory
mkdir -p build_out
cd build_out

# Configure with CMake
cmake -DCMAKE_INSTALL_PREFIX=../build_out ..

# Build
make -j$(nproc)

# Install
make install
```

### Build Types

```bash
# Debug build (with symbols, no optimization)
cmake -DCMAKE_BUILD_TYPE=Debug -DCMAKE_INSTALL_PREFIX=../build_out ..

# Release build (optimized)
cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=../build_out ..

# RelWithDebInfo (optimized with debug symbols)
cmake -DCMAKE_BUILD_TYPE=RelWithDebInfo -DCMAKE_INSTALL_PREFIX=../build_out ..
```

### Custom Installation Prefix

```bash
cmake -DCMAKE_INSTALL_PREFIX=/opt/verifirewall ..
```

### Building Specific Targets

```bash
# Build only the HTTP transaction handler
make http_transaction_handler

# Build only the unified learning service
make unified_learning_service

# Build all nano services
make nano_services
```

## Running Tests

### Unit Tests

```bash
# Run all tests
ctest --output-on-failure

# Run specific test suite
ctest -R "http_transaction_handler" --output-on-failure

# Run with verbose output
ctest -V

# Run tests in parallel
ctest -j$(nproc) --output-on-failure
```

### Test Coverage

```bash
# Enable coverage build
cmake -DCMAKE_BUILD_TYPE=Debug -DENABLE_COVERAGE=ON ..

# Build and run tests
make -j$(nproc)
ctest --output-on-failure

# Generate coverage report
lcov --capture --directory . --output-file coverage.info
genhtml coverage.info --output-directory coverage_report
```

### Integration Tests

```bash
# Run integration tests (requires Docker)
cd test/integration
./run_integration_tests.sh
```

## Code Style

### C++ Style Guide

- **Indentation**: 4 spaces (no tabs)
- **Line Length**: Maximum 120 characters
- **Braces**: Allman style (braces on new lines)
- **Naming**:
  - Classes: `PascalCase`
  - Functions: `camelCase`
  - Variables: `snake_case`
  - Constants: `UPPER_SNAKE_CASE`
  - Namespaces: `lowercase`
- **Headers**: Include guard with `#pragma once` preferred
- **License**: Apache 2.0 header in every source file

### Example

```cpp
// Copyright (C) 2024 Check Point Software Technologies Ltd. All rights reserved.
//
// Licensed under the Apache License, Version 2.0 (the "License");
// you may not use this file except in compliance with the License.
// You may obtain a copy of the License at
//
//     http://www.apache.org/licenses/LICENSE-2.0
//
// Unless required by applicable law or agreed to in writing, software
// distributed under the License is distributed on an "AS IS" BASIS,
// WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
// See the License for the specific language governing permissions and
// limitations under the License.

#pragma once

#include <string>
#include <vector>

namespace verifirewall {

class HttpParser
{
public:
    explicit HttpParser(const std::string& config_path);
    ~HttpParser() = default;

    bool parseRequest(const std::string& raw_request);
    std::vector<std::string> getHeaders() const;

private:
    std::string config_path_;
    std::vector<std::string> headers_;
};

} // namespace verifirewall
```

### Formatting Tools

```bash
# Format with clang-format
find . -name "*.cc" -o -name "*.h" | xargs clang-format -i -style=file

# Check formatting without modifying
find . -name "*.cc" -o -name "*.h" | xargs clang-format --dry-run -Werror -style=file
```

## Static Analysis

### cppcheck

```bash
# Run cppcheck on all source files
cppcheck --enable=all \
    --std=c++17 \
    --suppress=missingIncludeSystem \
    --suppress=unusedFunction \
    --inline-suppr \
    --error-exitcode=1 \
    --quiet \
    .

# With HTML report
cppcheck --enable=all --std=c++17 --xml --xml-version=2 . 2> cppcheck_report.xml
```

### Clang Static Analyzer

```bash
# Run scan-build
scan-build --use-analyzer=/usr/bin/clang cmake ..
scan-build --use-analyzer=/usr/bin/clang make -j$(nproc)
```

### Include What You Use

```bash
# Check includes
include-what-you-use -Xiwyu --mapping_file=.iwyu.imp *.cc
```

## Debugging

### GDB

```bash
# Build with debug symbols
cmake -DCMAKE_BUILD_TYPE=Debug ..

# Run under GDB
gdb --args ./build_out/bin/http_transaction_handler --standalone

# In GDB:
# (gdb) break main.cc:42
# (gdb) run
# (gdb) bt
```

### Valgrind

```bash
# Memory leak detection
valgrind --leak-check=full --show-leak-kinds=all \
    --track-origins=yes \
    ./build_out/bin/http_transaction_handler --standalone

# Helgrind (thread errors)
valgrind --tool=helgrind ./build_out/bin/http_transaction_handler --standalone
```

### AddressSanitizer

```bash
# Build with ASan
cmake -DCMAKE_CXX_FLAGS="-fsanitize=address -fno-omit-frame-pointer" \
      -DCMAKE_BUILD_TYPE=Debug ..

# Run (will detect buffer overflows, use-after-free, etc.)
./build_out/bin/http_transaction_handler --standalone
```

## Creating Packages

### DEB Package (Debian/Ubuntu)

```bash
# Build package
make package_deb

# Output: build_out/*.deb
```

### RPM Package (RHEL/CentOS/Fedora)

```bash
# Build package
make package_rpm

# Output: build_out/*.rpm
```

### Tarball

```bash
# Create source tarball
make package_source

# Create binary tarball
make package
```

## Docker Development

### Build Development Image

```dockerfile
# Dockerfile.dev
FROM ubuntu:22.04

RUN apt-get update && apt-get install -y \
    cmake g++ libboost-all-dev libssl-dev libpcre2-dev \
    libxml2-dev libgtest-dev libgmock-dev libcurl4-openssl-dev \
    libhiredis-dev redis-server libmaxminddb-dev yq git

WORKDIR /verifirewall
```

```bash
docker build -f Dockerfile.dev -t verifirewall-dev .
docker run -it -v $(pwd):/verifirewall verifirewall-dev bash
```

### Build in Docker

```bash
# Build using the provided docker-compose
docker-compose -f deployment/nginx/docker-compose.yaml build

# Or use the make target
make docker
```

## Useful Make Targets

```bash
make help              # Show all available targets
make clean             # Clean build artifacts
make install           # Install to CMAKE_INSTALL_PREFIX
make package           # Create binary package
make package_deb       # Create DEB package
make package_rpm       # Create RPM package
make docker            # Build Docker image
make test              # Run all tests
make format            # Format code with clang-format
make lint              # Run static analysis
make docs              # Generate Doxygen documentation
```

## Troubleshooting

### Common Issues

#### CMake cannot find Boost

```bash
# Set Boost root explicitly
cmake -DBOOST_ROOT=/usr/local/boost_1_80_0 ..
```

#### Redis connection refused in tests

```bash
# Start Redis server
sudo systemctl start redis-server
# or
redis-server --daemonize yes
```

#### Missing yq

```bash
# Install yq (Go version)
wget https://github.com/mikefarah/yq/releases/latest/download/yq_linux_amd64 -O /usr/local/bin/yq
chmod +x /usr/local/bin/yq
```

### Getting Help

- Check existing [issues](https://github.com/verifirewall/verifirewall/issues)
- Read the [documentation](https://docs.verifirewall.io/)
- Contact: opensource@verifirewall.io

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for contribution guidelines.