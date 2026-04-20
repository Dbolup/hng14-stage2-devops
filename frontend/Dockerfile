# Stage 1: Builder
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

# Stage 2: Final image
FROM node:18-alpine
WORKDIR /app

# Create non-root user
RUN adduser -D appuser

# Copy dependencies and code
COPY --from=builder /app/node_modules ./node_modules
COPY . .

RUN chown -R appuser:appuser /app
USER appuser

EXPOSE 3000

HEALTHCHECK --interval=30s --timeout=10s --start-period=10s --retries=3 \
  CMD wget -qO- http://localhost:3000/health || exit 1

CMD ["node", "app.js"]
