 ![openEMS](https://raw.github.com/thliebig/openEMS-Project/master/other/openEMS.png "openEMS")<br />

# openEMS (alpLab fork)

This is the [alpLab](https://github.com/alplabai) fork of [openEMS](https://github.com/thliebig/openEMS-Project), maintained for integration into [Signex](https://github.com/alplabai/alp-eda) as the primary PCB electromagnetic field solver.

**Upstream**: [https://github.com/thliebig/openEMS-Project](https://github.com/thliebig/openEMS-Project)<br />
**Upstream Website**: [https://openEMS.de](https://openEMS.de)<br />
**Upstream Docs**: [https://docs.openEMS.de](https://docs.openEMS.de)<br />

## Branch: `signex`

The `signex` branch (on openEMS and CSXCAD submodules) contains all fixes required for safe shared-library embedding. Changes are kept minimal and upstreamable.

### Fixes applied

**openEMS (`signex` branch):**
- Replace all `exit()` calls with exception hierarchy (`openEMS_SetupError`, `openEMS_AllocationError`, `openEMS_InternalError`) for safe shared-library use
- Replace `volatile` with `std::atomic` for proper thread safety in multi-threaded engine
- Fix `setlocale` race conditions with RAII `ScopedNumericLocale` guard
- `SnapToMeshLine` O(N) linear search replaced with O(log N) binary search (~14x speedup)
- Fix `diff_pow` and `m_LM_pos[n]` memory leaks, `size_t` overflow, C++14 compliance, `-Wall -Wextra` clean
- Merged upstream WIP branch: multi-threaded SAR (IEEE/IEC compliant), HDF5 improvements, arraylib

**CSXCAD (`signex` branch):**
- Fix iterator UB after erase
- Fix `UpdateIDs` logic bug

### Building (MSYS2 MinGW on Windows)

```bash
# Install dependencies
pacman -S mingw-w64-x86_64-{gcc,cmake,boost,hdf5,vtk,tinyxml,cgal,nlohmann-json}

# Build order: fparser -> CSXCAD -> openEMS
cd fparser && mkdir -p build && cd build
cmake .. -G "MinGW Makefiles" -DCMAKE_INSTALL_PREFIX=../../install && mingw32-make -j$(nproc) install && cd ../..

cd CSXCAD && mkdir -p build && cd build
cmake .. -G "MinGW Makefiles" -DCMAKE_INSTALL_PREFIX=../../install && mingw32-make -j$(nproc) install && cd ../..

cd openEMS && mkdir -p build && cd build
cmake .. -G "MinGW Makefiles" -DCMAKE_INSTALL_PREFIX=../../install && mingw32-make -j$(nproc) install && cd ../..
```

## openEMS Features

+ Fully 3D Cartesian and cylindrical coordinates graded mesh
+ Multi-threading, SIMD (SSE) and MPI support for high speed FDTD
+ Octave/Matlab and Python interface
+ Dispersive material (Drude/Lorentz/Debye type)
+ Field dumps in time and frequency domain as VTK or HDF5 file format
+ Multi-threaded SAR calculation with IEEE/IEC compliance
+ Flexible post-processing routines in Octave/Matlab and Python

## License

openEMS is licensed under GPL-3.0. CSXCAD is licensed under LGPL-3.0. See individual submodule LICENSE files.
