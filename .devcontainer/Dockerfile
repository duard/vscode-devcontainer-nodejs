#-------------------------------------------------------------------------------------------------------------
# Copyright (c) Microsoft Corporation. All rights reserved.
# Licensed under the MIT License. See https://go.microsoft.com/fwlink/?linkid=2090316 for license information.
#-------------------------------------------------------------------------------------------------------------

# For information on the contents of the container image below, see following Dockerfile:
# https://github.com/microsoft/vscode-dev-containers/tree/v0.43.0/containers/javascript-node-12/.devcontainer/Dockerfile
# FROM mcr.microsoft.com/vscode/devcontainers/javascript-node:0-12
FROM node:10

# The image referenced above includes a non-root user with sudo access. Add 
# the "remoteUser" property to devcontainer.json to use it. On Linux, the container 
# user's GID/UIDs will be updated to match your local UID/GID when using the image
# or dockerFile property. Update USER_UID/USER_GID below if you are using the
# dockerComposeFile property or want the image itself to start with different ID
# values. See https://aka.ms/vscode-remote/containers/non-root-user for details.
ARG USER_UID=1000
ARG USER_GID=$USER_UID
ARG USERNAME=node
ENV SHELL /bin/zsh
# [Optional] Update UID/GID if needed
RUN if [ "$USER_GID" != "1000" ] || [ "$USER_UID" != "1000" ]; then \
    groupmod --gid $USER_GID $USERNAME \
    && usermod --uid $USER_UID --gid $USER_GID $USERNAME \
    && echo "IF----IF----IF----IF----IF----IF----IF----IF----IF----IF----IF----IF----IF----IF----IF----IF----" \
    && chown -R $USER_UID:$USER_GID /home/$USERNAME; \
    fi

# *************************************************************
# * Uncomment this section to use RUN instructions to install *
# * any needed dependencies after executing "apt-get update". *
# * See https://docs.docker.com/engine/reference/builder/#run *
# *************************************************************
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update \
    && apt-get -y install sudo git procps nano zip htop openssh-server net-tools curl  \
    #
    # Clean up
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/* 

# adiciona o usuario ao sudoers :-D 
RUN echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME; 

ENV DEBIAN_FRONTEND=dialog
ENV SHELL /bin/zsh

# Uncomment to default to non-root user
USER $USER_UID
COPY config/dev.nginx.conf ./
COPY config/zsh-in-docker.sh /tmp
# RUN sh -c "$(wget -O- https://raw.githubusercontent.com/deluan/zsh-in-docker/master/zsh-in-docker.sh)" -- \
RUN sh -c "$(cat /tmp/zsh-in-docker.sh)" -- \
    -p "git" \
    -p https://github.com/zsh-users/zsh-autosuggestions \
    -p https://github.com/zsh-users/zsh-completions \
    -p https://github.com/zsh-users/zsh-history-substring-search \
    -p https://github.com/zsh-users/zsh-syntax-highlighting

SHELL ["/bin/zsh", "--login", "-c"]

WORKDIR /home/node

# RUN ln -s "$ZSH_CUSTOM/themes/spaceship-prompt/spaceship.zsh-theme" "/home/node/.oh-my-zsh/themes/spaceship.zsh-theme"
# RUN rm -rf /home/node/.zshrc 
# COPY config/zshrc  ./home/node/.zshrc
ENV SHELL /bin/zsh
RUN cat /etc/passwd

CMD ["zsh"]
ENTRYPOINT sysctl -p; fs.inotify.max_user_watches=524288; sysctl -p; 
