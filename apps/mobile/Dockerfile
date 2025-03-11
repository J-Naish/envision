# Build stage
FROM node:18-alpine AS builder

WORKDIR /app

# Copy root package files for workspace
COPY package*.json ./
COPY nx.json ./

# Copy app-specific files
COPY apps/mobile/package*.json ./apps/mobile/
COPY apps/mobile/app.json ./apps/mobile/
COPY apps/mobile/tsconfig.json ./apps/mobile/

# Install dependencies
RUN npm ci

# Copy source code
COPY apps/mobile ./apps/mobile

# Install Expo CLI
RUN npm install -g expo-cli

# Build the application
# Note: This builds the web version. For iOS/Android, you typically use EAS (Expo Application Services)
RUN cd apps/mobile && expo export:web

# Production stage for web build
FROM nginx:alpine

# Copy the built web assets
COPY --from=builder /app/apps/mobile/web-build /usr/share/nginx/html

# Expose port
EXPOSE 80

# Start nginx
CMD ["nginx", "-g", "daemon off;"]

# Note: For actual mobile builds, you would typically:
# 1. Use EAS Build on Expo's cloud infrastructure
# 2. Or use fastlane in a CI/CD pipeline
# 3. Or use platform-specific build containers (Xcode for iOS, Android SDK for Android)
