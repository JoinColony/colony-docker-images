# colony-docker-images

Colony docker images for hub auto-builds

## Images

### circleci-cypress
An image based on circleci/node which also contains the cypress testing dependencies

### app

The App production docker image builds the production version of the bundle, moves it to separate folder, and serves it via a simple `nginx` server on port `80`

The `Dockerfile` just needs to be built, and run. The `nginx` service `CMD` will keep the container alive.

While the app itself uses ENV variables, they are expected to be set in the envrinment that this image will run in. For the required ones, check the [app repository](https://github.com/JoinColony/colonyDapp).

Usage example:
```bash
docker build --no-cache .
```

#### Building from a different branch / commit hash

If you want to build the docker image from a different part of the repo, you can specify either a branch or a commit hash via the `COMMMIT_HASH` build argument which will checkout the repo at that point in time.

Usage example:
```bash
docker build --build-arg COMMMIT_HASH='feature/cool-new-feature' --no-cache .
```

### server

The server production docker image builds the production version of the bundle, starts the express server and serves it, by default on port `3000`.

#### Build Args

This docker file allows you to pass in build time arguments to change the environment the server bundle is being built with. There is only one build arguments that is **required**: your github token passed into `GH_PAT`

Build args:
- `GH_PAT`: the GitHub Personal Access Token **required** to authenticate and pull information from our private repo _(Note: you **have to** supply this yourself, otherwise it won't work)_.

While there are _other_ ENV variables that are needed for the server itself to function properly, they are expected to be set in the envrinment that this image will run in. For the required ones, check the [server repository](https://github.com/JoinColony/colonyServer).

Usage example:
```bash
docker build --build-arg GH_PAT='XXX' --no-cache .
```

#### Building from a different branch / commit hash

If you want to build the docker image from a different part of the repo, you can specify either a branch or a commit hash via the `COMMMIT_HASH` build argument which will checkout the repo at that point in time.

Usage example:
```bash
docker build --build-arg COMMMIT_HASH='feature/cool-new-feature' --no-cache .

## License

MIT
