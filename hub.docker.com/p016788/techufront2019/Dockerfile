#Imagen base
FROM node:latest

#Directorio de la app
WORKDIR /app

#Copio archivos
ADD /build/default /app/build/default
ADD server.js /app
ADD package.json /app

#Dependencias
RUN npm install

#Puerto que expongo
EXPOSE 3002

#Comando
CMD ["npm", "start"]
