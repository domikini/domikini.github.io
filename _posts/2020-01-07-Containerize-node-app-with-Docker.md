# [Dockerizing a Node.js web app](https://nodejs.org/de/docs/guides/nodejs-docker-webapp/#dockerizing-a-node-js-web-app)

## [Creating a Dockerfile](https://nodejs.org/de/docs/guides/nodejs-docker-webapp/#creating-a-dockerfile)

Create an empty file called `Dockerfile`:

```markup
touch Dockerfile
```

Open the `Dockerfile` in your favorite text editor

The first thing we need to do is define from what image we want to build from. Here we will use the latest LTS (long term support) version `10` of `node` available from the [Docker Hub](https://hub.docker.com/):

```docker
FROM node:10
```

Next we create a directory to hold the application code inside the image, this will be the working directory for your application:

```docker
# Create app directory
WORKDIR /usr/src/app
```

This image comes with Node.js and NPM already installed so the next thing we need to do is to install your app dependencies using the `npm` binary. Please note that if you are using `npm` version 4 or earlier a `package-lock.json` file will _not_ be generated.

```docker
# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm install
# If you are building your code for production
# RUN npm ci --only=production
```

Note that, rather than copying the entire working directory, we are only copying the `package.json` file. This allows us to take advantage of cached Docker layers. bitJudo has a good explanation of this [here](http://bitjudo.com/blog/2014/03/13/building-efficient-dockerfiles-node-dot-js/). Furthermore, the `npm ci` command, specified in the comments, helps provide faster, reliable, reproducible builds for production environments. You can read more about this [here](https://blog.npmjs.org/post/171556855892/introducing-npm-ci-for-faster-more-reliable).

To bundle your app's source code inside the Docker image, use the `COPY` instruction:

```docker
# Bundle app source
COPY . .
```

Your app binds to port `8080` so you'll use the `EXPOSE` instruction to have it mapped by the `docker` daemon:

```docker
EXPOSE 8080
```

Last but not least, define the command to run your app using `CMD` which defines your runtime. Here we will use `node server.js` to start your server:

```docker
CMD [ "node", "server.js" ]
```

Your `Dockerfile` should now look like this:

```docker
FROM node:10

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm install
# If you are building your code for production
# RUN npm ci --only=production

# Bundle app source
COPY . .

EXPOSE 8080
CMD [ "node", "server.js" ]
```

## .dockerignore file[](https://nodejs.org/de/docs/guides/nodejs-docker-webapp/#dockerignore-file)

Create a `.dockerignore` file in the same directory as your `Dockerfile` with following content:

```
node_modules
npm-debug.log
```

This will prevent your local modules and debug logs from being copied onto your Docker image and possibly overwriting modules installed within your image.

## Building your image[](https://nodejs.org/de/docs/guides/nodejs-docker-webapp/#building-your-image)

Go to the directory that has your `Dockerfile` and run the following command to build the Docker image. The `-t` flag lets you tag your image so it's easier to find later using the `docker images` command:

```bash
docker build -t <your username>/node-web-app .
```

Your image will now be listed by Docker:

```bash
$ docker images

# Example
REPOSITORY                      TAG        ID              CREATED
node                            10         1934b0b038d1    5 days ago
<your username>/node-web-app    latest     d64d3505b0d2    1 minute ago
```

## Run the image[](https://nodejs.org/de/docs/guides/nodejs-docker-webapp/#run-the-image)

Running your image with `-d` runs the container in detached mode, leaving the container running in the background. The `-p` flag redirects a public port to a private port inside the container. Run the image you previously built:

```bash
docker run -p 49160:8080 -d <your username>/node-web-app
```

Print the output of your app:

```bash
# Get container ID
$ docker ps

# Print app output
$ docker logs <container id>

# Example
Running on http://localhost:8080
```

If you need to go inside the container you can use the `exec` command:

```bash
# Enter the container
$ docker exec -it <container id> /bin/bash
```

## Test[](https://nodejs.org/de/docs/guides/nodejs-docker-webapp/#test)

To test your app, get the port of your app that Docker mapped:

```bash
$ docker ps

# Example
ID            IMAGE                                COMMAND    ...   PORTS
ecce33b30ebf  <your username>/node-web-app:latest  npm start  ...   49160->8080
```

In the example above, Docker mapped the `8080` port inside of the container to the port `49160` on your machine.

Now you can call your app using `curl` (install if needed via: `sudo apt-get install curl`):

```bash
$ curl -i localhost:49160

HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Content-Length: 12
ETag: W/"c-M6tWOb/Y57lesdjQuHeB1P/qTV0"
Date: Mon, 13 Nov 2017 20:53:59 GMT
Connection: keep-alive

Hello world
```

We hope this tutorial helped you get up and running a simple Node.js application on Docker.

You can find more information about Docker and Node.js on Docker in the following places:

-   [Official Node.js Docker Image](https://hub.docker.com/_/node/)
-   [Node.js Docker Best Practices Guide](https://github.com/nodejs/docker-node/blob/master/docs/BestPractices.md)
-   [Official Docker documentation](https://docs.docker.com/)
-   [Docker Tag on Stack Overflow](https://stackoverflow.com/questions/tagged/docker)
-   [Docker Subreddit](https://reddit.com/r/docker)
