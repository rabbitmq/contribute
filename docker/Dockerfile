FROM ubuntu:22.04

# essentials
RUN apt-get autoclean -y
RUN apt-get autoremove -y
RUN apt update -y && apt install -y apt-transport-https curl git software-properties-common build-essential rsync emacs-nox vim python3 zip gnupg

# Modern Erlang and Elixir
RUN add-apt-repository ppa:rabbitmq/rabbitmq-erlang
RUN apt install -y elixir erlang-dev erlang-eunit erlang-common-test erlang-dialyzer erlang-debugger erlang-parsetools erlang-runtime-tools erlang-os-mon erlang-ssl

# Packaging
RUN apt install -y make nsis tofrodos mandoc bsdmainutils

RUN curl -L --output /usr/local/bin/bazel https://github.com/bazelbuild/bazelisk/releases/download/v1.15.0/bazelisk-linux-amd64
RUN chmod +x /usr/local/bin/bazel

# Contributor user
RUN useradd -ms /bin/bash contributor

USER contributor
WORKDIR /home/contributor

RUN echo <<-EOF \
export RABBITMQ_PACKAGING_REPO=/home/contributor/packaging \
EOF > /home/contributor/.bashrc

RUN git clone https://github.com/rabbitmq/rabbitmq-server.git server
RUN git clone https://github.com/rabbitmq/rabbitmq-packaging.git packaging
