# Build stage
FROM node:18-alpine AS builder

WORKDIR /app

# Copy root package files for workspace
COPY package*.json ./
COPY nx.json ./

# Copy server-specific files
COPY server/package*.json ./server/
COPY server/tsconfig.json ./server/

# Install dependencies
RUN npm ci

# Copy source code
COPY server ./server

# Build the application
RUN npm run build -w server

# Production stage
FROM node:18-alpine

WORKDIR /app

# Copy built assets
COPY --from=builder /app/server/dist ./dist
COPY --from=builder /app/server/package.json ./package.json

# Install production dependencies
RUN npm ci --only=production

# Expose port
EXPOSE 4000

# Start the application
CMD ["node", "dist/app.js"]
