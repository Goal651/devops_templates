# ========== STAGE 1: BUILDER ==========
FROM node:18-alpine AS builder

WORKDIR /app

# Copy package files
COPY package.json package-lock.json ./

# Install ALL dependencies
RUN npm ci

# Copy source code
COPY . .

# Build the NestJS app
RUN npm run build

# ========== STAGE 2: PRODUCTION ==========
FROM node:18-alpine AS production

WORKDIR /app

ENV NODE_ENV=production

# Copy package files
COPY package.json package-lock.json ./

# Install ONLY production dependencies
RUN npm ci --only=production

# Copy built app from builder
COPY --from=builder /app/dist ./dist

# Create non-root user
RUN addgroup -g 1001 -S nodegroup && \
    adduser -S nodeuser -u 1001 -G nodegroup && \
    chown -R nodeuser:nodegroup /app

# Switch to non-root user
USER nodeuser

# Expose port
EXPOSE 3000

# Start the NestJS app
CMD ["node", "dist/main"]