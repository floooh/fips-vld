# fips-vld
Fipsified VLD (Visual Leak Detector)

VLD: https://github.com/KindDragon/vld

fips: https://github.com/floooh/fips

Only works on Windows (the import will be ignored when compiling for
other platforms).

### How to use:

First make sure to update fips itself (the feature to detect 
custom build configs in imported project is new)!

#### Importing into your fips project:

Add fips-vld as import to your project's fips.yml file:

```yaml
imports:
    fips-vld:
        git: https://github.com/floooh/fips-vld.git
```

Run 'fips fetch' to fetch fips-vld from github:

#### Integrating VLD into your project:

In your toplevel CMakeLists.txt or fips-include.cmake file, check for the 
FIPS\_USE\_VLD, and if this is set, setup a C preprocessor define of your
choice (all following examples are taken from the Oryol 3D engine):

```cmake
# use Visual Leak Detector?
# see https://github.com/floooh/fips-vld
if (FIPS_USE_VLD)
    add_definitions(-DORYOL_USE_VLD=1)
endif()
```

Now in one of your source files, include the **vld.h** header if the define
is set:

```cpp
#if ORYOL_USE_VLD
#include "vld.h"
#endif
```

In the CMakeLists.txt file of the source file's module (in Oryol's case: the 
Core module) define a library dependency to **vld**. Every app which uses the
Core module will now also link automatically against VLD. Also, an extra target dependency
must be defined to trigger fips-vld's special target **vld\_copy\_dlls** (this is explained
further down):

```cmake
# example from Oryol's Core module CMakeLists.txt
fips_begin_module(Core)
    ...
    if (FIPS_USE_VLD)
        fips_libs(vld)
    endif()
fips_end_module()
if (FIPS_USE_VLD)
    add_dependencies(Core vld_copy_dlls)
endif()
```

That's all for preparations, now build with one of the the 
fips-vld custom-configs **win32-vstudio-vld** or **win64-vstudio-vld** 
inside Visual Studio:

```bash
# 32-bit Windows: set win32 default config and open in Visual Studio:
> fips set config win32-vstdio-vld
> fips open
# 64-bit Windows: likewise
> fips set config win64-vstudio-vld
> fips open
```

Build and run the application (in debug mode). Now, on application shutdown, 
VLD should list the memory leaks it found, or if you're lucky you'll see a:

```
No memory leaks detected.
Visual Leak Detector is now exiting.
```

### How it works:

The VLD fipsification is a bit unusual:

- the VLD DLLs are precompiled and are located under fips-vld/libs
- the CMakeLists.txt file only defines a custom target called **vld\_copy\_dlls** 
which copies the precompiled DLLs to the fips-deploy directory where the
executables are built to
- in order to trigger the copying, a target dependencies must be defined
in a CMakeLists.txt file of the application being tested (in Oryol, the
Core module sets up this dependency since it is linked into every Oryol
application)

The fips custom configs under **fips-vld/fips-configs** simply set the
cmake option FIPS\_USE\_VLD to ON, the same can be achieved by using
one of the default debug configs (e.g. win64-vstudio-debug) and 
set the FIPS\_USE\_VLD option to ON in ccmake / cmake-gui (via 'fips config').


