# 20231012-Lab_Containerization

## Steps in Dockerizing Backend API

### 1. Setting up ExpressJS

Make directory for this lab
```shell
mkdir lab
```
```shell
cd lab
```

Make directory lets call 'backend-api'
```shell
cd backend-api
```
Initialise package.json
```shell
npm init -y
```
Add express dependencies
```json
  "dependencies": {
    "express": "^4.18.2"
  }
```

Create index.js and replace the content with
```js
  'use strict';
  
  const express = require('express')
  const app = express()
  const port = 3000
  
  app.get('/', (req, res) => {
    res.send('Hello World!')
  })
  
  app.listen(port, () => {
    console.log(`Example app listening on port ${port}`)
  })
```

Done.


### 2. Creating Dockerfile for Backend API Service
```shell
touch Dockerfile
```

Replace the content with
```Dockerfile
# stage one
FROM node:lts-alpine as builder

WORKDIR /app

COPY ./package.json .
RUN npm install --only=prod

# stage two
FROM node:lts-alpine

EXPOSE 3000
ENV NODE_ENV=production

USER node
RUN mkdir -p /home/node/app
WORKDIR /home/node/app

COPY . .
COPY --from=builder /app/node_modules  /home/node/app/node_modules

CMD [ "node", "index.js" ]
```

### 3. Building backend-api image
```shell
  docker build -t backend-api:0.0.1 -f Dockerfile .
```
List out images
```shell
 docker image ls
```
### 4. Running image into a container
```shell
docker container run --rm -d -p 3000:3000 backend-api:0.0.1 
```
Listing containers running
```shell
docker container ps
```
### 5. Stopping running container
```shell
docker container stop <names>
# @
docker container stop <container id>
```

## Adding Database

