# Stage 1: Build the Angular project
FROM node:12.13.1 AS builder
WORKDIR /app
COPY . .
RUN npm install
RUN npm run build --prod

# Stage 2: Use Caddy to serve static files
FROM caddy:2-alpine
WORKDIR /srv
COPY --from=builder /app/dist/microservice-ui /srv

# Add Caddyfile for SPA routing
COPY Caddyfile /etc/caddy/Caddyfile
EXPOSE 80
CMD ["caddy", "run", "--config", "/etc/caddy/Caddyfile"]
