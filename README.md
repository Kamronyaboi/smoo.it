This branch shows how `api2.smoo.it` is setup as an example to others.

It runs on a free Google Cloud `e2-micro` VM.

## Structure

The following folders reside in the user's home directory which persists VM restarts.

### `~/api2.smoo.it/`

This contains the actual status check software.

A git cloned `api` branch with a slightly modified `docker-compose.yml` and `html/data/.env`.

### `~/ddclient/`

Cronjob that automatically detects public IP address changes to update the DynDNS entry for `api2-smoo-it.spdns.de`.

(There's a `CNAME` from `api2.smoo.it` to `api2-smoo-it.spdns.de`.)

### `~/proxy/`

HTTP & HTTPS proxy that handles TLS, automatic Let's Encrypt certificate renewal, and forwards requests to other docker containers based on their labels.

## Secrets

`<redacted>` secrets in:
- `/api2.smoo.it/html/data/.env.example`
  - This is an example file for how `/api2.smoo.it/html/data/.env` should be.
- `/ddclient/config/ddclient.conf`
  - Not publising my password for the DynDNS service.
- `/proxy/traefik.toml`
  - Not publishing my Let's Encrypt e-mail address.

## How to setup `docker compose` on an Google Cloud VM

```sh
# download compose to a path that is executable and is preserved on VM restarts
curl -SL https://github.com/docker/compose/releases/download/v2.29.6/docker-compose-linux-x86_64 -o /var/lib/google/docker-compose

# create a symlink to integrate it into the `docker` command of the current user
ln -s /var/lib/google/docker-compose/ ~/.docker/cli-plugins/docker-compose
```
