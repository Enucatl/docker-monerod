ARG MONERO_BRANCH=v0.17.3.0

# Use Ubuntu for the build image base
FROM debian:stable-slim as build

# Dependency list from https://github.com/monero-project/monero#compiling-monero-from-source
# Added DEBIAN_FRONTEND=noninteractive to workaround tzdata prompt on installation
COPY binaryfate.asc monerokey.asc
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get -y install ca-certificates bzip2 wget gpg\
    && rm -rf /var/lib/apt/lists/*\
    && wget --no-verbose https://downloads.getmonero.org/cli/linux64/monero-linux-x64-v0.17.3.0.tar.bz2\
    && wget --no-verbose https://www.getmonero.org/downloads/hashes.txt\
    && gpg --import monerokey.asc\
    && gpg --verify hashes.txt\
    && grep $(sha256sum monero-linux-x64-v0.17.3.0.tar.bz2) hashes.txt \
    && tar xjf monero-linux-x64-v0.17.3.0.tar.bz2\
    && chmod +x monero-x86_64-linux-gnu-v0.17.3.0/monerod

# Clean Ubuntu layer for the runtime image
FROM gcr.io/distroless/base-debian11
COPY --from=build monero-x86_64-linux-gnu-v0.17.3.0/monerod /usr/local/bin/monerod

# Expose p2p and restricted RPC ports
EXPOSE 18080
EXPOSE 18089

# Start monerod with required --non-interactive flag and sane defaults that are overridden by user input (if applicable)
ENTRYPOINT ["monerod"]
CMD ["--help"]
