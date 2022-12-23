## Running

First, create a volume for the data directory created in the above section:

```
docker volume create \
  --opt type=none \
  --opt o=bind \
  --opt device="/path/to/data/dir" data_volume
```

Optional: If you want to use the postgres container defined in
`docker-compose.yaml`, start that first:

```
docker-compose up -d postgres
```

Start the bot with:

```
docker-compose up my-project-name
```

This will run the bot and log the output to the terminal. You can instead run
the container detached with the `-d` flag:

```
docker-compose up -d my-project-name
```

(Logs can later be accessed with the `docker logs` command).

This will use the `latest` tag from
[Docker Hub](https://hub.docker.com/somebody/my-project-name).

If you would rather run from the checked out code, you can use:

```
docker-compose up local-checkout
```

This will build an optimized, production-ready container. If you are developing
instead and would like a development container for testing local changes, use
the `start-dev.sh` script and consult [CONTRIBUTING.md](../CONTRIBUTING.md).

**Note:** If you are trying to connect to a Synapse instance running on the
host, you need to allow the IP address of the docker container to connect. This
is controlled by `bind_addresses` in the `listeners` section of Synapse's
config. If present, either add the docker internal IP address to the list, or
remove the option altogether to allow all addresses.

## Updating

To update the container, navigate to the bot's `docker` directory and run:

```
docker-compose pull my-project-name
```

Then restart the bot.

## Systemd

A systemd service file is provided for your convenience at
[my-project-name.service](my-project-name.service). The service uses
`docker-compose` to start and stop the bot.

Copy the file to `/etc/systemd/system/my-project-name.service` and edit to
match your setup. You can then start the bot with:

```
systemctl start my-project-name
```

and stop it with:

```
systemctl stop my-project-name
```

To run the bot on system startup:

```
systemctl enable my-project-name
```

## Building the image

To build a production image from source, use the following `docker build` command
from the repo's root:

```
docker build -t somebody/my-project-name:latest -f docker/Dockerfile .
```
