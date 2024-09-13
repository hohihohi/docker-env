# How to use Dockerfile for development

## Description

This document show how to create python environment by Dockerfile.

## Getting Started

### Dependencies

* [Docker desktop for mac](https://www.docker.com/ja-jp/)
  * With Docker Desktop, you get the Docker Engine,   
  the Docker CLI with Compose plugin support,   
  as well as other components and tools.
* [VisualStdioCode](https://code.visualstudio.com/)
  * [Dev Containers](https://code.visualstudio.com/docs/devcontainers/containers)

### Installing

### Create local development environment

1. Pre-requirement
   - Install "Docker Desktop"
2. Copy Dockerfile on this directory
3. Build poetry container
   - `docker build --no-cache --target withpoetry -t python/withpoetry .`
4. Run poetry container and install some libraries for development
   1. `docker run -it --rm --name tmp -v "$(pwd)":/home/docker/$PROJECT_DIR_NAME python/withpoetry /bin/bash`
   2. (In container) `cd $PROJECT_DIR_NAME`
   3. (In container) `poetry init`
   4. (In container) Install some libraries for development
      - exp: `poetry add --dev flake8 black isort mypy pytest pytest-cov pytest-mock`
   5. (In container) Confirm whether  `pyproject.toml` `poetry.lock` generated or not
   6. (In container) Confirm no difference: `poetry install`
   7. (Out container) Delete unnecessary directories and files if generated
      - exp: `rm -rf .venv`
5. Build dev container
   - `docker build --no-cache --target dev -t python/dev .`
6. Run dev container
   - `docker run -it --rm --name tmp python/dev /bin/bash`

### Create local development environment as DevContainer

1. Pre-requirement
   1. Install "Docker Desktop"
   2. Install "VisualStdioCode"
   3. Install "Dev Containers" plugin into "VisualStdioCode"
2. Copy Dockerfile on this directory
3. Build poetry container
   - `docker build --no-cache --target withpoetry -t python/withpoetry .`
4. Run poetry container and install some libraries for development
   1. `docker run -it --rm --name tmp -v "$(pwd)":/home/docker/$PROJECT_DIR_NAME python/withpoetry /bin/bash`
   2. (In container) `cd $PROJECT_DIR_NAME`
   3. (In container) `poetry init`
   4. (In container) Install some libraries for development
      - exp: `poetry add --dev flake8 black isort mypy pytest pytest-cov pytest-mock`
   5. (In container) Confirm whether  `pyproject.toml` `poetry.lock` generated or not
   6. (In container) Confirm no difference: `poetry install`
   7. (Out container) Delete unnecessary directories and files if generated
      - exp: `rm -rf .venv`
5. [Open an existing folder in a container](https://code.visualstudio.com/docs/devcontainers/containers#_quick-start-open-an-existing-folder-in-a-container)
   
### Set pre-commit

- Pre-requirement
  - git

1. Move to devcontainer
2. Update pre-commit
  - `poetry run pre-commit sample-config > .pre-commit-config.yaml`
3. Install hook
  - `poetry run pre-commit install`


## Help

### How to execute code

- `poetry run python $PYTHON_FILE`
- `poetry run pytest`

### How to update libraries

Run poetry container and install some libraries for development
1. `docker run -it --rm --name tmp -v "$(pwd)":/home/docker/$PROJECT_DIR_NAME python/dev /bin/bash`
2. (In container) `cd $PROJECT_DIR_NAME`
3. (In container) Confirm package update
   - `poetry update --dry-run`
4. (In container) Execute packages
   - `poetry update`
5. Rebuild poetry container
   - `docker build --no-cache --target dev -t python/dev .`

## Authors

Contributors names and contact info

ex. Dominique Pizzie  
ex. [@DomPizzie](https://twitter.com/dompizzie)

## Version History

* 0.2
    * Various bug fixes and optimizations
    * See [commit change]() or See [release history]()
* 0.1
    * Initial Release

## License

This project is licensed under the [NAME HERE] License - see the LICENSE.md file for details

## Acknowledgments

Inspiration, code snippets, etc.
* [awesome-readme](https://github.com/matiassingers/awesome-readme)
* [PurpleBooth](https://gist.github.com/PurpleBooth/109311bb0361f32d87a2)
* [dbader](https://github.com/dbader/readme-template)
* [zenorocha](https://gist.github.com/zenorocha/4526327)
* [fvcproductions](https://gist.github.com/fvcproductions/1bfc2d4aecb01a834b46)