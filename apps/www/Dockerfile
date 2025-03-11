# Build stage
FROM node:18-alpine AS builder

WORKDIR /app

# Copy root package files for workspace
COPY package*.json ./
COPY nx.json ./

# Copy app-specific files
COPY apps/www/package*.json ./apps/www/
COPY apps/www/next.config.ts ./apps/www/
COPY apps/www/tsconfig.json ./apps/www/

# Install dependencies
RUN npm ci

# Copy source code
COPY apps/www ./apps/www

# Build the application
RUN npm run build -w apps/www

# Production stage
FROM node:18-alpine

WORKDIR /app

# Copy built assets
COPY --from=builder /app/apps/www/.next ./.next
COPY --from=builder /app/apps/www/public ./public
COPY --from=builder /app/apps/www/package.json ./package.json

# Install production dependencies
RUN npm ci --only=production

# Expose port
EXPOSE 3000

# Start the application
CMD ["npm", "start"]
