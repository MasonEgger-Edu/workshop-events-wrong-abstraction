# ABOUTME: Sandbox image that carries vendored source code and pre-warmed uv
# cache. Instruqt setup script pulls this image and extracts assets.
FROM python:3.14-slim

# Install uv so we can pre-warm the dependency cache
COPY --from=ghcr.io/astral-sh/uv:latest /uv /usr/local/bin/uv

# Copy vendored source code into the image
COPY instruqt/assets/ /opt/assets/

# Pre-warm uv cache: sync each project so deps are downloaded and cached.
# The .venv directories are path-specific but the global uv cache at
# /root/.cache/uv is portable — we copy that to the VM during setup.
RUN cd /opt/assets/durable-vs-normal-execution/durable && uv sync && rm -rf .venv
RUN cd /opt/assets/durable-vs-normal-execution/normal && uv sync && rm -rf .venv
RUN cd /opt/assets/durable-wordle && uv sync --frozen && rm -rf .venv

CMD ["/bin/bash"]
