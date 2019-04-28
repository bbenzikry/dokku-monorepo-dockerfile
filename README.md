# Dokku Monorepo+Docker plugin

## Installation

```sh
dokku plugin:install https://github.com/bbenzikry/dokku-monorepo-dockerfile
```

## Usage

- Create a `.monorepo.dokku` file in your root folder

```sh
# .monorepo.dokku
DOKKU_APP_NAME=DOCKER_NAME
```

- The plugin will rename a file called `DOCKER_NAME.Dockerfile` in the root directory so dokku picks it up

```sh
$ git push -f REMOTE_NAME HEAD:master
=====> Testing docker monorepo presence...
=====> Docker monorepo detected
=====> Building Dockerfile from DOCKER_NAME.Dockerfile
-----> Cleaning up...
-----> Building APP from dockerfile...
remote: build context to Docker daemon
...
```

- In case there's no definition present for the application, Dokku will fallback to default configuration

```sh
=====> Docker monorepo detected
=====> The application APP is not defined in .monorepo.dokku
-----> Cleaning up...
-----> Building APP from herokuish...
```

### Managing different `.dockerignore` files

Add a `DOCKER_NAME.dockerignore` file in your root

## Mix and match with buildpacks

See the [dokku-monorepo](https://github.com/notpushkin/dokku-monorepo) plugin

## FAQ

> Dokku is using heroku buildpack detection for the app

From the [Dokku documentation](https://github.com/dokku/dokku/blob/master/docs/deployment/methods/dockerfiles.md)

- In case your app was previously deployed using buildpacks, make sure to run the following:

```sh
  dokku config:unset --no-restart APP_NAME DOKKU_PROXY_PORT_MAP
```

- Make sure the application does not have a `BUILDPACK_URL` environment variable set via the `dokku config:set` command or in a committed `.env` file.
- Make sure the application does not have a `.buildpacks` file in the root of the repository.

> TCP workloads

```sh

# disable nginx proxying
proxy:disable APP

# Add host/container mapping
dokku docker-options:add APP deploy "-p PORT:PORT"
```

> `app.json` tasks

Simply make sure `app.json`/`.scripts.dokku` is added to the docker `WORKDIR`

## Issues

<img src="https://i.imgflip.com/2zjvnu.jpg" width="40%" height="40%" title="made at imgflip.com"/></a>

> Multi-stage caching doesn't work/sporadic errors on push

```sh
! [remote rejected] HASH -> master (pre-receive hook declined)
error: failed to push some refs to 'REMOTE:APP'
Exited with code 1
```

See:

- https://github.com/dokku/dokku/issues/3116
- https://github.com/dokku/dokku/issues/3474
