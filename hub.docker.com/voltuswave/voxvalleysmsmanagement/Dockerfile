FROM node:10

# Create app directory
RUN mkdir -p /usr/src/VoxvalleySmsManagement
WORKDIR /usr/src/VoxvalleySmsManagement           

# Bundle app source
COPY . /usr/src/VoxvalleySmsManagement

# Install app dependencies
RUN WITH_SASL=0 npm install

CMD [ "node", "server.js"]


