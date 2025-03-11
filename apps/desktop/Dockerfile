# Build stage
FROM node:18 AS builder

WORKDIR /app

# Install required system dependencies for Electron
RUN apt-get update && apt-get install -y \
    python3 \
    pkg-config \
    make \
    g++ \
    libx11-dev \
    libxkbfile-dev \
    libsecret-1-dev

# Copy root package files for workspace
COPY package*.json ./
COPY nx.json ./

# Copy app-specific files
COPY apps/desktop/package*.json ./apps/desktop/
COPY apps/desktop/tsconfig.json ./apps/desktop/

# Install dependencies
RUN npm ci

# Copy source code
COPY apps/desktop ./apps/desktop

# Build the application
RUN npm run build -w apps/desktop

# Package stage
FROM electronuserland/builder:wine

WORKDIR /app

# Copy built assets
COPY --from=builder /app/apps/desktop/dist ./dist
COPY --from=builder /app/apps/desktop/package.json ./package.json

# Install production dependencies
RUN npm ci --only=production

# Build for different platforms
CMD ["npm", "run", "dist"]
