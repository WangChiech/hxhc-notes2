---
{"dg-publish":true,"permalink":"/2-areas/programming/cpp/clang-tidy-config-file/"}
---


In VSCode:
1. Install `clangd`
2. press F1 and type `clangd: open user configuration file`

```yaml
Diagnostics:
  ClangTidy:
    Add:
      [
        boost-*,
        bugprone-*,
        clang-analyzer-*,
        cppcoreguidelines-*,
        modernize-*,
        openmp-*,
        performance-*,
        readability-*,
      ]
    Remove:
      [
        readability-braces-around-statements,
        modernize-use-trailing-return-type,
        cppcoreguidelines-special-member-functions,
        cppcoreguidelines-avoid-magic-numbers,
        readability-magic-numbers,
        readability-identifier-length,
        readability-make-member-function-const,
        bugprone-easily-swappable-parameters,
        modernize-use-equals-default,
      ]
Index:
  Background: Build
```