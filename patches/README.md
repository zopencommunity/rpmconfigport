# Patches for rpmconfig

This directory contains patches applied to the RPM source code to generate only configuration files for z/OS.

## Patch Descriptions

### CMakeLists.txt.patch
**Purpose**: Skip building RPM libraries and executables - only generate configuration files.

This patch comments out the following subdirectories from the build:
- `docs` - Documentation generation
- `include/rpm` - Header installation
- `misc`, `rpmio`, `lib`, `build`, `sign` - Library compilation
- `tools` - Executable compilation

It also comments out the library export statements that would fail without the libraries.

**What still gets built:**
- Configuration file generation (macros, rpmrc, platform, rpmpopt)
- File attributes installation (fileattrs/)
- Helper scripts installation (scripts/)

### macros.in.patch
**Purpose**: Apply z/OS-specific customizations to RPM macros.

Changes:
- Exclude common interpreter paths from auto-generated Requires
- Use lower compression level (w3.zstdio) for faster builds
- Use target platform instead of ARCH for package naming

### rpmrc.in.patch
**Purpose**: Apply z/OS-specific customizations to RPM runtime configuration.

## Testing

After applying these patches, the build should:
1. Configure successfully with CMake
2. Skip all library/executable compilation
3. Generate config files in the build directory
4. Install only the config files to the target directory
