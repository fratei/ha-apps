# Ollama's official image, pinned. The weekly update workflow raises this pin
# after a test build of the new release.
ARG OLLAMA_VERSION=0.40.16
FROM ollama/ollama:${OLLAMA_VERSION}

ARG BUILD_VERSION
ARG BUILD_ARCH

LABEL \
    io.hass.version="${BUILD_VERSION}" \
    io.hass.type="app" \
    io.hass.arch="${BUILD_ARCH}" \
    org.opencontainers.image.title="Ollama" \
    org.opencontainers.image.description="Local AI models for Home Assistant, with Ollama's cloud turned off" \
    org.opencontainers.image.version="${BUILD_VERSION}" \
    org.opencontainers.image.source="https://github.com/fratei/ha-apps"

ENV \
    OLLAMA_HOST=0.0.0.0:11434 \
    OLLAMA_MODELS=/data/models \
    OLLAMA_NO_CLOUD=1 \
    OLLAMA_KEEP_ALIVE=5m \
    OLLAMA_MAX_LOADED_MODELS=1 \
    OLLAMA_NUM_PARALLEL=1

# Each check adds two request lines to the log, so it runs every 5 minutes once
# the server is up. Supervisor shows the app as started after the first success.
HEALTHCHECK --interval=5m --timeout=30s --start-period=2m --start-interval=10s --retries=3 \
    CMD ["/usr/bin/ollama", "list"]

# Low CPU priority, so Home Assistant gets the CPU first when both are busy.
ENTRYPOINT ["/usr/bin/nice", "-n", "10", "/usr/bin/ollama"]
CMD ["serve"]
