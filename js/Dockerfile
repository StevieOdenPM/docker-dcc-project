# Stage 1: Build the Vue.js app
FROM node:18-alpine AS build-stage

# Set working directory
WORKDIR /app

# Copy package files and install dependencies
COPY package*.json ./
RUN npm install

# Copy the rest of the application code
COPY . .

# Build the app
RUN npm run build

# Stage 2: Serve the app using Nginx
FROM nginx:stable-alpine AS production-stage

# Copy built files from previous stage
COPY --from=build-stage /app/dist /usr/share/nginx/html

# Copy custom nginx config if available (optional)
# COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
