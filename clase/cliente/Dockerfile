
FROM  debian:13

RUN apt update && \
    apt install -y iproute2 procps iputils-ping dnsutils && \
    apt clean && \
    rm -rf var/lib/apt/lists/*

ENTRYPOINT ["sleep","infinity"]