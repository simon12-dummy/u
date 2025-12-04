ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=1000
FROM archlinux:latest

# Keep environment minimal and non-interactive during build
ENV TERM=xterm-256color
ENV LANG=en_US.UTF-8
ENV LC_ALL=en_US.UTF-8

# Update keyring, install common dev tools, clean cache
RUN pacman -Syu --noconfirm \
  && pacman -S --noconfirm archlinux-keyring \
  && pacman-key --init \
  && pacman-key --populate archlinux \
  && pacman -S --noconfirm --needed \
    sudo \
    git \
    base-devel \
    openssh \
    curl \
    wget \
    zsh \
    neovim \
    python \
    python-pip \
    nodejs \
    npm \
    unzip \
    ca-certificates \
  && pacman -Scc --noconfirm

# Create non-root user (vscode) with sudo (NOPASSWD)
RUN groupadd --gid ${USER_GID} ${USERNAME} 2>/dev/null || true \
  && useradd -m --uid ${USER_UID} --gid ${USER_GID} -s /bin/bash ${USERNAME} 2>/dev/null || true \
  && echo "${USERNAME} ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/${USERNAME} \
  && chmod 0440 /etc/sudoers.d/${USERNAME} \
  && chsh -s /bin/zsh ${USERNAME} || true

# Set up locale (best-effort)
RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && locale-gen || true

# Switch to non-root user for the container runtime
USER ${USERNAME}
WORKDIR /home/${USERNAME}

# Default command
CMD [ "sleep", "infinity" ]
