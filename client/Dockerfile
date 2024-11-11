# Build stage
FROM node:16-alpine3.16 as build-stage

# Set the working directory inside the container
# WORKDIR /home/vagrant/YoloApp/
WORKDIR /client

# Copy package.json and package-lock.json
COPY package*.json ./

# Install all dependencies (for both development and production)
RUN npm install --only=production && \
    npm cache clean --force && \
    rm -rf /tmp/*

# Copy the rest of the application code
COPY . .

# Build the application (this generates the static files)
RUN npm run build

# Production stage
FROM nginx:alpine as production-stage

# Copy the built static files from the build-stage to Nginx's HTML directory
COPY --from=build-stage /client /usr/share/nginx/html

# Expose port 80 for the Nginx server
EXPOSE 80

# Start Nginx to serve the static files
CMD ["nginx", "-g", "daemon off;"]
