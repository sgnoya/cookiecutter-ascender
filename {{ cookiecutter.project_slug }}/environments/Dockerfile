ARG BASE_IMAGE=ubuntu:24.04
FROM ${BASE_IMAGE}

ARG PROJECT_NAME={{ cookiecutter.project_slug }}
ARG USER_NAME=user
ARG GROUP_NAME=usergroup
ARG UID=1000
ARG GID=1000
ARG PYTHON_VERSION=3.13
ARG APPLICATION_DIRECTORY=/home/${USER_NAME}/${PROJECT_NAME}
ARG RUN_UV_INSTALL_AT_BUILD_TIME="false"


ENV DEBIAN_FRONTEND="noninteractive" \
    LC_ALL="C.UTF-8" \
    LANG="C.UTF-8" \
    PYTHONPATH=${APPLICATION_DIRECTORY}

RUN apt update && apt install --no-install-recommends -y \
    git curl make ssh openssh-client

# Install uv
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/


# Add user. Without this, following process is executed as admin.
# If GID and UID are already exist, change the existing user and group names to ${USER_NAME} and ${GROUP_NAME}.
# If ${USER_NAME} and ${GROUP_NAME} are already exist, change their UID and GID to ${UID} and ${GID}.
# If ${USER_NAME} and ${GROUP_NAME} are not exist, create new user and group with ${USER_NAME} and ${GROUP_NAME}.
# If ${USER_NAME} and ${GROUP_NAME} are not set, use default values "user" and "usergroup".
# If ${UID} and ${GID} are not set, use default values 1000.
RUN groupadd -g ${GID} ${GROUP_NAME} || groupmod -n ${GROUP_NAME} $(getent group ${GID} | cut -d: -f1) \
    && useradd -ms /bin/sh -u ${UID} -g ${GID} ${USER_NAME} || usermod -l ${USER_NAME} $(getent passwd ${UID} | cut -d: -f1)

USER ${USER_NAME}
WORKDIR ${APPLICATION_DIRECTORY}

# If ${RUN_UV_INSTALL_AT_BUILD_TIME} = "true", install Python package by uv and move .venv under ${HOME}.
# This process is for CI (GitHub Actions). To prevent overwrite by volume of docker compose, .venv is moved under ${HOME}.
COPY --chown=${UID}:${GID} pyproject.toml uv.lock ${APPLICATION_DIRECTORY}
RUN test ${RUN_UV_INSTALL_AT_BUILD_TIME} = "true" && uv sync --locked || echo "skip to run uv install."
RUN test ${RUN_UV_INSTALL_AT_BUILD_TIME} = "true" && mv ${APPLICATION_DIRECTORY}/.venv ${HOME}/.venv || echo "skip to move .venv."
