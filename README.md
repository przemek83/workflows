# Table of contents
- [Overview](#overview)
- [Workflows](#workflows)
   * [Build & test](#build-test)
   * [Analysis and coverage](#analysis-and-coverage)
   * [Pylint](#pylint)
   * [CodeQL](#codeql)
   * [Qt](#qt)
- [Licensing](#licensing)

# Overview
This repository contains reusable GitHub Actions workflows that can be called from any repository.

# Workflows
Repository contains multiple reusable workflows. There are workflows for building and testing, analysis and coverage reporting using SonarCloud and Codecov, custom CodeQL analysis, and Pylint analysis. For some workflows there are seperate files dedicated each supported programming language. As of Nov 2024 languages are: C++, C++ with Qt, Go, Python.

> **_NOTE:_**  for some workflows there are multiple versions dedicated different programming language. Examples are for C++.

## Build & test
To use building and testing reusable workflow, place following file in your repo:
```yaml
name: Build & test

on: [push, pull_request]

jobs:
  linux:
    uses: przemek83/workflows/.github/workflows/build-and-test-cpp.yml@main
    with:
      platform: ubuntu-latest

  windows:
    uses: przemek83/workflows/.github/workflows/build-and-test-cpp.yml@main
    with:
      platform: windows-latest
```
or more universal:
```yaml
name: Build & test

on: [push, pull_request]

jobs:
  build-and-test:
    strategy:
      matrix:
        platform: [ubuntu-latest, windows-latest]
    uses: przemek83/workflows/.github/workflows/build-and-test-cpp.yml@main
    with:
      platform: ${{ matrix.platform }}
```

Workflow will launch building and testing of project using CMake on Linux and Windows platforms. Check my other projects like `data-explorer` or `penna-model` for usage examples.

## Analysis and coverage
To use SonarCloud static analysis and coverage on Codecov and SonarCloud, place following file in your repo:
```yaml
name: Static analysis and coverage

on: [push, pull_request]

jobs:
  analyze:
    uses: przemek83/workflows/.github/workflows/static-analysis-and-coverage-cpp.yml@main
    secrets: inherit
```
Make sure that appropriate secrets:
- CODECOV_TOKEN - token for Codecov coverage reporting,
- SONAR_TOKEN - token for SonarCloud
 analysis and coverage reporting.

Are present in secrets section on GitHub. Also copy `sonar-project.properties` to your project and fill project key and project name. Check my other project like `data-explorer` or `penna-model` for usage examples.

## Pylint
Static analysis reusable workflow for Python projects using Pylint. To call reusable workflow place following content in your workflow file:
```yaml
name: Pylint

on: [push, pull_request]

jobs:
  analyze:
    uses: przemek83/workflows/.github/workflows/pylint.yml@main
```

Check `penna-model` project for example use.

## CodeQL
Workflow for CodeQL analysis in case default version does not work correctly. Dedicated for C++ projects using Qt framework.

```yaml
name: "CodeQL analysis"

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]
  schedule:
    - cron: '29 2 * * 0'

jobs:
  analyze:
    uses: przemek83/workflows/.github/workflows/codeql-cpp.yml@main
    with:
      use-qt: true
      qt-version: 6.5.*
```
Check `sqlite-browser` project for example use.

# Licensing
Software is released under the MIT license.
