#FROM python:3.10.2-alpine@sha256:4eff19dfce481c125674c902b24aa6667b9bc166f6bbcae79a171ce2e6c64ee1
ARG BUILD_FROM
ARG BUILD_VERSION
FROM ${BUILD_FROM}

ARG TEMPIO_VERSION BUILD_ARCH
RUN \
    curl -sSLf -o /usr/bin/tempio \
    "https://github.com/home-assistant/tempio/releases/download/${TEMPIO_VERSION}/tempio_${BUILD_ARCH}"

# Copy root filesystem
COPY rootfs /

ARG PIP_VERSION
ARG SELENIUM_VERSION

RUN apk add --no-cache gcc libc-dev libffi-dev && \
    pip install --no-cache-dir pip=="$PIP_VERSION" && \
    pip install --no-cache-dir --user selenium=="$SELENIUM_VERSION"



#FROM python:3.10.2-alpine@sha256:4eff19dfce481c125674c902b24aa6667b9bc166f6bbcae79a171ce2e6c64ee1
FROM ${BUILD_FROM}

RUN apk add --no-cache chromium chromium-chromedriver && \
    rm -rf /var/cache/apk/* /tmp/* /usr/share/doc

COPY --from=0 /root/.local /root/.local

ENV PATH=/root/.local/bin:$PATH

ADD renew.py .

ENTRYPOINT ["python3", "renew.py"]
