FROM --platform=$TARGETOS/$TARGETARCH rust:slim

LABEL author="Ethan Coward" maintainer="ethan.coward@icloud.com"

# Installa i pacchetti necessari e crea l'utente "container"
RUN apt update \
    && apt -y install git dnsutils curl iproute2 ffmpeg tini pkg-config \
    && useradd -m -d /home/container container

# Imposta le variabili d'ambiente e usa l'utente non root
USER container
ENV USER=container \
    HOME=/home/container \
    CARGO_HOME=/home/container/.cargo \
    RUSTUP_HOME=/home/container/.rustup \
    TMPDIR=/home/container/tmp

# Imposta la working directory e crea la directory tmp scrivibile
WORKDIR /home/container

RUN mkdir -p /home/container/tmp && \
    chown -R container:container /home/container/tmp

# Copia lo script di entrypoint
COPY --chown=container:container ./../entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

# Usa tini come init system ed esegui lo script
ENTRYPOINT ["/usr/bin/tini", "-g", "--"]
CMD ["/entrypoint.sh"]
