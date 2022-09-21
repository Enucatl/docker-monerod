ARG MONERO_BRANCH=v0.18.1.1

# Use Ubuntu for the build image base
FROM debian:stable-slim as build
ARG MONERO_BRANCH

# Dependency list from https://github.com/monero-project/monero#compiling-monero-from-source
# Added DEBIAN_FRONTEND=noninteractive to workaround tzdata prompt on installation
COPY src/binaryfate.asc monerokey.asc
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get -y install ca-certificates bzip2 wget gpg\
    && rm -rf /var/lib/apt/lists/*\
    && wget --no-verbose https://downloads.getmonero.org/cli/linux64/monero-linux-x64-$MONERO_BRANCH.tar.bz2\
    && wget --no-verbose https://www.getmonero.org/downloads/hashes.txt\
    && gpg --import monerokey.asc\
    && gpg --verify hashes.txt\
    && echo $MONERO_BRANCH\
    && grep $(sha256sum monero-linux-x64-$MONERO_BRANCH.tar.bz2) hashes.txt \
    && tar xjfv monero-linux-x64-$MONERO_BRANCH.tar.bz2\
    && chmod +x "monero-x86_64-linux-gnu-$MONERO_BRANCH/monerod"

# Clean Ubuntu layer for the runtime image
FROM gcr.io/distroless/base-debian11
ARG MONERO_BRANCH
COPY --from=build monero-x86_64-linux-gnu-$MONERO_BRANCH/monerod /usr/local/bin/monerod

# Expose p2p and restricted RPC ports
EXPOSE 18080
EXPOSE 18089

# Start monerod with required --non-interactive flag and sane defaults that are overridden by user input (if applicable)
ENTRYPOINT ["monerod"]
CMD ["--help"]
