# Stage 1: Build the Angular application
FROM node:25-alpine AS build
WORKDIR /app

# Copy package files and install dependencies
COPY package*.json ./
RUN npm install

# Copy source code
COPY . .

# Updated build command with memory limit to prevent SIGKILL
RUN node --max-old-space-size=512 ./node_modules/@angular/cli/bin/ng build --configuration production

# Stage 2: Serve the application with Nginx
FROM nginx:stable-alpine

# Clean default nginx contents
RUN rm -rf /usr/share/nginx/html/*

# Copy built assets from build stage
# Note: Ensure 'angular-15-crud' matches the project name in your angular.json
COPY --from=build /app/dist/angular-15-crud /usr/share/nginx/html

# Copy custom nginx configuration
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 8081

CMD ["nginx", "-g", "daemon off;"]
