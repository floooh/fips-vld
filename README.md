# fips-vld
Fipsified VLD (Visual Leak Detector)

https://github.com/KindDragon/vld

**This is work in progress!**

### How to use:

Only works on Windows (the import will be ignored when compiling for
other platforms).

First make sure to update fips itself (the feature to detect 
custom build configs in imported project is new)!

Add fips-vld as import to your project's fips.yml file:

```yaml
imports:
    fips-vld:
        git: https://github.com/floooh/fips-vld.git
```

Run 'fips fetch' to fetch fips-vld from github:

```bash
> fips fetch
```

Compile with one of the provided \*-vld build configs:

```bash
# 32-bit Windows:
> fips set config win32-vstdio-vld
> fips build
# 64-bit Windows:
> fips set config win64-vstudio-vld
```

You also need to integrate VLD into your project's source code
(see VLD docs: https://vld.codeplex.com/documentation).

