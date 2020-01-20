# colony-docker-images

Colony docker images for hub auto-builds

## Images

### circleci-cypress
An image based on circleci/node which also contains the cypress testing dependencies

### dapp

The dApp production docker image builds the production version of the bundle, moves it to separate folder, and serves it via a simple `nginx` server on port `80`

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

### server

The server production docker image builds the production version of the bundle, starts the express server and serves it, by default on port `3000`.

#### Build Args

This docker file allows you to pass in build time arguments to change the environment the server bundle is being built with. There are tree build arguments that are **required**: `INFURA_ID`, `JWT_SECRET` and your github token passed into `GH_PAT`

Build args:
- `GH_PAT`: the GitHub Personal Access Token **required** to authenticate and pull information from our private repo _(Note: you **have to** supply this yourself, otherwise it won't work)_.
- `INFURA_ID`: The infura project id we are using as a provider. This is **required** as we don't provide a default for it in the `Dockerfile`. See the _Infura Dashboard_ for the correct project id
- `JWT_SECRET`: The secret string to use for authentication. [More on JWT here](https://stackoverflow.com/a/28503265)
- `NETWORK`: Network value to be passed to the environment, defaults to `goerli`
- `NETWORK_CONTRACT_ADDRESS`: The address of the ETHRouter contract deployed for the current network. Defaults to _(as the `NETWORK` above)_ to the `goerli` deployed one: `0x79073fc2117dD054FCEdaCad1E7018C9CbE3ec0B`
- `APOLLO_PORT`: The port the server will be accessible on, defaults to `3000`
- `DISABLE_EXPIRY_CHECK`: Disable the expiry logic check. Defaults to `false`. **Do not** set to `true` while using it in production.
- `DISABLE_AUTH_CHECK`: Disable the authentication logic check. Defaults to `false`. **Do not** set to `true` while using it in production.

Usage example:
```bash
docker build --build-arg GH_PAT='XXX' --build-arg INFURA_ID='XXX' --build-arg JWT_SERCRET='this-should-be-really-really-secret' --no-cache .
```

#### Building from a different branch / commit hash

If you want to build the docker image from a different part of the repo, you can specify either a branch or a commit hash via the `COMMMIT_HASH` build argument which will checkout the repo at that point in time.

Usage example:
```bash
docker build --build-arg COMMMIT_HASH='feature/cool-new-feature' --no-cache .

## License

MIT
