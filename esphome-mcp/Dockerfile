ARG BUILD_FROM
FROM ${BUILD_FROM}

# BUILD_FROM is ghcr.io/esphome/docker-base:debian-ha-addon-<version> — the
# same Debian + bashio + s6-overlay base the official ESPHome add-on uses.
# A plain Alpine HA base (the previous BUILD_FROM) fails to compile esp-idf
# framework devices: pioarduino's esp-idf toolchain pip-installs manylinux
# (glibc) wheels into its own venv, which breaks under musl libc with
# "Failed to install Python dependencies into penv". Debian avoids this and
# already ships git/build-essential/curl, so no apk/apt step is needed here.

# Install Python dependencies and ESPHome
COPY requirements.txt /tmp/
RUN pip3 install --no-cache-dir -r /tmp/requirements.txt \
    && pip3 install --no-cache-dir esphome \
    && rm /tmp/requirements.txt

# Copy server code
COPY server/ /opt/server/
COPY run.sh /opt/

RUN chmod a+x /opt/run.sh

WORKDIR /opt

CMD ["/opt/run.sh"]
