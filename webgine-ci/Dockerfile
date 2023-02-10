FROM php:8.1-apache

LABEL maintainer="Jonatan Moleiro <jmoleiro@gmail.com>"

ENV ACCEPT_EULA=Y

# Microsoft SQL Server Prerequisites
RUN apt-get update \
    && curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
    && curl https://packages.microsoft.com/config/debian/9/prod.list \
        > /etc/apt/sources.list.d/mssql-release.list \
    && apt-get install -y --no-install-recommends \
        locales \
        apt-transport-https \
    && echo "en_US.UTF-8 UTF-8" > /etc/locale.gen \
    && locale-gen \
    && apt-get update \
    && apt-get -y --no-install-recommends install \
        unixodbc-dev \
        msodbcsql17

# Install PHP extensions
RUN docker-php-ext-install mbstring pdo pdo_mysql pear cgi common bz2 calendar curl bcmath dom gd json libxml mysqlnd gettext fpm intl cli xml apcu bcmath opcache posix shmop sodium pecl-zendopcache zip imap mcrypt \
    && pecl install sqlsrv pdo_sqlsrv xdebug \
    && docker-php-ext-enable sqlsrv pdo_sqlsrv xdebug

RUN \
    apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y --no-install-recommends \
    apt-transport-https \
    curl \
    default-mysql-client \
    dirmngr \
    git \
    gpg \
    gpg-agent \
    make \
    openssh-client \
    rsync \
    software-properties-common \
    tini \
    unzip \
    vim \
    xz-utils \
    zip \
    zsh \
  && apt-get autoremove -y --purge \
  && apt-get autoclean -y \
  && apt-get clean -y \
  && rm -rf /var/cache/debconf/*-old \
  && rm -rf /usr/share/doc/* \
  && rm -rf /var/lib/apt/lists/* \
  && rm -rf /var/cache/apt/*

# https://getcomposer.org/download/
ARG COMPOSER_VERSION="2.4.1"
RUN \
  curl -L "https://getcomposer.org/installer" --output "composer-setup.php" \
  && php -r "if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
  && php composer-setup.php \
    --install-dir=/usr/local/bin \
    --filename=composer \
    --version=${COMPOSER_VERSION} \
  && rm "composer-setup.php"


ARG DEPLOYER_VERSION="v6.8.0"
RUN \
  curl -L "https://deployer.org/releases/${DEPLOYER_VERSION}/deployer.phar" \
      --output /usr/local/bin/dep \
  && chmod +x /usr/local/bin/dep

ARG NODE_VERSION="16"
RUN \
    curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash \
    && source ~/.profile \
    && nvm install $NODE_VERSION default \
    && npm i -g yarn

RUN \  
  apt-get autoremove -y --purge \
  && apt-get autoclean -y \
  && apt-get clean -y \
  && rm -rf /var/cache/debconf/*-old \
  && rm -rf /usr/share/doc/* \
  && rm -rf /var/lib/apt/lists/* \
  && rm -rf /var/cache/apt/*

COPY zshrc /root/.zshrc

ENTRYPOINT ["/usr/bin/tini", "--"]