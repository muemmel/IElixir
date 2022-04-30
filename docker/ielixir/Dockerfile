FROM jupyter/base-notebook

USER root

ENV WORK_DIR=/opt \
    ELIXIR_VERSION=1.12.2-1 \
    NOTEBOOK_ARGS=--notebook-dir=/home/jovyan/work

RUN set -xe \
  && apt-get update \
  && apt install -y --no-install-recommends \
  git \
  wget \
  autoconf \
  g++ \
  gcc \
  make \
  libzmq3-dev \
  libsqlite3-dev \
  gnupg2

RUN wget https://packages.erlang-solutions.com/erlang-solutions_2.0_all.deb \
  && dpkg -i erlang-solutions_2.0_all.deb

RUN set -xe \
  && apt update \
  && apt install -f -y --no-install-recommends \
  elixir=$ELIXIR_VERSION \
  erlang-dev \
  erlang-parsetools

USER $NB_UID

RUN cd $HOME \
  && git clone https://github.com/awill1988/IElixir.git \
  && cd IElixir \
  && git switch feature/elixir_1_12_otp_24_support \
  && mix local.hex --force \
  && mix local.rebar --force \
  && mix deps.get \
  && mix deps.compile \
  && ./install_script.sh

CMD ["start-notebook.sh"]