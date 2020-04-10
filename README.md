# Simple Docker Setup for Foundry VTT

This is a simple Dockerfile and docker-compose.yml to get [Foundry VTT]([https://foundryvtt.com/](https://foundryvtt.com/)) up and running with Docker quickly. They are based on [mikysan's Dockerfile](https://github.com/mikysan/simple-fvtt-dockerfile).

They should work out of the box with only a tiny amount editing, but please note this assumes you have access to Foundry VTT through Patreon, and a working Docker installation with some minimal knowledge. I have also [written this up on my blog](http://blog.roberthallam.org/2020/04/running-foundry-vtt-with-docker/).

## Six Step Setup

1. Create a directory for Foundry VTT on the Docker host
2. Clone this repo (or download the [Dockerfile](https://raw.githubusercontent.com/bertiebaggio/foundryvtt-docker/master/Dockerfile) and [docker-compose.yml](https://raw.githubusercontent.com/bertiebaggio/foundryvtt-docker/master/docker-compose.yml)) into that directory
3. Edit the Dockerfile and change FOUNDRY_ZIP_URL from example.com to the URL provided on Patreon (or if you have the zip, you can use a COPY statement, see the Dockerfile for details)
4. If using a bind mount, create the directory for data persistence as ./foundryvtt-data (see docker-compose.yml for details + alternatives)
5. run `docker-compose build` to create the image
6. run `docker-compose up` to run the application; if all is successful you can terminate with Ctrl+C and run `docker compose up -d` to detach and run the the background

## Troubleshooting

If the container exits with an error like the following:
```bash
foundryvtt | (node:1) UnhandledPromiseRejectionWarning: TypeError: Cannot destructure property 'app' of '_0x1b0815' as it is undefined.
foundryvtt | at initialize (/home/foundry/app/resources/app/dist/init.js:1:3804)
foundryvtt | at Object. (/home/foundry/app/resources/app/main.js:153:16)
foundryvtt | at Module._compile (internal/modules/cjs/loader.js:1158:30)
foundryvtt | at Object.Module._extensions..js (internal/modules/cjs/loader.js:1178:10)
foundryvtt | at Module.load (internal/modules/cjs/loader.js:1002:32)
foundryvtt | at Function.Module._load (internal/modules/cjs/loader.js:901:14)
foundryvtt | at Function.executeUserEntryPoint [as runMain](http://blog.roberthallam.org/2020/04/running-foundry-vtt-with-docker/internal/modules/run_main.js:74:12)
foundryvtt | at internal/main/run_main_module.js:18:47
foundryvtt | (node:1) UnhandledPromiseRejectionWarning: Unhandled promise rejection. This error originated either by throwing inside of an async function without a catch b
lock, or by rejecting a promise which was not handled with .catch(). To terminate the node process on unhandled promise rejection, use the CLI flag `--unhandled-rejections=st rict` (see https://nodejs.org/api/cli.html#cli_unhandled_rejections_mode). (rejection id: 1)
foundryvtt | (node:1) [DEP0018] DeprecationWarning: Unhandled promise rejections are deprecated. In the future, promise rejections that are not handled will terminate the
Node.js process with a non-zero exit code.
```

then there is a permissions issue with the data persistence directory. 

Recreate it, make sure it matches FOUNDRY_DATA_DIR in the Dockerfile, and make sure it is owned and read-writeable by UID 1000 (or whatever you changed this to).

## Further Reading

- [Docker setup](https://foundry-vtt-community.github.io/wiki/Docker/) on the Foundry VTT community wiki
- [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) from Docker
- [docker-compose reference](https://docs.docker.com/compose/compose-file/) from Docker
- [Traefik](https://docs.traefik.io/) for automatic routing and termination of Docker services

Further help may be found on the #installation-support channel on the [Foundry Discord](https://discordapp.com/invite/DDBZUDf).
