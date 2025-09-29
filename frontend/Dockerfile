FROM node:20-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm ci
COPY . .

RUN npm run build -- --prod


FROM nginx:stable-alpine AS final

RUN apk add --no-cache curl
COPY nginx.tmpl /etc/nginx/templates/default.conf.template
COPY --from=builder /app/dist/frontend /usr/share/nginx/html

HEALTHCHECK --interval=30s --timeout=5s \
  CMD curl -I http://localhost/ || exit 1

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]