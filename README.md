# Play Framework

### Play Framework 2.1 tags

Based on image [`java:7`](https://hub.docker.com/_/java/)

- [`2.1.4` (Dockerfile)](https://github.com/ifi-navet/docker-playframework/tree/2.1.4)
- [`2.1.5` (Dockerfile)](https://github.com/ifi-navet/docker-playframework/tree/2.1.5)

### Play Framework 2.2 tags

Based on image [`java:7`](https://hub.docker.com/_/java/)

- [`2.2.0` (Dockerfile)](https://github.com/ifi-navet/docker-playframework/tree/2.2.0)
- [`2.2.1` (Dockerfile)](https://github.com/ifi-navet/docker-playframework/tree/2.2.1)
- [`2.2.2` (Dockerfile)](https://github.com/ifi-navet/docker-playframework/tree/2.2.2)
- [`2.2.3` (Dockerfile)](https://github.com/ifi-navet/docker-playframework/tree/2.2.3)
- [`2.2.4` (Dockerfile)](https://github.com/ifi-navet/docker-playframework/tree/2.2.4)
- [`2.2.5` (Dockerfile)](https://github.com/ifi-navet/docker-playframework/tree/2.2.5)
- [`2.2.6` (Dockerfile)](https://github.com/ifi-navet/docker-playframework/tree/2.2.6)

## Tips for your `Dockerfile`

Make builds quicker by only downloading dependencies when they change.

Example `Dockerfile`

```
# Select a tag that fits your project
FROM ifinavet/playframework:2.1.5

# Choose a work directory
WORKDIR /app

# Copy project/ which holds your project definitions
COPY project project

# Only for Play Framework 2.2.*
COPY build.sbt ./

# Trigger dependency download
RUN play help

# Copy your entire project
COPY ./ ./

# Your start command
CMD play run
```

This will create intermediate builds for your image.
If there are changes to your `project/` directory, it will build your image from the `COPY project project`.

If the only thing you changed is in your source code, it will run from `COPY ./ ./` and keep your dependency installs like it was on your last build.


**Worth noting**

There are generated output in your `project/` directory that may trigger a complete rebuild each time you build your image.
The following should be included in your `.dockerignore` file

```
target/
project/target/
project/project/
```

### Development mode

Just mount your source directory to the container workdir, and with `play run` as your `CMD` it should recompile your running code on the fly.

With docker-compose you can do the following

File: `docker-compose.yml`

```
app:
  build: . # Builds Dockerfile in this directory, your application
  volumes:
    - ./:/app # Mount this directory to container /app
  ports:
    - 9000:9000 # Expose the necessary ports
  command: play run # Override CMD in Dockerfile
```

Run it with `docker-compose up` (defaults to `docker-compose.yml` if no `-f` tag is specified)

Stop docker-compose containers: `docker-compose stop`

Remove docker-compose containers: `docker-compose rm <optional labels>`
