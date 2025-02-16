# Installation

## Binary
- Download the [latest release](https://github.com/knadh/listmonk/releases) and extract the listmonk binary.
- `./listmonk --new-config` to generate config.toml. Then, edit the file.
- `./listmonk --install` to setup the Postgres DB.
- Run `./listmonk` and visit `http://localhost:9000`.

## Docker

The latest image is available on DockerHub at `listmonk/listmonk:latest`

Use the sample [docker-compose.yml](https://github.com/knadh/listmonk/blob/master/docker-compose.yml) to run listmonk and Postgres DB with docker-compose as follows:

### Demo

```
wget -O docker-compose.yml https://raw.githubusercontent.com/knadh/listmonk/master/docker-compose.yml
docker-compose up -d demo-db demo-app
```

The demo does not persist Postgres after the containers are removed. **DO NOT** use this demo setup in production.

### Production
- `wget -O docker-compose.yml https://raw.githubusercontent.com/knadh/listmonk/master/docker-compose.yml` to download the `docker-compose.yml` locally.
- `docker-compose up db` to run the Postgres DB.
- `docker-compose run --rm app ./listmonk --install` to setup the DB.
- Run `docker-compose up app db` and visit `http://localhost:9000`.

### Mounting a custom config.toml
To mount a local `config.toml` file, add the following section to `docker-compose.yml`:

```
  app:
    <<: *app-defaults
    depends_on:
      - db
    volume:
    - ./path/on/your/host/config.toml/:/listmonk/config.toml
```

This will mount the local `config.toml` inside the container at `listmonk/config.toml`. The example `docker-compose.yml` file works with Docker Engine 18.06.0+ and `docker-compose` which supports file format 3.7.

### Note
- See [configuring with environment variables](../configuration).
- Ensure that both `app` and `db` containers are in running. If the containers are not up, restart them `docker-compose restart app db`.
- Refer to [this tutorial](https://yasoob.me/posts/setting-up-listmonk-opensource-newsletter-mailing/) for setting up a production instance with Docker + Nginx + LetsEncrypt SSL.
