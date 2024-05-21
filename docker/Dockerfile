FROM ubuntu:24.04

#ARG eksctl_version="v0.109.0"
ARG eksctl_version="v0.177.0"

#kubectl download url

#ARG kubectl_dl_url="https://s3.us-west-2.amazonaws.com/amazon-eks/1.23.7/2022-06-29/bin/linux/amd64/kubectl"
ARG kubectl_dl_url="https://s3.us-west-2.amazonaws.com/amazon-eks/1.29.0/2024-01-04/bin/linux/amd64/kubectl"


ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get -y install ca-certificates curl fish vim zip pipx git && \
    install -m 0755 -d /etc/apt/keyrings && \
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc && \
    chmod a+r /etc/apt/keyrings/docker.asc && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu noble stable" | \
    tee /etc/apt/sources.list.d/docker.list > /dev/null && \
    apt-get update && \
    apt-get -y install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Install helm
RUN curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

# Install dependent packages
#RUN apk add helm --repository=http://dl-cdn.alpinelinux.org/alpine/edge/testing
#RUN apk add helm
#RUN apk add --no-cache \
#    binutils \
#    bash \
#    fish \
#    pipx \
#    python3 \
#    curl \
#    groff \
#    jq \
#    ncurses \
#    vim \
#    less \
#    docker-cli \
#    openssh \
#    git \
#    gcompat


#    py3-pip \

# Install glibc for awscli2
#RUN curl -sL https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub -o /etc/apk/keys/sgerrand.rsa.pub && \
#    curl -sLO https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.33-r0/glibc-2.33-r0.apk && \
#    curl -sLO https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.33-r0/glibc-bin-2.33-r0.apk && \
#    apk add --no-cache glibc-2.33-r0.apk glibc-bin-2.33-r0.apk

# Install awscli2
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" && \
    unzip awscliv2.zip && \
    ./aws/install

# Install eksctl
RUN curl --location "https://github.com/weaveworks/eksctl/releases/download/${eksctl_version}/eksctl_Linux_amd64.tar.gz" | tar xz -C /usr/local/bin

# Install kubectl
RUN curl -o /usr/local/bin/kubectl ${kubectl_dl_url} && \
    chmod +x /usr/local/bin/kubectl

# Install krew
RUN cd "$(mktemp -d)" && \
    OS="$(uname | tr '[:upper:]' '[:lower:]')" && \
    ARCH="$(uname -m | sed -e 's/x86_64/amd64/' -e 's/\(arm\)\(64\)\?.*/\1\2/' -e 's/aarch64$/arm64/')" && \
    KREW="krew-${OS}_${ARCH}" && \
    curl -fsSLO "https://github.com/kubernetes-sigs/krew/releases/latest/download/${KREW}.tar.gz" && \
    tar zxvf "${KREW}.tar.gz" && \
    ./"${KREW}" install krew

# Install awslogs
RUN pipx install awslogs

# Install kustomize
RUN curl -s "https://raw.githubusercontent.com/kubernetes-sigs/kustomize/master/hack/install_kustomize.sh" | bash && \
    mv /kustomize /usr/local/bin/

# Install helm diff
RUN helm plugin install https://github.com/databus23/helm-diff

# Install pluto
RUN curl -fsSLO "https://github.com/FairwindsOps/pluto/releases/download/v5.19.4/pluto_5.19.4_linux_amd64.tar.gz" && \
    tar zxvf "pluto_5.19.4_linux_amd64.tar.gz" && \
    mv /pluto /usr/local/bin/


#RUN mkdir -p $HOME/.config/fish && \
#    bash -c 'echo fish_vi_key_bindings  >> $HOME/.config/fish/config.fish'

WORKDIR /root
COPY ./update_config /usr/local/bin/

ENV PS1="[\u@\h:\w]$ "
