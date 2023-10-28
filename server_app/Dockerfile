# Use the official PHP image as the base image
FROM php:8.2-apache

RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Set working directory
WORKDIR /var/www/html

# Copy project files to working directory
COPY . .

# Install project dependencies using Composer
RUN composer install --optimize-autoloader --no-dev

# Set permissions for storage and bootstrap cache
RUN chown -R www-data:www-data storage bootstrap/cache

# Generate Laravel application key
RUN php artisan key:generate

# Install Node.js and npm
RUN curl -sL https://deb.nodesource.com/setup_16.x | bash -
RUN apt-get install -y nodejs

# Install project dependencies using npm
RUN npm install --global npm@latest
RUN npm install

# Build assets using Laravel Mix
RUN npm run production

# Expose port 80
EXPOSE 80

# Start Apache server
CMD ["apache2-foreground"]