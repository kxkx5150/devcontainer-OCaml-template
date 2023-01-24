FROM mcr.microsoft.com/vscode/devcontainers/base:ubuntu-22.04

ARG USERNAME=vscode
ENV DEBIAN_FRONTEND noninteractive

ENV DISPLAY :0
ENV LANG=en_US.UTF-8 \
    LANGUAGE=en_US.UTF-8 \
    LC_ALL=C.UTF-8 


ARG OCAML_VERSION=4.14.0
ENV OPAM_VERSION  2.1.4
ENV OCAML_VERSION $OCAML_VERSION

# C++
# python
# node
# SDL2
# OpenGL
# Qt5
# GTK4
RUN apt-get update && apt-get install -y --no-install-recommends build-essential manpages-dev \
    git unzip dosfstools curl wget vim sudo ssh tzdata ca-certificates \
    dbus dbus-x11 alsa-utils pulseaudio pulseaudio-utils mesa-utils x11-apps \
    # cmake clang clang-format clangd gdb nasm lldb\ 
    # python3 python-is-python3 python3-distutils python3-numpy python3-pip python3-tk \
    # nodejs \
    # libsdl2-dev libsdl2-image-dev libsdl2-mixer-dev libsdl2-net-dev libsdl2-ttf-dev \
    # libglu1-mesa-dev freeglut3-dev mesa-common-dev libglew-dev libglfw3-dev \
    # qtcreator qtbase5-dev qttools5-dev-tools qtbase5-examples qt5-qmake \
    # libgtk-4-bin libgtk-4-common libgtk-4-dev \
    && apt-get clean -y && rm -rf /var/lib/apt/lists

# Install OPAM
RUN curl -L -o /usr/bin/opam "https://github.com/ocaml/opam/releases/download/$OPAM_VERSION/opam-$OPAM_VERSION-x86_64-Linux" && \
    chmod 755 /usr/bin/opam



# && \
#     find /root/.opam -regex '.*\.\(cmt\|cmti\|annot\|byte\)' -delete && \
#     rm -rf /root/.opam/archives \
#     /root/.opam/repo/default/archives \
#     /root/.opam/$OCAML_VERSION/man \
#     /root/.opam/$OCAML_VERSION/build

USER ${USERNAME}

# OCaml
RUN opam init -a -y --comp $OCAML_VERSION --disable-sandboxing
RUN opam update && \
    opam install -y \
    ocaml-lsp-server \
    dune \
    utop \
    mparser \
    ocamlformat \
    ounit2 \
    qcheck \
    merlin \
    ocamlformat-rpc

# Install Rust
# ENV RUST_VERSION stable
# RUN curl https://sh.rustup.rs -sSf | sh -s -- -y --default-toolchain ${RUST_VERSION}
# ENV PATH $PATH:/home/${USERNAME}/.cargo/bin
# RUN rustup install stable
# RUN rustup install nightly
# RUN rustup component add rust-analysis rust-src rustfmt clippy
# RUN cargo install cargo-watch
# RUN cargo install cargo-check

RUN mkdir -p /home/${USERNAME}/workspace
WORKDIR /home/${USERNAME}/workspace

USER root
COPY set_uid.sh /home/
ENTRYPOINT ["/home/set_uid.sh"]
COPY ./.inject_bashrc /root
RUN cat /root/.inject_bashrc >> /home/${USERNAME}/.bashrc
CMD ["/bin/sh", "-c", "echo Container started ; trap \"exit 0\" 15; while sleep 1 & wait $!; do :; done"]
