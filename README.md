# Python - venv package

- This pipeline is designed to export a venv package for the specified Python version with the installed requirements.
When [Poetry](https://python-poetry.org/) is used, the requirements will be exported and the requirements will be installed in the venv.
- The pipeline is designed to be as generic as possible, so they can be easily reused in any project.
- This repository is a part of the generic GitHub Actions pipeline collection that can be used in any project.

## Usage

Here are basic examples of how you can integrate it in your project.

<details>
  <summary>Example workflow</summary>

This workflow is executed automatically on push of tags.

In the code below you need to replace the `<python_version>` and `<package_file_name>`. See the [configuration section](#configuration).

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
      # Using the action
      - name: Build venv package
        uses: minvws/action-python-venv-package/.github/actions/python-venv-package@main
        with:
          python_version: <python_version>
          package_file_name: <package_file_name>

```

</details>

<details>
  <summary>Example workflow self checkout</summary>

This workflow is executed automatically on the push of tags. The workflow will check out the repo and the action won't. Now it is possible to run additional actions before using the venv package action.

In the code below you need to replace the `<python_version>` and `<package_file_name>`. See the [configuration section](#configuration).

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

      # Using the action
      - name: Build venv package
        uses: minvws/action-python-venv-package/.github/actions/python-venv-package@main
        with:
          python_version: <python_version>
          package_file_name: <package_file_name>
          checkout_repository: 'false'
```

</details>

### Configuration

The action has inputs. The inputs are:

- python_version: Semver version of the Python version you want to use. For example `3.11` or `3.9`.
- package_file_name: File name for the venv package. For example `nl-example-package`.
- checkout_repository: Boolean value inside string to enable or disable checkout repository
  in the action. For example `'true'` or `'false'`. Default `'true'`.
- working_directory: Directory containing the Python project. The directory should contain
  a `requirements.txt` file or a `pyproject.toml` file. Default `'.'`.

### Result

This action will create a `.tar.gz` file containing the `.venv` directory. The file will be available as an artifact.

The name of the artifact will be `<package_file_name>_venv_<tag_version>_python<python_version>.tar.gz`. For example `nl-example-package_venv_v0.0.1_python3.9.tar.gz`.

The uploaded artifact will have a limited lifetime depending on what is currently configured.

## Contributing

If you want to contribute a new pipeline, please check the reusable workflow guidelines in the
[GitHub documentation](https://docs.github.com/en/actions/using-workflows/reusing-workflows#creating-a-reusable-workflow).
