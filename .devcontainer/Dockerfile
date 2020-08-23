# **********************************************************
# * Note: Using a Dockerfile is optional when using Docker *
# * Compose but has been included here for completeness.   *
# **********************************************************
FROM elixir:1.10-alpine

# This Dockerfile adds a non-root user with sudo access. Use the “remoteUser” property in
# devcontainer.json to use it. More info: https://aka.ms/vscode-remote/containers/non-root-user.
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Options for common setup script
ARG INSTALL_ZSH="true"
ARG COMMON_SCRIPT_SOURCE="https://raw.githubusercontent.com/microsoft/vscode-dev-containers/master/script-library/common-alpine.sh"
ARG COMMON_SCRIPT_SHA="dev-mode"

# Install needed packages and setup non-root user. Use a separate RUN statement to add your own dependencies.
RUN apk update \
  && apk add --no-cache curl ca-certificates \
  && curl -sSL  ${COMMON_SCRIPT_SOURCE} -o /tmp/common-setup.sh \
  && if [ "$COMMON_SCRIPT_SHA" != "dev-mode" ]; then echo "$COMMON_SCRIPT_SHA */tmp/common-setup.sh" | sha256sum -c - ; fi \
  && /bin/ash /tmp/common-setup.sh "${INSTALL_ZSH}" "${USERNAME}" "${USER_UID}" "${USER_GID}" \
  && rm /tmp/common-setup.sh

# ** [Optional] Uncomment this section to install additional packages. **
RUN apk update \
  && apk add --no-cache nodejs npm inotify-tools

USER vscode

RUN mix local.hex --force \
  && mix archive.install hex phx_new 1.5.4
