# stage: 1
FROM node:8 as react-build
WORKDIR /app
COPY . ./
RUN yarn
RUN yarn run build

# stage: 2 — the production environment
FROM nginx:alpine
COPY --from=react-build /app/build /usr/share/nginx/html
COPY --from=react-build /app/nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]