FROM mhart/alpine-node:10 as front
ENV TZ="Europe/Amsterdam"
# install missing alpine packages
RUN apk add --update git openssh
RUN npm i npm@6.9.0 -g
RUN mkdir /frontend
WORKDIR /frontend
COPY package*.json ./
ENV NODE_ENV=production
# RUN npm i --only=production && npm cache clean --force
RUN npm i --only=production
COPY . .
RUN npm run build
# this is where the compiled version of the frontend lands
RUN ls -al /frontend/build

# Stage 2: the react app runs behind its own nginx instance.
FROM nginx:1.17.7-alpine
EXPOSE 3000
# Remove default nginx website
RUN rm -rf /usr/share/nginx/html/*
COPY ./nginx.conf /etc/nginx/conf.d/default.conf
# copy only static files (the minimized version) of the react app
COPY --from=front /frontend/build /usr/share/nginx/html

CMD ["nginx", "-g", "daemon off;"]
RUN ls -al /etc/nginx/conf.d/
RUN ls -al /usr/share/nginx/html

RUN echo "\e[104mAfter running the docker container the frontend will start at localhost:80\e[0m\n\n"

# https://github.com/nodejs/docker-node/blob/master/docs/BestPractices.md#memory
# docker build -t production_frontend -f ./Dockerfile.prod . --no-cache
# docker run --init --name=tg-frontend -p 3000:80 -m "300M" --memory-swap "1G" -u "node" production_frontend

# docker run -d --name=tg-frontend -p 3000:80 production_frontend
# docker attach tg-frontend