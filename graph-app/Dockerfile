# -------------------------------
# Stage 1: Build Angular App
# -------------------------------
FROM node:18-bullseye-slim AS build
WORKDIR /app

# Copy package files and install dependencies
COPY package*.json ./
RUN npm ci

# Copy the rest of the source code
COPY . .

# Build Angular app in production mode
RUN npx ng build --configuration=production

# -------------------------------
# Stage 2: Serve with Nginx
# -------------------------------
FROM nginx:alpine

# Remove default nginx config
RUN rm -rf /etc/nginx/conf.d/default.conf

# Copy custom nginx config
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy built Angular app
COPY --from=build /app/dist/graph-app /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Run nginx in foreground
CMD ["nginx", "-g", "daemon off;"]


# # From the root of your Angular app (where Dockerfile is)
# # 1. Build Angular app + Docker image
# docker build -t libraryacr.azurecr.io/library-frontend:sso .

# # 2. Log in to Azure Container Registry
# az acr login --name libraryacr

# # 3. Push image to ACR
# docker push libraryacr.azurecr.io/library-frontend:sso
