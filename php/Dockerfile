# Stage 1: PHP dependencies with Composer
FROM composer:2 AS composer

WORKDIR /app

COPY composer.json composer.lock ./
RUN composer install --no-dev --prefer-dist --no-scripts

COPY . .

RUN cp .env.example .env
RUN composer dump-autoload --optimize
RUN php artisan config:clear && php artisan key:generate

# Stage 2: Frontend assets with Node (optional, for Vite or Laravel Mix)
FROM node:20-alpine AS frontend

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm install

COPY resources ./resources
COPY vite.config.js ./

RUN npm run build

# Stage 3: Production image
FROM php:8.2-fpm-alpine

# Set working directory
WORKDIR /var/www/html

# Copy application and built assets
COPY --from=composer /app ./
COPY --from=frontend /app/public/build ./public/build

# Set file permissions
RUN chown -R www-data:www-data . && chmod -R 755 .

EXPOSE 8000

CMD ["php", "artisan", "serve", "--host=0.0.0.0", "--port=8000"]