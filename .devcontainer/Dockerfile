FROM python:3.10

# Create a non-root user with sudo privileges
# as explained in https://code.visualstudio.com/remote/advancedcontainers/add-nonroot-user
ARG USERNAME=Johannes
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME && \
    useradd --uid $USER_UID --gid $USER_GID -m $USERNAME && \
    apt-get update && \
    apt-get install -y sudo && \
    echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME && \
    chmod 0440 /etc/sudoers.d/$USERNAME

# Add bash autocompletion
RUN apt-get install bash-completion

# Install azure-cli
# based on https://github.com/devcontainers/features/tree/main/src/azure-cli
RUN curl -sL https://raw.githubusercontent.com/devcontainers/features/main/src/azure-cli/install.sh -o azure-cli.sh && \
    chmod +x azure-cli.sh && \
    ./azure-cli.sh

# Enable docker-from docker
# based on https://github.com/devcontainers/features/tree/main/src/docker-from-docker &
# https://github.com/microsoft/vscode-dev-containers/tree/main/containers/docker-from-docker
RUN curl -sL https://raw.githubusercontent.com/devcontainers/features/main/src/docker-from-docker/install.sh -o docker-from-docker.sh && \
    chmod +x docker-from-docker.sh && \
    ./docker-from-docker.sh

# Persist the bash history
# based on https://code.visualstudio.com/remote/advancedcontainers/persist-bash-history
RUN SNIPPET="export PROMPT_COMMAND='history -a' && export HISTFILE=/commandhistory/.bash_history" && \
    mkdir /commandhistory && \
    touch /commandhistory/.bash_history && \
    chown -R $USERNAME /commandhistory && \
    echo "$SNIPPET" >> "/home/$USERNAME/.bashrc"

# Install python requirements
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt


# ********************************************************
# * Anything else you want to do goes here *
# ********************************************************

USER $USERNAME
