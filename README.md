# poetry-publish

[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://GitHub.com/JRubics/poetry-publish/graphs/commit-activity)
[![Latest release](https://badgen.net/github/release/JRubics/poetry-publish)](https://github.com/JRubics/poetry-publish/releases)

An action to build and publish python package to [PyPI](https://pypi.org/), [TestPyPI](https://test.pypi.org/) or a private wheels repo using [poetry](https://github.com/python-poetry/poetry).

## Inputs

### `python_version`

The version of python to install (default: latest). Use default for a shorter build time.

### `poetry_version`

The version of poetry to install (default: latest).

### `pypi_token`

**Required** API token to authenticate when uploading package to PyPI (You can find your token [here](https://pypi.org/manage/account/)).

### `repository_name`

The name of a repository where the package will be uploaded. Necessary if you'd like to upload to test PyPi or a private wheels repo. Uploads to official PyPi if not informed.

### `repository_url`

The URL where the package will be uploaded. Necessary if you'd like to upload to test PyPi or a private wheels repo. Uploads to official PyPi if not informed.

### `repository_username`

The Username to log in into a repository where the package will be uploaded if using http-basic authentification instead of api token.

### `repository_password`

The Password to log in into a repository where the package will be uploaded if using http-basic authentification instead of api token.

### `build_format`

By default, poetry's build command outputs two formats: **wheel** and **sdist**. If you intend to use
only one of them, you may specify that with the `build_format` parameter.

### `ignore_dev_requirements`

This will instruct poetry **not** to install any developer requirements. this may lead to an overall quicker experience.

### `allow_poetry_pre_release`

Allow poetry pre-release versions to be installed.

## Example usage

The following will build and publish the python package to the PyPI using the last version of python and poetry. Specify the python package version and dependencies in `pyproject.toml` in the root directory of your project.

```yaml
- name: Build and publish to pypi
  uses: JRubics/poetry-publish@v1.9
  with:
    pypi_token: ${{ secrets.PYPI_TOKEN }}
```

Python and poetry versions can be specified in inputs as well as the build_format, allow_poetry_pre_release and ignore_dev_requirements.

```yaml
- name: Build and publish to pypi
  uses: JRubics/poetry-publish@v1.9
  with:
    python_version: "3.7.1"
    poetry_version: "==1.0.5" # (PIP version specifier syntax)
    pypi_token: ${{ secrets.PYPI_TOKEN }}
    build_format: "sdist"
    allow_poetry_pre_release: "yes"
    ignore_dev_requirements: "yes"
```

Repository can be changed to TestPyPI or a private wheels repo by specifying repository_name and repository_url.

```yaml
- name: Build and publish to pypi
  uses: JRubics/poetry-publish@v1.9
  with:
    pypi_token: ${{ secrets.PYPI_TOKEN }}
    repository_name: "testpypi"
    repository_url: "https://test.pypi.org/legacy/"
```

Repository authentication can be cahnged to http-basic authentification by specifying repository_username and repository_password instead of pypi_token.

```yaml
- name: Build and publish to private Python package repository
  uses: JRubics/poetry-publish@v1.9
  with:
    repository_name: "foo"
    repository_url: "https://foo.bar/simple/"
    repository_username: "username"
    repository_password: "password"
```
## Example workflow

The following will build and publish the python package when project is tagged in the `v*.*.*` form.

```yaml
name: Python package
on:
  push:
    tags:
      - "v*.*.*"
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Build and publish to pypi
        uses: JRubics/poetry-publish@v1.9
        with:
          pypi_token: ${{ secrets.PYPI_TOKEN }}
```
