FROM phusion/baseimage:latest
MAINTAINER helbing <helbingxxx@gmail.com>

# 切换sh脚本指向，从dash修改为bash
RUN rm /bin/sh && ln -s /bin/bash /bin/sh

# 设置镜像源
ARG APT_REPOSITORY=false
ENV APT_REPOSITORY ${APT_REPOSITORY}

RUN if [ ${APT_REPOSITORY} != false ]; then \

    cp /etc/apt/sources.list /etc/apt/sources.list.bak && \
    sed -i "s%archive.ubuntu.com%$APT_REPOSITORY%g" /etc/apt/sources.list \
;fi

# 加入一些镜像源
RUN echo 'deb http://mirrors.163.com/ubuntu/ xenial main restricted universe multiverse' >> /etc/apt/sources.list && \
    apt-get update

# 安装一些包
RUN apt-get install -y git apt-utils

# 设置时区
ARG TIME_ZONE=false
ENV TIME_ZONE ${TIME_ZONE}

RUN if [ ${TIME_ZONE} != false ]; then \
    ln -snf /usr/share/zoneinfo/$TIME_ZONE /etc/localtime && \
    echo $TIME_ZONETZ > /etc/timezone \
;fi

# 安装nodejs
ARG NODE_VERSION=false
ENV NODE_VERSION ${NODE_VERSION}

ARG NODE_REPOSITORY=false
ENV NODE_REPOSITORY ${NODE_REPOSITORY}

ARG YARN_REPOSITORY=false
ENV YARN_REPOSITORY ${YARN_REPOSITORY}

RUN if [ ${NODE_VERSION} != false ]; then \

    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.6/install.sh | bash && \

    if [ ${NODE_REPOSITORY} != false ]; then \

        export NVM_NODEJS_ORG_MIRROR=$NODE_REPOSITORY && \
        source $HOME/.nvm/nvm.sh; \
    fi && \

    nvm install $NODE_VERSION && \
    nvm use $NODE_VERSION && \
    nvm alias $NODE_VERSION && \

    # 安装yarn
    curl -o- -L https://yarnpkg.com/install.sh | bash && \
    export PATH="$HOME/.yarn/bin:$PATH" && \

    # 设置yarn源
    if [ ${YARN_REPOSITORY} != false ]; then \

        yarn config set registry $YARN_REPOSITORY; \
    fi && \

    yarn global add gulp webpack vue-cli \
;fi

# 安装composer
ARG COMPOSER_ENABLE=false
ENV COMPOSER_ENABLE ${COMPOSER_ENABLE}

ARG COMPOSER_REPOSITORY=false
ENV COMPOSER_REPOSITORY ${COMPOSER_REPOSITORY}

RUN if [ ${COMPOSER_ENABLE} = true ]; then \

    apt-get update && apt-get install -y php7.0 php7.0-mbstring php7.0-dom php7.0-zip && \
    curl -sS https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer && \

    if [ ${COMPOSER_REPOSITORY} != false ]; then \

        composer config -g repo.packagist composer $COMPOSER_REPOSITORY; \
    fi \
;fi

CMD ["/bin/bash"]
