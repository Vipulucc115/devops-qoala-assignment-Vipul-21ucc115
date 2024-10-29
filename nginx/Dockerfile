FROM nginx:latest

# Make sure the configuration file is spelled correctly
COPY nginx.conf /etc/nginx/nginx.conf

# Copy the HTML files to the Nginx server directory


# Expose port 80
EXPOSE 80

# Start Nginx in the foreground
CMD ["nginx", "-g", "daemon off;"]
