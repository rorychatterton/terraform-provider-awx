FROM docker.pkg.github.com/nolte/vscode-devcontainers/k8s-operator:latest

# https://github.com/bflad/tfproviderdocs/releases
ENV TFPROVIDERDOCS_VERSION=0.7.0

# https://github.com/bats-core/bats-core
ENV BATS_VERSION=1.2.0

USER root

COPY --from=docker.pkg.github.com/nolte/vscode-devcontainers/devops:latest /usr/local/bin/swagger /usr/local/bin/swagger
COPY --from=docker.pkg.github.com/nolte/vscode-devcontainers/devops:latest /usr/local/bin/terraform /usr/local/bin/terraform
COPY --from=docker.pkg.github.com/nolte/vscode-devcontainers/devops:latest /usr/local/bin/terraform-doc  /usr/local/bin/terraform-doc
COPY --from=docker.pkg.github.com/nolte/vscode-devcontainers/devops:latest /usr/local/bin/shellcheck  /usr/local/bin/shellcheck

RUN curl -sSL -k https://github.com/bflad/tfproviderdocs/releases/download/v${TFPROVIDERDOCS_VERSION}/tfproviderdocs_${TFPROVIDERDOCS_VERSION}_linux_amd64.tar.gz -o /tmp/tfproviderdocs.tgz \
    && tar -zxf /tmp/tfproviderdocs.tgz -C /tmp \
    && mv /tmp/tfproviderdocs /usr/local/bin/

RUN apk add --update-cache \
    nodejs npm \
    && rm -rf /var/cache/apk/*

RUN mkdir -p /go/src && chown -R ${USER_UID}:${USER_GID} /go/src \
    && mkdir -p /go/pkg && chown -R ${USER_UID}:${USER_GID} /go/pkg

RUN curl -sSL -k https://github.com/goreleaser/goreleaser/releases/download/v0.141.0/goreleaser_Linux_x86_64.tar.gz -o /tmp/goreleaser.tgz \
    && tar -zxf /tmp/goreleaser.tgz -C /tmp \
    && mv /tmp/goreleaser /usr/local/bin/


USER ${USERNAME}

RUN mkdir -p /home/${USERNAME}/.terraform.d/plugins/linux_amd64

ENV PATH="/usr/local/share/npm/bin:/usr/local/bin:/usr/local/sbin:~/bin:$PATH"

RUN mkdir "/home/${USERNAME}/.npm-packages"
RUN npm config set prefix "/home/${USERNAME}/.npm-packages"


RUN curl -sSfL https://raw.githubusercontent.com/golangci/golangci-lint/master/install.sh | sh -s -- -b $(go env GOPATH)/bin v1.27.0

RUN curl -sfL https://raw.githubusercontent.com/securego/gosec/master/install.sh | sh -s -- -b $(go env GOPATH)/bin v2.3.0

RUN npm i markdown-spellcheck -g

RUN curl https://raw.githubusercontent.com/nektos/act/master/install.sh | sudo bash

RUN npm i madr adr-log -g
