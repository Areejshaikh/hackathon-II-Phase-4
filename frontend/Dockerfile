# Use Node 18 as base image
FROM node:18-alpine AS builder

# Set working directory
WORKDIR /app

# Install git (needed for some dependencies that might be pulled from git)
RUN apk add --no-cache git

# Copy package files
COPY package*.json ./

# Install all dependencies (including dev dependencies for building)
RUN npm ci

# Copy the rest of the application code
COPY . .

# Build the application
RUN npm run build

# Production stage
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Install dumb-init for proper signal handling
RUN apk add --no-cache dumb-init

# Copy package files
COPY package*.json ./

# Install only production dependencies
RUN npm ci --only=production && npm cache clean --force

# Copy built application from builder stage
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/node_modules ./node_modules

# Expose port
EXPOSE 3000

# Health check - ping a simple route
HEALTHCHECK --interval=30s --timeout=30s --start-period=5s --retries=3 \
    CMD wget -qO- http://localhost:3000/health || exit 1

# Start the application with dumb-init
ENTRYPOINT ["dumb-init", "--"]
CMD ["npm", "start"]