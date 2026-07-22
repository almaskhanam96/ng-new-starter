FROM node:22-alpine AS build
WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build

FROM nginx:1.27-alpine
COPY --from=build /app/dist /usr/share/nginx/html/dist

RUN set -eux; \
    project_dir="$(find /usr/share/nginx/html/dist -mindepth 1 -maxdepth 1 -type d | head -n1)"; \
    if [ -d "$project_dir/browser" ]; then cp -r "$project_dir/browser/." /usr/share/nginx/html/; \
    else cp -r "$project_dir/." /usr/share/nginx/html/; fi; \
    rm -rf /usr/share/nginx/html/dist

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]