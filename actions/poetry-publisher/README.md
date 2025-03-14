# 🚀 Poetry Publisher GitHub Action

**Category:** Deployment & Package Management

This **Poetry Publisher** GitHub Action automates the process of building, testing, and publishing Python packages using [Poetry](https://python-poetry.org/).

## 📌 Features

- Installs Poetry and required dependencies.
- Validates the PyPI authentication token.
- Automatically bumps package version (`patch`, `minor`, etc.).
- Builds and verifies the package before publishing.
- Runs tests using `pytest` (if enabled).
- Publishes the package to Test PyPI.

## Inputs

| Name                   | Description                                       | Required | Default      |
| ---------------------- | ------------------------------------------------- | -------- | ------------ |
| `package_version`      | Specific version to set for the package           | No       | `''` (empty) |
| `poetry_version_bump`  | Version increment option (`patch`, `minor`, etc.) | No       | `patch`      |
| `poetry_build_options` | Additional build parameters for Poetry            | No       | `''` (empty) |
| `run_pytest`           | Run tests with `pytest` before publishing         | No       | `false`      |
| `pytest_options`       | Additional parameters for `pytest`                | No       | `''` (empty) |

## Environment Variables

| Name         | Description                      | Required |
| ------------ | -------------------------------- | -------- |
| `PYPI_TOKEN` | API token for publishing to PyPI | Yes      |

## Usage Example

Here’s an example of how to use this action in a GitHub workflow:

```yaml
name: Publish Python Package

on:
  push:
    branches:
      - main
  workflow_dispatch:
    inputs:
      package_version:
        description: "Specific version for the package"
        required: false
        default: ""
      poetry_version_bump:
        description: "Version increment option"
        required: false
        default: "patch"
      poetry_build_options:
        description: "Additional build parameters"
        required: false
        default: ""
      run_pytest:
        description: "Run tests with pytest"
        required: false
        default: "false"
      pytest_options:
        description: "Parameters for pytest"
        required: false
        default: "--maxfail=3 -v"

jobs:
  publish:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: "3.11"
          cache: pip

      - name: Run Poetry Publisher Action
        uses: Netcracker/qubership-workflow-hub/actions/poetry-publisher
        with:
          package_version: ${{ github.event.inputs.package_version }}
          poetry_version_bump: ${{ github.event.inputs.poetry_version_bump }}
          poetry_build_options: ${{ github.event.inputs.poetry_build_options }}
          run_pytest: ${{ github.event.inputs.run_pytest }}
          pytest_options: ${{ github.event.inputs.pytest_options }}
        env:
          PYPI_API_TOKEN: ${{ secrets.PYPI_API_TOKEN }}
```
