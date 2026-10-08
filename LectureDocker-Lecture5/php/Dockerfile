FROM php:7.3-apache
COPY ./php.ini /usr/local/etc/php/

# =================================================================
# 【重要】古いDebianの参照先をアーカイブサーバーに切り替える処理
# =================================================================
RUN sed -i 's/deb.debian.org/archive.debian.org/g' /etc/apt/sources.list && \
    sed -i 's/security.debian.org/archive.debian.org/g' /etc/apt/sources.list && \
    sed -i '/stretch-updates/d' /etc/apt/sources.list && \
    sed -i '/bullseye-updates/d' /etc/apt/sources.list

# 一度だけアップデートを行い、必要なパッケージをまとめてインストールする
RUN apt-get update && apt-get install -y --no-install-recommends \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    vim \
    default-mysql-client \
    openssh-server \
    libmemcached-dev \
    zlib1g-dev \
    && rm -rf /var/lib/apt/lists/*

# PHPの拡張機能をまとめてインストール・有効化
RUN docker-php-ext-install pdo pdo_mysql mysqli mbstring gd iconv

# Memcached 拡張のインストール
RUN pecl install memcached \
    && docker-php-ext-enable memcached

# Apacheの設定
COPY ./php_apache2.conf /etc/apache2/mods-enabled
RUN apache2ctl restart

