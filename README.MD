# What is this fork about?
Fork of [lchaia/monerod](https://github.com/leonardochaia/docker-monerod)

- Run as nonroot user on a distroless image
- Proper volume mounting of /monero, that works also for userns-remap

# Docker Monerod

This image clones `https://github.com/monero-project/monero` and builds it according to instructions.

## Usage

Append any `monerod` params after the image name. i.e:

```shell
docker run -it --rm enucatl/monerod --help
```

### Blockchain Volume

The blockchain will get downloaded at `/home/monero/.bitmonero`, so you should
volume-map that path for persistance

## Docker Compose example

See [docker-compose.yml](docker-compose.yml) and modify it for your environment.

## Copyrights

Code from this repository is released under MIT license.
[Monero License](https://github.com/monero-project/monero/blob/master/LICENSE)

## Donations

Please consider [donating to Monero](https://ccs.getmonero.org/)

## Security baseline

This compose project uses the shared [docker-compose-security-baseline](https://github.com/Enucatl/docker-compose-security-baseline) for common container hardening defaults, including capabilities, no-new-privileges, memory/swap, and PID limits.
