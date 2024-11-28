# Stage 1: Build the Node.js frontend application
FROM node:20.18-alpine3.19 AS builder

# Set working directory inside the container
WORKDIR /app

# Copy package.json and package-lock.json files for dependency installation
COPY package*.json ./
RUN npm cache clean --force
# Install dependencies
RUN npm install

# Copy the rest of the application's code
COPY . .

# Build the application (output will go to the 'build' directory for React)
RUN npm run build

# Stage 2: Set up Nginx to serve the static files
FROM nginx:1.10.1-alpine

# Copy a custom Nginx configuration file for reverse proxy
COPY nginx.conf /etc/nginx/nginx.conf
COPY /home/ubuntu/nginx-cert/libertini.crt /etc/letsencrypt/live/libertini.crt
COPY /home/ubuntu/nginx-cert/libertini.key /etc/letsencrypt/live/libertini.key

# Copy built application from the previous stage to Nginx html directory
COPY --from=builder /app/dist /usr/share/nginx/html

# Expose port 80 to be accessible outside of the container
EXPOSE 80
EXPOSE 443

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
