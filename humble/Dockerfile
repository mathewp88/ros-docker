FROM osrf/ros:humble-simulation

ENV NVIDIA_DRIVER_CAPABILITIES=all \
    QT_X11_NO_MITSHM=1 \
    DEBIAN_FRONTEND=noninteractive

WORKDIR /root/humble

# Install dependencies and tools
RUN apt update -y && \
    apt install -y \
        curl \
        wget \
        ripgrep \
        tmux \
        unzip \
        zsh \
        fzf \
        wl-clipboard \
        python3-tk \
        python3-pip \
        python3-venv \
        git libnvidia-gl-580

RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash && \
    export NVM_DIR="$HOME/.nvm" && \
    [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" && \
    [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" && \
    nvm install node

RUN git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y

RUN . "$HOME/.cargo/env" && \
    cargo install eza && \
    cargo install zoxide --locked 

# Copy tmux.conf and .zshrc into container
COPY tmux.conf /root/.tmux.conf
COPY .zshrc /root/.zshrc
COPY .p10k.zsh /root/.p10k.zsh

# Install tmux plugins
RUN ~/.tmux/plugins/tpm/scripts/install_plugins.sh

RUN git clone https://github.com/neovim/neovim /tmp/neovim

RUN cd /tmp/neovim && git checkout stable && make CMAKE_BUILD_TYPE=RelWithDebInfo

RUN cd /tmp/neovim && sudo make install

RUN rm -rf /tmp/neovim

# Set default shell to zsh
RUN chsh -s /bin/zsh

RUN rm -rf /var/lib/apt/lists/*

SHELL ["/bin/zsh", "-c"]
CMD ["zsh"]
