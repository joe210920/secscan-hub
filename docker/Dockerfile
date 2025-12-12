# 1. 基礎映像（PHP-FPM）
FROM php:8.3-fpm

# 2. 安裝系統依賴（圖片處理、壓縮等）
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    zip \
    unzip \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libmagickwand-dev \
    libicu-dev \
    zlib1g-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# 3. 安裝 PHP 擴展（包含 redis）
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install \
    pdo_mysql \
    mbstring \
    exif \
    pcntl \
    bcmath \
    gd \
    intl \
    zip \
    opcache \
    sockets \
    fileinfo \
    && pecl install redis \
    && docker-php-ext-enable redis

# 4. 安裝 Composer（套件管理）
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# 5. 設定工作目錄
WORKDIR /var/www/html

# 6. 複製專案檔案 
COPY . /var/www/html

# 7. 權限設定
RUN mkdir -p storage/framework/views storage/logs logs \
    && chown -R www-data:www-data \
    /var/www/html \
    /var/www/html/storage \
    /var/www/html/bootstrap/cache \
    /var/www/html/logs \
    && chmod -R 775 \
    /var/www/html/storage \
    /var/www/html/bootstrap/cache \
    /var/www/html/logs \
    && chown -R www-data:www-data logs \
    && chmod -R 777 logs

# 9. 暴露 PHP-FPM 埠號（給 Nginx 用）
EXPOSE 9000

# 10. 啟動 PHP-FPM
CMD ["php-fpm"]
