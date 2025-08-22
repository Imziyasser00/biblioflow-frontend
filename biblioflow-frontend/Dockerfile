# ===== Stage DEV (hot reload) =====
FROM node:22-alpine AS dev
WORKDIR /app

# Deps
COPY package*.json ./
RUN npm ci

# Code
COPY . .

# Serve the app in dev (hot reload)
ENV CHOKIDAR_USEPOLLING=1
EXPOSE 4200
CMD ["npx","ng","serve","--host","0.0.0.0","--port","4200"]


# ===== Stage PROD (build + runtime) =====
FROM node:22-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .

# Angular 17+ builds into dist/<project>/browser by default
RUN npx ng build --configuration=production


# ===== Final runtime: Nginx =====
FROM nginx:alpine AS prod
# Copy only the compiled Angular app
COPY --from=builder /app/dist/*/browser/ /usr/share/nginx/html/
EXPOSE 80
CMD ["nginx","-g","daemon off;"]
