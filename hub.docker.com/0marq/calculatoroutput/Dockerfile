FROM node:8-alpine

# Create app directory
WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install

# Bundle app source
COPY . .
EXPOSE 8060 8070
CMD ["npm", "start"]
