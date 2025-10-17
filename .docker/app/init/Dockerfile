ARG BASE_IMAGE=node:18-buster-slim
FROM ${BASE_IMAGE}

ENV DEBIAN_FRONTEND=noninteractive

ARG SHINOBI_BRANCH=dev
# Usar archive.debian.org para Buster
RUN sed -i 's|http://deb.debian.org/debian|http://archive.debian.org/debian|g' /etc/apt/sources.list \
    && echo 'Acquire::Check-Valid-Until "false";' > /etc/apt/apt.conf.d/99no-check-valid-until

RUN apt-get update
RUN apt-get install -y build-essential \
    curl \
    git \
    ffmpeg \
    default-mysql-client \
    procps \
    && rm -rf /var/lib/apt/lists/*  # Clean up to reduce image size

RUN git clone --branch $SHINOBI_BRANCH https://gitlab.com/Shinobi-Systems/Shinobi.git /opt/shinobi

WORKDIR /opt/shinobi
RUN npm install && npm install pm2 -g

WORKDIR /home/Shinobi

COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

EXPOSE 8080

ENTRYPOINT ["/entrypoint.sh"]
