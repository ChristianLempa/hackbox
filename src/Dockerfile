######### Config Arguments ###########
ARG NERD_FONTS_ARCHIVE="Hack.zip"
ARG NERD_FONTS_VERSION="v2.1.0"


######### Temporary Files Layer ###########
FROM busybox:1.36.1-glibc AS temp-files

# Make config arguments from above known in this layer
ARG NERD_FONTS_ARCHIVE
ARG NERD_FONTS_VERSION

# Local files
COPY assets/mr-robot-wallpaper.png  /copy/configs/usr/share/extra/backgrounds/bg_default.png
COPY config/starship.toml /copy/configs/root/.config/starship.toml
COPY config/terminator.toml /copy/configs/root/.config/terminator/config

# Nerd fonts (remote)
ADD https://github.com/ryanoasis/nerd-fonts/releases/download/${NERD_FONTS_VERSION}/${NERD_FONTS_ARCHIVE} /tmp/${NERD_FONTS_ARCHIVE}
RUN mkdir -p /copy/configs/usr/local/share/fonts
RUN unzip /tmp/${NERD_FONTS_ARCHIVE} -d /copy/configs/usr/local/share/fonts

# Starship (remote)
ADD https://starship.rs/install.sh /copy/starship/install.sh
RUN chmod +x /copy/starship/install.sh


######### KASM Container Here ###########

FROM kasmweb/core-ubuntu-bionic:1.10.0
USER root

ENV HOME /home/kasm-default-profile
ENV STARTUPDIR /dockerstartup
ENV INST_SCRIPTS $STARTUPDIR/install
WORKDIR $HOME


######### Customize Container Here ###########

# Install packages and configure the container
RUN --mount=type=bind,from=temp-files,source=/copy,target=/tmp/copy \
    # Make APT/DPKG aware that we're building an image, so no user interaction is possible
    # (only for this specific shell)
    export DEBIAN_FRONTEND=noninteractive && \
    # Fix locales when installing packages
    # (only for this specific shell)
    export LC_ALL=C \
    && \
        # Upgrade the system
        apt-get update && \
        apt-get -y full-upgrade \
    && \
        # Install packages
        apt-get -y install --no-install-recommends \
            numix-gtk-theme \
            openvpn \
            terminator \
    && \
        # Clear APT caches
        apt-get -y clean && \
        rm -rf /var/lib/apt/lists/* \
    && \
        # Copy config files from mounted /tmp/copy/configs directory
        mkdir -p root/.config/terminator && \
        cp -R /tmp/copy/configs/* / && \
        # Install Starship
        /tmp/copy/starship/install.sh -y && \
        # Configure Bash for Starship
        echo 'eval "$(starship init bash)"' >> .bashrc


######### End Customizations ###########

RUN chown 1000:0 $HOME
RUN $STARTUPDIR/set_user_permission.sh $HOME

ENV HOME /home/kasm-user
WORKDIR $HOME
RUN mkdir -p $HOME && chown -R 1000:0 $HOME
