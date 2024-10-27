# # Base image
# FROM node:14-alpine

# # Set working directory
# WORKDIR /home/vagrant/YoloApp/

# # Copy package.json and package-lock.json
# COPY package*.json /home/vagrant/YoloApp/

# # Install dependencies
# RUN npm install --silent

# # Copy the remaining backend files
# COPY . /home/vagrant/YoloApp/

# EXPOSE 3000
# # Start the app
# CMD ["npm", "start"]


# Build stage
FROM node:16-alpine3.16 as build-stage

# Set the working directory inside the container
WORKDIR /home/vagrant/YoloApp/

# Copy package.json and package-lock.json
COPY package*.json /home/vagrant/YoloApp/

# Install dependencies and clears the npm cache and removes any temporary files
RUN npm install --only=production && \
    npm cache clean --force && \
    rm -rf /tmp/*

# Copy the rest of the application code
COPY . /home/vagrant/YoloApp/

# Build the application and  remove development dependencies
RUN npm run build && \
    npm prune --production


EXPOSE 3000
# Start the app
CMD ["npm", "start"]