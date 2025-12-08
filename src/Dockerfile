ARG MONERO_BRANCH=v0.18.4.4

FROM debian:13-slim as build
ARG MONERO_BRANCH

# Dependency list from https://github.com/monero-project/monero#compiling-monero-from-source
# Added DEBIAN_FRONTEND=noninteractive to workaround tzdata prompt on installation
COPY src/binaryfate.asc monerokey.asc
RUN apt update && DEBIAN_FRONTEND=noninteractive apt -y install ca-certificates bzip2 wget gpg\
  && rm -rf /var/lib/apt/lists/*\
  && wget --no-verbose https://downloads.getmonero.org/cli/linux64/monero-linux-x64-$MONERO_BRANCH.tar.bz2\
  && wget --no-verbose https://www.getmonero.org/downloads/hashes.txt\
  && gpg --import monerokey.asc\
  && gpg --verify hashes.txt\
  && echo $MONERO_BRANCH\
  && grep $(sha256sum monero-linux-x64-$MONERO_BRANCH.tar.bz2) hashes.txt \
  && tar xjfv monero-linux-x64-$MONERO_BRANCH.tar.bz2 \
  && chmod +x "monero-x86_64-linux-gnu-$MONERO_BRANCH/monerod" \
  && mkdir -p /monero

# Clean layer for the runtime image
FROM gcr.io/distroless/base-debian13
ARG MONERO_BRANCH
COPY --from=build monero-x86_64-linux-gnu-$MONERO_BRANCH/monerod /usr/local/bin/monerod
COPY --from=build --chown=nonroot:nonroot /monero /monero

# Expose p2p and restricted RPC ports
EXPOSE 18080
EXPOSE 18089

USER nonroot:nonroot
VOLUME /monero

ENTRYPOINT ["monerod", "--data-dir", "/monero", "--non-interactive"]
CMD ["--help"]
