FROM debian:bullseye

ENV NODE_VERSION='lts/*' \
    USERNAME=node \
    NVM_V="v0.39.2" \
    NVM_DIR=/opt/nvm \
    NVM_SYMLINK_CURRENT=true \
    PATH=${NVM_DIR}/current/bin:${PATH} \
    GIT_DELTA_V="0.14.0"

RUN sed -i '/^#/ ! s/$/ contrib non-free/' /etc/apt/sources.list \
  && export DEBIAN_FRONTEND=noninteractive \
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
  # node-gyp dependencies
  && apt-get -y install --no-install-recommends \
        make \
        gcc \
        g++ \
        python3-minimal \
        python-is-python3 \
  && echo "${USERNAME} ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/${USERNAME} \
  && chmod 440 /etc/sudoers.d/${USERNAME} \
  && echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
  && locale-gen \
#   && echo "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Yarn setup ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~" \
#   && curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | gpg --dearmor > /usr/share/keyrings/yarn-archive-keyring.gpg \
#   && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/yarn-archive-keyring.gpg] https://dl.yarnpkg.com/debian/ stable main" > /etc/apt/sources.list.d/yarn.list \
#   && apt-get -yqq update \
#   && apt-get -y install --no-install-recommends yarn \
  && echo "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Adding Node.js user ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~" \
  && groupadd -g 1000 ${USERNAME} \
  && useradd -m -u 1000 -g 1000 -s /bin/bash ${USERNAME} \
  && echo "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ NVM setup ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~" \
  && groupadd -r nvm \
  && umask 0002 \
  && usermod -aG nvm ${USERNAME} \
  && mkdir -p ${NVM_DIR} \
  && chown :nvm ${NVM_DIR} \
  && chmod g+s ${NVM_DIR} \
  && git clone https://github.com/nvm-sh/nvm.git ${NVM_DIR} \
  && cd ${NVM_DIR} \
  && git checkout ${NVM_V} \
  && . ${NVM_DIR}/nvm.sh \
  && echo "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Installing $NODE_VERSION using NVM ~~~~~~~~~~~~~~~~~~~~~~~" \
  && su ${USERNAME} -c ". ${NVM_DIR}/nvm.sh && nvm install ${NODE_VERSION} --latest-npm && nvm clear-cache" \
  && export PATH=${NVM_DIR}/current/bin:${PATH} \
  && echo "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Installing global packages ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~" \
  # umask 0002 https://github.com/npm/npm/issues/1509
  && su ${USERNAME} -c "umask 0002 ; npm install -g npm pnpm yarn eslint typescript" \
  && npm cache clean --force > /dev/null 2>&1 \
  && echo "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ git delta ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~" \
  && curl -Ls https://github.com/dandavison/delta/releases/download/${GIT_DELTA_V}/git-delta_${GIT_DELTA_V}_amd64.deb -o git-delta.deb \
  && dpkg -i git-delta.deb ; rm -rf git-delta.deb \
  && echo "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Cleanup ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~" \
  && apt-get clean all \
  && apt-get autoremove -y \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
  && echo "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ Oh-My-ZSH ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~" \
  && git clone --depth=1 "https://github.com/ohmyzsh/ohmyzsh.git" /root/.oh-my-zsh \
  && cp -Ra /root/.oh-my-zsh /home/node/.oh-my-zsh

COPY zshrc /root/.zshrc
COPY vimrc /root/.vimrc

RUN cp /root/.zshrc /root/.vimrc /home/${USERNAME}/ \
  && chmod 640 /root/.zshrc /root/.vimrc /home/${USERNAME}/.zshrc /home/${USERNAME}/.vimrc \
  && chown -R ${USERNAME}:${USERNAME} /home/${USERNAME} \
  && cd /home/${USERNAME}/.oh-my-zsh ; git repack -a -d -f --depth=1 --window=1 \
  && cd /root/.oh-my-zsh ; git repack -a -d -f --depth=1 --window=1 \
  && usermod -s /bin/zsh ${USERNAME} \
  && usermod -s /bin/zsh root

WORKDIR /root
