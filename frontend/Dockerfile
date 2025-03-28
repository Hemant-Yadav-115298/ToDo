# Dockerfile for React client

# Build react client
FROM node:16-alpine

# Working directory be app
WORKDIR /usr/src/app

COPY package*.json ./

###  Installing dependencies

RUN npm install

# copy local files to app folder
COPY . .

RUN npm run build

RUN npm install -g serve

EXPOSE 3000

CMD ["serve","-s","build","-l","3000"]