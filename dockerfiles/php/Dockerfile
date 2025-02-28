# Используем официальный образ с PHP 8.1
FROM php:8.1.9-fpm

# Устанавливаем системные зависимости
RUN apt-get update && apt-get install -y \
    libzip-dev \
    unzip \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libxml2-dev \
    libonig-dev \
    libcurl4-openssl-dev \
    libgmp-dev \
    libmagickwand-dev \
    git \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd \
    && docker-php-ext-install zip \
    && docker-php-ext-install pdo pdo_mysql \
    && docker-php-ext-install bcmath \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install exif \
    && docker-php-ext-install intl \
    && docker-php-ext-install curl \
    && docker-php-ext-install gmp \
    && pecl install imagick \
    && docker-php-ext-enable imagick \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Устанавливаем Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Устанавливаем настройки PHP
COPY php.ini /usr/local/etc/php/php.ini



# Создание пользователя и группы
RUN groupadd -g 1000 dvoreckii && \
    useradd -u 1000 -g dvoreckii -m dvoreckii && \
    chown -R dvoreckii:dvoreckii /var/www

# Копирование файлов в контейнер
COPY . src

WORKDIR /var/www

# Переключение на созданного пользователя
USER dvoreckii

# Установка прав на папку с приложением
#RUN sudo chown -R dvoreckii:dvoreckii /var/www


## Устанавливаем рабочую директорию
#WORKDIR /var/www
#
## Копируем код приложения
#COPY . src

# Открываем порт 9000 для PHP-FPM
EXPOSE 9000

# Задаем команду запуска
CMD ["php-fpm"]