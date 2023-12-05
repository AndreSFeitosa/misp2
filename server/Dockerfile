# PHP build stage
FROM debian:bullseye-slim as php-build
ENV DEBIAN_FRONTEND noninteractive
ARG MISP_TAG
ARG PHP_VER

RUN apt-get update && apt-get install -y --no-install-recommends \
    gcc \
    make \
    libfuzzy-dev \
    ca-certificates \
    php \
    php-curl \
    php-dev \
    php-pear \
    php-mbstring \
    zip unzip \
    librdkafka-dev \
    git \
    && apt-get autoremove -y && apt-get clean -y && rm -rf /var/lib/apt/lists/*

RUN pecl channel-update pecl.php.net
RUN cp "/usr/lib/$(gcc -dumpmachine)"/libfuzzy.* /usr/lib; pecl install ssdeep && pecl install rdkafka
RUN git clone --recursive --depth=1 https://github.com/kjdev/php-ext-brotli.git && cd php-ext-brotli && phpize && ./configure && make && make install

WORKDIR /tmp
COPY --from=composer /usr/bin/composer /usr/bin/composer
ADD https://raw.githubusercontent.com/MISP/MISP/${MISP_TAG}/composer.json /tmp
RUN composer config --no-interaction allow-plugins.composer/installers true && \
    composer install && \
    composer require jakub-onderka/openid-connect-php:1.0.0-rc1

# Python build stage
FROM debian:bullseye-slim as python-build
RUN apt-get update && apt-get install -y --no-install-recommends \
    gcc \
    git \
    python3 \
    python3-dev \
    python3-pip \
    python3-setuptools \
    python3-wheel \
    python3-venv \
    libfuzzy-dev \
    libffi-dev \
    ca-certificates \
    && apt-get autoremove -y && apt-get clean -y && rm -rf /var/lib/apt/lists/*

# Criação do diretório /wheels
RUN mkdir /wheels

WORKDIR /tmp
# ... (Continue com a instalação de bibliotecas Python e outras operações necessárias)

# Final stage
FROM debian:bullseye-slim
ENV DEBIAN_FRONTEND noninteractive
ARG MISP_TAG
ARG PHP_VER

# OS Packages
# ... (mantenha as instalações de pacotes OS existentes)

# MISP code
# ... (mantenha as instalações de código MISP existentes)

# Python Modules
COPY --from=python-build /wheels /wheels
RUN pip3 install --no-cache-dir /wheels/*.whl && rm -rf /wheels

# PHP
# ... (mantenha as instalações de extensões PHP existentes)

# nginx
# ... (mantenha a configuração nginx existente)

# Make a copy of the file store, so we can sync from it
# ... (mantenha os comandos existentes)

# Change Workdirectory
WORKDIR /var/www/MISP

# Set environment variable
ENV MINHA_VARIAVEL_DE_AMBIENTE=valor_padrao
