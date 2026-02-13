# Use nginx base image
FROM nginx:alpine

# Copy nginx configuration
COPY nginx.conf /etc/nginx/nginx.conf

# Copy frontend files to nginx html directory
COPY . /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start nginx
CMD ["nginx", "-g", "daemon off;"]