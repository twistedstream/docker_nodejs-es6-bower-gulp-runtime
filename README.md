# Node.js (ES6) + Bower/Gulp Runtime Docker image

This Docker image is essentially the same thing as [dockerfile/nodejs-bower-gulp-runtime](https://registry.hub.docker.com/u/dockerfile/nodejs-bower-gulp-runtime/), with the following changes:

- It's running Node v0.11 so Harmony (ES6) scripts can be executed (ex: [Koa](http://koajs.com/))
- It copies over a `.bowerrc` file (if it exists) before running `bower install`
- It runs the default Gulp task instead of one called `build`
- It passes the exposed port (`8080`) to the node process via the `PORT` env variable.

### Base Docker Image

* [node:0.11-onbuild](https://registry.hub.docker.com/u/library/node)

### Installation

1. Install [Docker](https://www.docker.com/).

2. Download [automated build](https://registry.hub.docker.com/u/twistedstream/nodejs-es6-bower-gulp-runtime/) from public [Docker Hub Registry](https://registry.hub.docker.com/): `docker pull twistedstream/nodejs-es6-bower-gulp-runtime`

(alternatively, you can build an image from Dockerfile: `docker build -t="twistedstream/nodejs-es6-bower-gulp-runtime" github.com/twistedstream/docker_nodejs-es6-bower-gulp-runtime`)

### Usage

This image assumes that your application:

* has a file named [package.json](https://docs.npmjs.com/files/package.json) listing its dependencies.
* has a file named [bower.json](http://bower.io/docs/creating-packages/) listing its dependencies.
* might have a file named `.bowerrc` specifying Bower configuration.
* has a file named [Gulpfile.js](https://github.com/gulpjs/gulp/blob/master/README.md) with a `default` task.
* has a file named `server.js` as the entrypoint script or define in package.json the attribute: `"scripts": {"start": "node <entrypoint_script_js>"}`
* listens on port `8080` or uses the port passed by the `PORT` env variable.

When building your application docker image, `ONBUILD` triggers install NPM module dependencies of your application using `npm install`.

* **Step 1**: Create a `Dockerfile` in your Node.js application directory with the following content:

```dockerfile
FROM twistedstream/nodejs-es6-bower-gulp-runtime
```

* **Step 2**: Build your container image by running the following command in your application directory:

```sh
docker build -t="app" .
```

* **Step 3**: Run application by mapping port `8080`:

```sh
APP=$(docker run -d -p 8080 app)
PORT=$(docker port $APP 8080 | awk -F: '{print $2}')
echo "Open http://localhost:$PORT/"
```
