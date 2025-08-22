FROM node:22-alpine AS dev
WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .

ENV CHOKIDAR_USEPOLLING=1
EXPOSE 4200
CMD ["npx","ng","serve","--host","0.0.0.0","--port","4200"]


FROM node:22-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .

RUN npx ng build --configuration=production


FROM nginx:alpine AS prod
COPY --from=builder /app/dist/*/browser/ /usr/share/nginx/html/
EXPOSE 80
CMD ["nginx","-g","daemon off;"]
