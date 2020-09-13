# micro-nodejs-server

* Micro nodejs native app server
* Running node.js server on docker container

## Build from the scratch

### Init with yarn

```bash
# npm init
$ yarn init
```

### Edit package.json file
```json
{
  "name": "micro_nodejs_server",
  "version": "1.0.0",
  "description": "Node.js on Docker",
  "author": "akagaeng<akagaeng@gmail.com>",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.17.1"
  }
}
```

### Install package

```bash
# npm install
$ yarn
```

### Write server.js

```js
'use strict';

const express = require('express');
const PORT = 8080;
const HOST = '0.0.0.0';

const app = express();
app.get('/', (req, res) => {
  res.send('Hello world\n');
});

app.listen(PORT, HOST);
console.log(`Running on http://${HOST}:${PORT}`);
```


### Create Dockerfile

```dockerfile
FROM node:12

WORKDIR /usr/src/app

COPY package.json yarn.lock ./
RUN yarn

COPY . .

EXPOSE 8080
CMD [ "yarn", "start" ]
```

### .dockerignore

```
node_modules
npm-debug.log
```

### Build docker image

```bash
# docker build -t <your username>/<app name> .
$ docker build -t akagaeng/micro-nodejs-server .
```

### Inspect images

```bash
$ docker images

REPOSITORY                                   TAG                             IMAGE ID            CREATED             SIZE
akagaeng/micro-nodejs-server                 latest                          cef2fa8d9dc6        3 minutes ago       915MB
```

### Run Docker container using image

```bash
docker run -p 80:8080 -d --name micro-nodejs-server akagaeng/micro-nodejs-server

# external port: 80
# internal port: 8080
```

### Inspect docker container

```bash
$ docker ps

CONTAINER ID        IMAGE                          COMMAND                  CREATED             STATUS              PORTS                               NAMES
a60e16cdda5e        akagaeng/micro-nodejs-server   "docker-entrypoint.s…"   3 seconds ago       Up 3 seconds        0.0.0.0:80->8080/tcp                micro-nodejs-server
```

### Show Docker logs
```
# docker logs -f <Container ID or name>
$ docker logs -f micro-nodejs-server

yarn run v1.19.1
$ node server.js
Running on http://0.0.0.0:8080
```

### Start a Bash session
```
# docker exec -it <Container ID or name> /bin/bash
docker exec -it <Container ID or name> bash
```

### Inspect web app running

```
$ curl localhost:80
Hello world
```