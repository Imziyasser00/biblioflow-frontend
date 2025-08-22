# ===== Stage DEV (skip in CI, kept for local dev) =====
FROM node:22-alpine AS dev
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
ENV CHOKIDAR_USEPOLLING=1
EXPOSE 4200
CMD ["npx","ng","serve","--host","0.0.0.0","--port","4200"]

# ===== Stage PROD (builder) =====
FROM node:22-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npx ng build --configuration=production

# ===== Final: Nginx =====
FROM nginx:alpine
COPY --from=builder /app/dist/*/browser/ /usr/share/nginx/html/
EXPOSE 80
CMD ["nginx","-g","daemon off;"]
