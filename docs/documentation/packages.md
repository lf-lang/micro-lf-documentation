# Packages

Reusable reactors can be shared as **packages**: plain directories, typically git
clones of a library repository, that the micro-LF compiler finds by name. Import from a package
with angle brackets instead of a relative path:

```lf
import Count from <my-package/Count.ulf>   // explicit library file
import Count from <my-package>             // file name defaults to Count.ulf
```

## Package Layout

A package keeps its reactors under `src/lib`:

```text
my-package/
└── src/
    └── lib/
        ├── Count.ulf
        └── sensors/
            └── Imu.ulf
```

The path after the package name is resolved inside `src/lib`:

| Import | File |
|--------|------|
| `import Count from <my-package/Count.ulf>` | `my-package/src/lib/Count.ulf` |
| `import Count from <my-package>` | `my-package/src/lib/Count.ulf` |
| `import Imu from <my-package/sensors>` | `my-package/src/lib/sensors/Imu.ulf` |
| `import Imu from <my-package/sensors/Imu.ulf>` | `my-package/src/lib/sensors/Imu.ulf` |

When the path does not end in a `.ulf` file, the file is named after the
imported reactor. Imports cannot reach outside the package's `src/lib`
directory.

## Where Packages Are Found

The compiler determines the project root as the parent of the `src` directory that
contains the importing file, and looks for the package directory in this order:

1. `<project root>/build/lfc_include/<package>`
2. `<project root>/lf-packages/<package>`
3. `$LF_PACKAGES/<package>`, if the `LF_PACKAGES` environment variable is set

For example, to use a library in a project:

```bash
git clone https://github.com/<owner>/my-package.git lf-packages/my-package
```
