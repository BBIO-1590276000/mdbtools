# Mestrelab's MDB Tools Modifications

This is a fork containing Mestrelab-specific modifications to the [MDB Tools](https://github.com/mdbtools/mdbtools) project. The main purpose of these modifications is to enable building the **libmdb** library on Windows using the Visual Studio compiler and to modernize the build system by using CMake instead of autoconf.

## Purpose

The modifications in this repository serve two main objectives:

1. **Windows Support with Visual Studio**: Enable compilation of libmdb on Windows using Microsoft Visual Studio compilers (VS2019 or VS2022 using the VS2019 compiler tools).

2. **CMake Build System**: Replace the traditional autoconf/automake build system with CMake, providing a modern, cross-platform build solution that works seamlessly on Windows, Linux, and macOS.

## Build Instructions

The project uses CMake workflow presets to simplify the build process across different platforms. Below are the instructions for each supported platform.

### Prerequisites

- **CMake** 3.25 or later
- **Ninja** build system (recommended for Linux and macOS)
- Platform-specific compiler:
  - **Windows**: Visual Studio 2019 or Visual Studio 2022
  - **Linux**: GCC
  - **macOS**: Xcode Command Line Tools (Clang)

### Building on Windows

For Windows, several workflow presets are available depending on your Visual Studio version and target architecture:

#### Using Visual Studio 2019:

**For x64 (64-bit) builds:**
```bash
cmake --workflow --preset msvc2019-x64
```

**For x86 (32-bit) builds:**
```bash
cmake --workflow --preset msvc2019-x86
```

#### Using Visual Studio 2022 with VS2019 toolset:

**For x64 (64-bit) builds:**
```bash
cmake --workflow --preset msvc2022_vs2019-x64
```

**For x86 (32-bit) builds:**
```bash
cmake --workflow --preset msvc2022_vs2019-x86
```

These workflows will configure the project, then build both Debug and RelWithDebInfo configurations, and install the binaries to the `install` folder.

### Building on Linux

**For 64-bit Linux builds:**
```bash
cmake --workflow --preset linux
```

This workflow will configure the project using Ninja Multi-Config generator, build both Debug and Release configurations, and install the binaries to the `install` folder.

### Building on macOS

**For Universal Binary (x86_64 + ARM64):**
```bash
cmake --workflow --preset macos-universal
```

This workflow creates a universal binary supporting both Intel (x86_64) and Apple Silicon (ARM64) architectures, with a minimum deployment target of macOS 11.0.

## Build Output

After a successful build, you will find:

- **Build files**: Located in `build/<preset-name>/` directory
- **Install files**: Located in the `install/` directory at the project root

The installed files will include:
- Libraries (static or shared, depending on configuration)
- Header files

## Additional Files

This folder also contains:

- `libmdb.rc.in`: Windows resource file template for version information
- `resource.h`: Resource definitions for Windows builds
- `version_linux.c.in`: Version information template for Linux builds
- `ya_getopt/`: Portable implementation of getopt for Windows compatibility
- `mdbtools.code-workspace`: VS Code workspace configuration

## Using libmdb with CMake's find_package

After installing libmdb, other CMake projects can easily integrate it using the `find_package` mechanism.

### Basic Usage

In your CMakeLists.txt:

```cmake
find_package(mdb REQUIRED)

add_executable(my_app main.c)
target_link_libraries(my_app mdb::mdb)
```

### Advanced Usage

If you installed libmdb to a custom prefix, specify the path:

```cmake
find_package(mdb REQUIRED PATHS /custom/install/path/lib/cmake/mdb)
```

### Accessing Library Information

You can also access the library information variables directly:

```cmake
find_package(mdb)

if(mdb_FOUND)
    message(STATUS "libmdb found!")
    message(STATUS "Include directories: ${mdb_INCLUDE_DIRS}")
    message(STATUS "Libraries: ${mdb_LIBRARIES}")

    target_include_directories(my_target PRIVATE ${mdb_INCLUDE_DIRS})
    target_link_libraries(my_target PRIVATE ${mdb_LIBRARIES})
endif()
```

### Version Checking

You can specify a minimum version requirement:

```cmake
find_package(mdb 1.0.1 REQUIRED)
```

## Note

These modifications maintain compatibility with the original MDB Tools functionality while enabling modern build workflows across all major platforms. The CMake presets provide a consistent and simplified build experience regardless of the target platform.
