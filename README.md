# colony-docker-images

Colony docker images for hub auto-builds

## Images

### circleci-cypress
An image based on circleci/node which also contains the cypress testing dependencies

### dapp

The [dApp production] docker image builds the production version of the bundle, moves it to separate folder, and serves it via a simple `nginx` server on port `80`

The `Dockerfile` just needs to be built, and run. The `nginx` service `CMD` will keep the container alive.

#### Build Args

This docker file allows you to pass in build time arguments to change the environment the dApp's bundle is being built with. The only one of these that is **required** is the `INFURA_ID` one, without which, you won't be able to use the provider.

Build args:
- `INFURA_ID`: The infura project id we are using in the dApp. This is **required** as we don't provide a default for it in the `Dockerfile`. See the _Infura Dashboard_ for the correct project id
- `LOADER`: Loader value to be passed to the dApp's environment, defaults to `network`
- `NETWORK`: Network value to be passed to the dApp's environment, defaults to `goerli`
- `VERBOSE`: If the dApp's console output should be verbose or not, defaults to `false`
- `COLONY_NETWORK_ENS_NAME`: The ENS name the dApp should use for users and colonies, defaults to `joincolony.eth`
- `SERVER_ENDPOINT`: Address of the appollo server to connect to, defaults to `http://127.0.0.1:3000`

Usage example:
```bash
docker build --build-arg INFURA_ID='XXX' --build-arg COLONY_NETWORK_ENS_NAME='joincolony.test' --build-arg VERBOSE='true' --no-cache .
```

#### Building from a different branch / commit hash

If you want to build the docker image from a different part of the repo, you can specify either a branch or a commit hash via the `COMMMIT_HASH` build argument which will checkout the repo at that point in time.

Usage example:
```bash
docker build --build-arg COMMMIT_HASH='feature/cool-new-feature' --no-cache .
```

## License

MIT
