# fips-vld
Fipsified VLD (Visual Leak Detector)

https://github.com/KindDragon/vld

**Work In Progress**

### How to use:

Only works on Windows (the import will be ignored when compiling for
other platforms).

#### Add fips-vld as import to your fips.yml file:

```yaml
imports:
    fips-vld:
        git: https://github.com/floooh/fips-vld.git
```

#### Run 'fips fetch' to fetch fips-vld from github:

```bash
> fips fetch
```

#### Compile with one of \*-vld build configs:

```bash
# 32-bit Windows:
> fips set config win32-vstdio-vld
> fips build
# 64-bit Windows:
> fips set config win64-vstudio-vld
```

You also need to integrate VLD into your project's source code
(see VLD docs: https://vld.codeplex.com/documentation).

