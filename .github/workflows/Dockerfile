FROM node:22-alpine AS build
WORKDIR /app

# Copy dependency files first to utilize Docker layer caching
COPY package*.json ./
RUN npm ci

# Copy the rest of the application files
COPY . .

# Build the project for production
RUN npm run build --configuration=production

# Stage 2: Serve the application using Nginx
FROM nginx:1.27-alpine

# Copy custom Nginx configuration to handle Angular routing properly
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy the compiled build assets from the build stage
# Note: Replace 'your-app-name' with the actual output directory name found under dist/
COPY --from=build /app/dist/ng-new-starter/browser /usr/share/nginx/html

# Expose port 80 to the outside world
EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]