# Docker Monerod

Fork of [lchaia/monerod](https://github.com/leonardochaia/docker-monerod)
Run `monerod` using Docker.

This image clones `https://github.com/monero-project/monero` and builds it according to instructions.

## Usage

Append any `monerod` params after the image name. i.e:

```shell
docker run -it --rm Enucatl/monerod --help
```

### Blockchain Volume

The blockchain will get downloaded at `/home/monero/.bitmonero`, so you should
volume-map that path for persistance

## Docker Compose example

You'll need to modify it for your environment

```yaml
version: "3"

services:
  monerod:
    image: Enucatl/monerod:latest
    container_name: monerod
    volumes:
      - monero:/monero
    ports:
      - 18080:18080
      - 18089:18089
    command:
      - "--log-file=/monero/monerod.log"
      - "--data-dir=/monero"
      - "--non-interactive"
      - "--rpc-use-ipv6"
      - "--p2p-ignore-ipv4"
      - "--p2p-use-ipv6"
      - "--rpc-restricted-bind-ip=0.0.0.0"
      - "--rpc-restricted-bind-ipv6-address=::"
      - "--rpc-restricted-bind-port=18089"
      - "--confirm-external-bind"
      - "--max-concurrency=2"
      - "--public-node"
      - "--prune-blockchain"
      - "--limit-rate-up=20000"
      - "--limit-rate-down=20000"
    restart: unless-stopped

volumes:
  monero:
```

## Copyrights

Code from this repository is released under MIT license.
[Monero License](https://github.com/monero-project/monero/blob/master/LICENSE)

## Donations

Please consider [donating to Monero](https://ccs.getmonero.org/)
