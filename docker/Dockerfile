FROM alpine:3.16

ARG eksctl_version="v0.100.0"

#kubectl download url

#1.23
ARG kubectl_dl_url="https://s3.us-west-2.amazonaws.com/amazon-eks/1.23.7/2022-06-29/bin/linux/amd64/kubectl"


# Install dependent packages
RUN apk add helm --repository=http://dl-cdn.alpinelinux.org/alpine/edge/testing
RUN apk add --no-cache \
    binutils \
    bash \
    fish \
    py3-pip \
    python3 \
    curl \
    groff \
    jq \
    ncurses \
    vim \
    less \
    docker-cli \
    git

# Install glibc for awscli2
RUN curl -sL https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub -o /etc/apk/keys/sgerrand.rsa.pub && \
    curl -sLO https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.33-r0/glibc-2.33-r0.apk && \
    curl -sLO https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.33-r0/glibc-bin-2.33-r0.apk && \
    apk add --no-cache glibc-2.33-r0.apk glibc-bin-2.33-r0.apk

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


#RUN mkdir -p $HOME/.config/fish && \
#    bash -c 'echo fish_vi_key_bindings  >> $HOME/.config/fish/config.fish'

WORKDIR /root

ENV PS1="[\u@\h:\w]$ "
