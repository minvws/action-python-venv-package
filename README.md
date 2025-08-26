# Python - venv package GitHub Action

This repository provides a reusable GitHub Action for creating a venv package for the specified Python version with the installed requirements.
When [Poetry](https://python-poetry.org/) is used, the requirements will be exported and the requirements will be installed in the venv.

## Usage

To use the action, add it to a workflow in your repository:

```yml
name: Build Python project

on:
  push:
    tags:
      - v*

jobs:
  venv-package:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Build venv package
        uses: minvws/action-python-venv-package@v1
        with:
          python_version: <python_version>
          package_file_name: <package_file_name>
          checkout_repository: "false"
```

Replace `<python_version>` with the Python version your project needs and replace `<package_file_name>` with the desired name for the package. See [Configuration](#configuration) for details.

In this basic example, the workflow is executed automatically on push of tags.

### Configuration

The action has inputs. The inputs are:

- `python_version`: Which version of Python version to use. For example `"3.11"` or `">=3.9 <3.14"`.
- `package_file_name`: File name for the venv package. For example `nl-example-package`.
- `checkout_repository`: Boolean value inside string to enable or disable checkout repository
  in the action. For example `"true"` or `"false"`. Default `"true"`.
- `working_directory`: Directory containing the Python project. The directory should contain
  a `requirements.txt` file or a `pyproject.toml` file. Default `"."`.

### Result

This action will create a `.tar.gz` file containing the `.venv` directory. The file will be available as an artifact.

The name of the artifact will be `<package_file_name>_venv_<tag_version>_python<python_version>.tar.gz`. For example `nl-example-package_venv_v0.0.1_python3.9.tar.gz`.

The uploaded artifact will have a limited lifetime depending on what is currently configured.

## Contributing

If you want to contribute a new pipeline, please check the reusable workflow guidelines in the
[GitHub documentation](https://docs.github.com/en/actions/using-workflows/reusing-workflows#creating-a-reusable-workflow).

## License

This repository is released under the EUPL 1.2 license. See [LICENSE.txt](./LICENSE.txt) for details.

## Part of iCore

This package is part of the iCore project.
