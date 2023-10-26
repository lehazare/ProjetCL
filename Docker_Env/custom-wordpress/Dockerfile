# Use the official WordPress image as the base image
FROM wordpress:latest

# Install system dependencies and Imagick
RUN apt-get update && apt-get install -y imagemagick

# Start the WordPress entrypoint
CMD ["apache2-foreground"]