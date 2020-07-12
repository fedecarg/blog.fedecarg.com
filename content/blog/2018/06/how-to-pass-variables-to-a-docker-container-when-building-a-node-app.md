---
title: "How to pass variables to a Docker container when building a Node app"
date: "2018-06-05"
tags: [javascript,linux,node-js,open-source,programming]
---

Environment variables are declared with the **ENV** statement and are notated in the Dockerfile either with **$VARIABLE\_NAME** or **${VARIABLE\_NAME}**.

## Passing variables at build-time

The **ENV** instruction sets the environment variable to the value. The environment variables set using ENV will persist when a container is run from the resulting image. For example:

```sh
FROM node:9

ENV PORT 3000
ENV NODE_ENV development
```

The Dockerfile allows you to specify arguments at build-time. The **ARG** instruction defines a variable that users can pass at to the builder:

```sh
FROM node:9

ARG PORT
ARG NODE_ENV
```

When building a Docker image from the command line, you can set those values using –build-arg:

```sh
docker build --tag webapp --build-arg PORT=3000 --build-arg NODE_ENV=development .
```

## Executing commands using the shell

And, here is the secret ingredient. If the $NODE\_ENV variable is set, then you can use the shell to run an NPM script:

```sh
FROM node:9

ARG PORT
ARG NODE_ENV

ENV PORT $PORT
ENV NODE_ENV $NODE_ENV

RUN mkdir -p /usr/app
WORKDIR /usr/app
RUN cd /usr/app
ADD . .

RUN npm install
RUN /bin/bash -c '[[ "${NODE_ENV}" == "production" ]] && npm run build:prod || npm run build:dev'

EXPOSE $PORT

CMD ["npm", "run", "start"]
```

Finally, you expose the port number and start the HTTP server.

That's it! Thanks for reading and happy Dockering :)
