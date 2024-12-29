<div align="center">
  <h1
    style="font-size: 3rem; font-weight: bold; color: rgb(150, 108, 190);"
    >
    Vaultwarden
  </h1>
  <h3>
    My Vaultwarden setup
  </h3>
</div>

Vaultwarden is a lightweight, efficient, and self-hosted alternative to traditional password managers. It offers robust security features, easy deployment, and full control over your data, making it an excellent choice for managing passwords and passkeys.

This repository contains my Vaultwarden setup.

## Deployment

Deploying Vaultwarden is straightforward and can be done in a few simple steps. Here's how you can set up Vaultwarden on your server; starting with installing docker for [Docker's](https://docs.docker.com/engine/install/debian/) installation guide.

There is also a convenient script.

```bash
curl -fsSL https://get.docker.com | sh
```

Check to see if Docker is installed correctly by booting the hello-world container `docker run hello-world`. As long as you see the message `Hello from Docker!`, you're good to go.

For just the Vaultwarden container, you can use the following command:

```bash
docker run -d --name vaultwarden -p 80 -v /vw-data:/data vaultwarden/server:latest
```

For a more detailed setup, you can use the `docker-compose.yml` file in this repository. Simply clone the repository and create a `.env` file with the following content:

```bash
# Vaultwarden
ADMIN_TOKEN='<admin_token>'
DOMAIN_NAME='https://vaultwarden.example.com'

# MariaDB
VAULTWARDEN_PASSWD='<vaultwarden_passwd>'
MYSQL_ROOT_PASSWD='<mysql_root_passwd>'
```

To get the `ADMIN_TOKEN` use the following, `docker run --rm -it vaultwarden/server /vaultwarden hash` follow the prompt. You well see the `ADMIN_TOKEN` in the output. Copy and paste the full token into the `.env` file. Thought it is possible to use a simple password, it is recommended to use a token for security reasons.

Then run the following command:

```bash
docker compose -f /path/to/docker-compose.yml up -d
```

## Setting up Systemd Service

To set up the Vaultwarden service, first create a new directory `mkdir -v /etc/vaultwarden` and copy the _docker-compose.yml_ and _.env_ files into it. Then copy the updated _vaultwarden.{service,timer}_ files into `/usr/lib/systemd/system/`.

```bash
systemctl enable --now vaultwarden.timer
```

You Vaultwarden container should now be running and thanks to the timer it will update once a week.
