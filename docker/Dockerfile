FROM composer/composer:latest-bin AS composer
FROM php:8.3-apache

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN apt-get update \
	&& apt-get install -y --no-install-recommends wget unzip \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/* \
	&& mkdir -p /workspace \
	&& wget "https://github.com/DenverCoder1/github-readme-streak-stats/archive/refs/heads/main.zip" -O /workspace/main.zip
WORKDIR /workspace
RUN unzip main.zip \
	&& rm -f main.zip

# Install system dependencies and PHP extensions in one layer
RUN apt-get update && apt-get install -y --no-install-recommends \
	git \
	libicu-dev \
	inkscape \
	fonts-dejavu-core \
	curl \
	&& docker-php-ext-configure intl \
	&& docker-php-ext-install intl \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/*

# Install Composer
COPY --from=composer /composer /usr/bin/composer

# Set working directory
WORKDIR /var/www/html

# Copy composer files and install dependencies
RUN mv /workspace/github-readme-streak-stats-main/composer.json /workspace/github-readme-streak-stats-main/composer.lock ./ \
	&& mv /workspace/github-readme-streak-stats-main/src ./ \
	&& rm -rf /workspace \
	&& composer install --no-dev --optimize-autoloader --no-scripts \
	&& composer clear-cache

# Configure Apache to serve from src/ directory and pass environment variables
RUN a2enmod rewrite headers
COPY docker/rootfs /

# Set secure permissions
RUN chown -R www-data:www-data /var/www/html && \
	find /var/www/html -type d -exec chmod 755 {} \; && \
	find /var/www/html -type f -exec chmod 644 {} \;

# Expose port
EXPOSE 80

# Start Apache
CMD ["apache2-foreground"]
