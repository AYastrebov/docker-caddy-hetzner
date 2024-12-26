# 🐳 caddy-hetzner

[![DockerHub](https://img.shields.io/badge/DockerHub-ayastrebov%2Fcaddy--hetzner-blue?style=flat)](https://hub.docker.com/r/ayastrebov/caddy-hetzner)
[![GitHub tag (latest SemVer)](https://img.shields.io/github/v/tag/AYastrebov/docker-caddy-hetzner?label=Latest%20Version)](https://github.com/AYastrebov/docker-caddy-hetzner/tags)
[![Docker Build & Release](https://github.com/AYastrebov/docker-caddy-hetzner/actions/workflows/docker.yml/badge.svg)](https://github.com/AYastrebov/docker-caddy-hetzner/actions/workflows/docker.yml)
[![License](https://img.shields.io/github/license/AYastrebov/docker-caddy-hetzner)](https://github.com/AYastrebov/docker-caddy-hetzner/blob/main/LICENSE)
[![Dependabot](https://img.shields.io/badge/Dependabot-0366d6?style=flat&logo=dependabot&logoColor=white)](.github/dependabot.yml)

The [Caddy](https://hub.docker.com/_/caddy) Docker image with the added
[caddy-dns/hetzner](https://github.com/caddy-dns/hetzner) module for
DNS-01 ACME validation support.  
Built for the same platforms as the upstream Caddy project (except Windows, sorry)!

> [!NOTE]
> This image does not change anything with original Caddy Docker image except
> replace the `caddy` binary.

```sh
# Docker Hub
docker pull ayastrebov/caddy-hetzner:latest

# GHCR
docker pull ghcr.io/ayastrebov/caddy-hetzner:latest
```

## Tags

The following tags are the latest available for the
`ayastrebov/caddy-hetzner` image.

<!--START-TAGS-->

- [`latest`](https://github.com/AYastrebov/docker-caddy-hetzner/tree/v2.8.4)
- [`2.8.4`](https://github.com/AYastrebov/docker-caddy-hetzner/tree/v2.8.4)
- [`2.8`](https://github.com/AYastrebov/docker-caddy-hetzner/tree/v2.8)
- [`2`](https://github.com/AYastrebov/docker-caddy-hetzner/tree/v2)
<!--END-TAGS-->

Check the GitHub
[Tags](https://github.com/AYastrebov/docker-caddy-hetzner/tags)
for a list of all versions!

## Usage

### Docker

Since this is built off the official Docker image all of the same Volumes,
Environment variables, etc. can be used with this container. Please refer to the
official [Caddy](https://hub.docker.com/_/caddy) Docker image and
[docs](https://caddyserver.com/docs/) for more information on using Caddy.

Simply create the container as usual and include your `CF_API_TOKEN` (email no
longer required for API Tokens). We can utilize Caddy's support for
[Environment varaiables](https://caddyserver.com/docs/caddyfile/concepts#environment-variables)
to pass these values into our `Caddyfile`.

> [!IMPORTANT]
> Be sure to create your `Caddyfile` beforehand to avoid Docker startup errors.

```sh
docker run --rm -it \
  --name caddy \
  -p 80:80 \
  -p 443:443 \
  -v caddy_data:/data \
  -v caddy_config:/config \
  -v $PWD/Caddyfile:/etc/caddy/Caddyfile \
  -e HZ_API_TOKEN=UhKLc...JD9jk \
  ayastrebov/caddy-hetzner:latest
```

If you prefer `docker-compose` you can use the provided
[`docker-compose.yml`](docker-compose.yml) after updating it with your
`HZ_API_TOKEN`.

```sh
docker compose up
```

### Caddy

Set the global
[acme_dns](https://caddyserver.com/docs/caddyfile/options#acme-dns) directive
in your `Caddyfile`

```Caddyfile
{
  acme_dns hetzner {env.HZ_API_TOKEN}
}
```

or via JSON

```json
{
  "module": "acme",
  "challenges": {
    "dns": {
      "provider": {
        "name": "hetzner",
        "api_token": "{env.HZ_API_TOKEN}"
      }
    }
  }
}
```

See the [caddy-dns/hetzner](https://github.com/caddy-dns/hetzner) module
and [`tls`](https://caddyserver.com/docs/caddyfile/directives/tls#tls) directive
for advanced usage.

### Creating a Hetzner API Token

You can generate a Cloudflare API token via the Cloudflare web console using the following steps:

1. Login to your [Dashboard](https://dash.cloudflare.com/)
2. Go to [Account Profile](https://dash.cloudflare.com/profile) > [API Tokens](https://dash.cloudflare.com/profile/api-tokens)
3. Click "Create token" (Use the "Create Custom Token" option)
4. Grant the following permissions:
   - `Zone > Zone > Read`
   - `Zone > DNS > Edit`

## Building

You can easily build the Docker image locally by doing

```sh
docker build -t caddy-hetzner .
```

If you prefer `docker-compose` you can use the provided
[`docker-compose.yml`](docker-compose.yml) for building

```sh
docker compose build
```

## Container signatures

All container images will be automatically signed via
[Cosign](https://docs.sigstore.dev/cosign/overview/) using
[keyless signatures](https://docs.sigstore.dev/cosign/keyless/). You can use the
following command to verify the integrity of these images yourself.

```sh
cosign verify \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com \
  --certificate-identity-regexp https://github.com/AYastrebov/docker-caddy-hetzner/.github/workflows/ \
  ayastrebov/caddy-hetzner:latest
```

## Contributing

Feel free to contribute and make things better by opening an
[Issue](https://github.com/AYastrebov/docker-caddy-hetzner/issues) or
[Pull Request](https://github.com/AYastrebov/docker-caddy-hetzner/pulls).

## License

See [LICENSE](https://github.com/AYastrebov/docker-caddy-hetzner/blob/main/LICENSE).
