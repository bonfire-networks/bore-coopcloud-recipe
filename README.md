# bore

> A modern, simple TCP tunnel* in Rust that exposes local ports to a remote server, bypassing standard NAT connection firewalls. That's all it does: no more, and no less.

* plus HTTPS proxying by Traefik in our case

<!-- metadata -->

* **Category**: Tools
* **Status**: 0
* **Image**: [`bore`](https://hub.docker.com/r/ekzhang/bore), 4, upstream
* **Healthcheck**: No
* **Backups**: No
* **Email**: No
* **Tests**: No
* **SSO**: No

<!-- endmetadata -->

## Quick start

* `abra app new bore`
* `abra app config <app-name>` — set `SECRET` in `.env` if you want authentication
* `abra app deploy <app-name>`

## Usage

Install the cli on your machine: https://github.com/ekzhang/bore

Then you can expose a local port through the tunnel:

```
bore local 4000 --to yourapp.yourserver.tld --port 1 --secret <SECRET>
```

(or any number from 1 to 10 for the port, which will match the subdomain used)

Then access it at e.g. `https://1.yourapp.yourserver.tld`


## Configuration

- `SECRET`: Optional authentication secret. Clients must provide this to create tunnels.
- `CONTROL_PORT`: Port where bore clients connect (default: `7835`).
- `MIN_PORT` / `MAX_PORT`: Tunnel port range (default: `1`–`10`). Each port maps to a subdomain (`<port>.${DOMAIN}`). Must match the Traefik labels in `compose.yml`.

## Notes

- Requires wildcard DNS (`*.yourapp.yourserver.tld`) pointing at your server.
- Traefik will set up TLS and proxies HTTPS to bore's tunnel ports. 
- To add more tunnel slots beyond 10, add corresponding Traefik labels in `compose.yml` and increase `MAX_PORT`.
- TODO: find a way to put the secrets in docker secrets instead of env (seems not possible with current docker image since it uses `scratch` with no shell)

For more, see [`docs.coopcloud.tech`](https://docs.coopcloud.tech).
