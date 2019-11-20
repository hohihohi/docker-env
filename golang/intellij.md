## Pre-Requirement

* Add docker and docker-compose on local environment
    - docker-for-windows
    - docker-for-mac
* Docker plugin
* Go plugin
* Version > 2019.2

## How to set up development environment

1. [Docker Setting]()
2. [Docker Tools Setting]()
    - Preference -> Build,Execution,Deployment -> Docker -> Tools
3. Open docker tab
    - View -> Tool Windows -> Docker
4. Create environment file

```shell
cat << EOF >> .env
UID=$(id -u)
GID=$(id -g)
EOF
```

5. Create netrc file

```shell
cat << EOF >> .netrc
machine github.com
login username
password secret
EOF
```

6. Build docker image for development
    - `docker-compose build --no-cache`
7. Create docker container for development
    - `docker-compose up -d`
8. Set up FileWatcher to apply lint
    - Configure [FileWatcher](https://www.jetbrains.com/help/go/settings-tools-file-watchers.html) with arguments run --print-issued-lines=false $FileDir$. 

## Usage

Please read Makefile

* `docker-compose run --rm godev $(MAKE_COMMAND)`
* [How to use delve](https://github.com/go-delve/delve/tree/master/Documentation/cli)
