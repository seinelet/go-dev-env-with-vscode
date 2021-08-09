# [Choice] Go version: Choice the install Go version in see https://hub.docker.com/_/golang?tab=tags&page=1&ordering=last_updated
ARG VARIANT="1.16.6-stretch"
FROM golang:${VARIANT}

# [Optional] Uncomment the following line and install nodejs if needed.
ARG NODE_VERSION="lts"
RUN apt-get update && apt-get upgrade -y \
    &&curl -fsSL https://deb.nodesource.com/setup_${NODE_VERSION}.x | bash - \
    &&apt-get install -y nodejs sudo vim \
    &&apt-get clean

# [Optional] Uncomment this line to install global node packages.
# RUN npm install -g <your-package-here>

# [Optional] Uncomment the next line to install az command line tools.
RUN curl -sL https://aka.ms/InstallAzureCLIDeb | bash

# [Input] Input the using username 
ARG USER_NAME="vscode"
RUN useradd -m ${USER_NAME} -s /bin/bash \
    && echo "${USER_NAME}:${USER_NAME}" | chpasswd \
    && echo "${USER_NAME} ALL=NOPASSWD: ALL" >> /etc/sudoers
USER ${USER_NAME}
# [Input] Enter where to put .bashrc
ARG BASHRC_PATH="/home/${USER_NAME}"
COPY "./.bashrc" "$BASHRC_PATH"

WORKDIR /workspace
CMD sleep infinity

# Copy wait-for-it.sh
COPY ./wait-for-it.sh /workspace/

# [Optional] Uncomment the next line to use go install to install anything else you need.
# go install golang.org/x/tools/gopls

# [Optional] Install gopls & dlv on befor golang version 1.15 
# RUN go get golang.org/x/tools/gopls \
#     && go get github.com/go-delve/delve/cmd/dlv

# [Optional] Install gopls & dlv on golang version 1.16 or later 
RUN go install golang.org/x/tools/gopls@latest \
    && go install github.com/go-delve/delve/cmd/dlv@latest

# [Optional] Uncomment the next line to use mysql client
# RUN apt install -y mysql-client