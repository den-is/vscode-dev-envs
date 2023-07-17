FROM python:3.11.4-bullseye

ENV GIT_DELTA_V="0.16.5"

RUN export PIPX_HOME="/usr/local/py-utils" \
  && export PIPX_BIN_DIR="${PIPX_HOME}/bin" \
  && export PATH=${PIPX_BIN_DIR}:$PATH \
  && umask 0002 \
  && mkdir -p ${PIPX_BIN_DIR} \
  && groupadd -r pipx \
  && chown :pipx ${PIPX_HOME} ${PIPX_BIN_DIR} \
  && chmod g+s ${PIPX_HOME} ${PIPX_BIN_DIR} \
  && groupadd -g 1000 vscode \
  && useradd -m -u 1000 -g 1000 -s /bin/bash vscode \
  && usermod -aG pipx vscode \
  && usermod -aG pipx root \
  && pip3 install --no-cache-dir --upgrade pip setuptools \
  && pip3 install --no-cache-dir --upgrade ipython \
  && pip3 install --no-cache-dir --upgrade jupyterlab \
  && export PYTHONUSERBASE=/tmp/pip-tmp \
  && export PIP_CACHE_DIR=/tmp/pip-tmp/cache \
  && export USE_EMOJI=0 \
  && pip3 install --disable-pip-version-check --no-warn-script-location  --no-cache-dir --user pipx \
  && /tmp/pip-tmp/bin/pipx install --pip-args=--no-cache-dir pipx \
  && /tmp/pip-tmp/bin/pipx install --system-site-packages --pip-args '--no-cache-dir --force-reinstall' ruff \
  && /tmp/pip-tmp/bin/pipx install --system-site-packages --pip-args '--no-cache-dir --force-reinstall' black \
  && /tmp/pip-tmp/bin/pipx install --system-site-packages --pip-args '--no-cache-dir --force-reinstall' mypy \
  && /tmp/pip-tmp/bin/pipx install --system-site-packages --pip-args '--no-cache-dir --force-reinstall' bandit \
  && /tmp/pip-tmp/bin/pipx install --system-site-packages --pip-args '--no-cache-dir --force-reinstall' pipenv \
  && /tmp/pip-tmp/bin/pipx install --system-site-packages --pip-args '--no-cache-dir --force-reinstall' semgrep \
  && /tmp/pip-tmp/bin/pipx install --system-site-packages --pip-args '--no-cache-dir --force-reinstall' virtualenv \
  && rm -rf /tmp/pip-tmp \
  && export DEBIAN_FRONTEND=noninteractive \
  && sed -i '/^#/ ! s/$/ contrib non-free/' /etc/apt/sources.list \
  && apt-get -yqq update \
  && apt-get -y install --no-install-recommends \
        ca-certificates \
        apt-transport-https \
        apt-utils \
  && apt-get -y install --no-install-recommends \
        jq \
        git \
        tar \
        zip \
        zsh \
        zip \
        vim \
        bat \
        exa \
        curl \
        sudo \
        less \
        lsof \
        ncdu \
        tree \
        rsync \
        unzip \
        gnupg2 \
        dialog \
        strace \
        procps \
        sqlite3 \
        dirmngr \
        fd-find \
        ripgrep \
        lsb-release \
        openssh-client \
        zsh-syntax-highlighting \
        libc6 \
        libstdc++6 \
        locales \
        tzdata \
  && echo "vscode ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/vscode \
  && chmod 440 /etc/sudoers.d/vscode \
  && echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
  && locale-gen \
  && echo "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ git delta ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~" \
  && curl -Ls https://github.com/dandavison/delta/releases/download/${GIT_DELTA_V}/git-delta-musl_${GIT_DELTA_V}_amd64.deb -o git-delta.deb \
  # GIT_DELTA_V="0.15.1" has broken install of usual binary. had to switch to musl library
  #4 122.6 The following packages have unmet dependencies:
  #4 122.6  git-delta : Depends: libc6 (>= 2.34) but 2.31-13+deb11u5 is installed
  # && curl -Ls https://github.com/dandavison/delta/releases/download/${GIT_DELTA_V}/git-delta_${GIT_DELTA_V}_amd64.deb -o git-delta.deb \
  && dpkg -i git-delta.deb ; rm -rf git-delta.deb \
  && echo "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Cleanup ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~" \
  && apt-get clean all \
  && apt-get autoremove -y \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
  && echo 'export PIPX_HOME="/usr/local/py-utils"' >> /root/.bashrc \
  && echo 'export PIPX_BIN_DIR="${PIPX_HOME}/bin"' >> /root/.bashrc \
  && echo 'export PATH=${PIPX_BIN_DIR}:$PATH' >> /root/.bashrc \
  && echo "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Oh-My-ZSH ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~" \
  && git clone --depth=1 "https://github.com/ohmyzsh/ohmyzsh.git" /root/.oh-my-zsh \
  && cp -Ra /root/.oh-my-zsh /home/vscode/.oh-my-zsh

COPY zshrc /root/.zshrc
COPY vimrc /root/.vimrc

RUN cp /root/.zshrc /root/.vimrc /home/vscode/ \
  && chmod 640 /root/.zshrc /root/.vimrc /home/vscode/.zshrc /home/vscode/.vimrc \
  && chown -R vscode:vscode /home/vscode \
  && cd /home/vscode/.oh-my-zsh ; sudo -u vscode git repack -a -d -f --depth=1 --window=1 \
  && cd /root/.oh-my-zsh ; git repack -a -d -f --depth=1 --window=1 \
  && usermod -s /bin/zsh vscode \
  && usermod -s /bin/zsh root \
  && curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash \
  && su - vscode -c "curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash"

WORKDIR /root
