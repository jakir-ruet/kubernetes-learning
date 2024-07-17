#### Welcome to Docker Multi Stage Build
[Multi Stage Build](https://docs.docker.com/build/guide/multi-stage/)
Multi-stage builds are useful to anyone who has struggled to optimize Dockerfiles while keeping them easy to read and maintain.

**Use multi-stage builds**
Actually, executing the application only needs the runtime, not the base image. Suppose we want to run a .net/python application then we need only a .net/python runtime. But in Docker creating images we need base images such as 'Ubuntu Image, Apt, Yum' & other repositories. For this reason, image memory is more than runtime that way it will be slow work and more memory consumption.

With multi-stage builds, you use multiple `FROM` statements in your Dockerfile. Each `FROM` instruction can use a different base, and ***each of them begins a new stage*** of the build. You can selectively copy artifacts from one stage to another, leaving behind everything you don't want in the final image. There are two main reasons for why you’d want to use multi-stage builds: 
- They allow you to run build steps in parallel, making your build pipeline faster and more efficient.
- They allow you to create a final image with a smaller footprint, containing only what's needed to run your program.

The following Dockerfile has two separate stages: one for building a binary, and another where the binary gets copied from the first stage into the next stage.

#### Let's see hand on

Application Test
```bash
go build main.go
./main
```

Build as base image
```bash
# BASE IMAGE BUILD
FROM ubuntu AS build
RUN apt-get update && apt-get install golang-go -y
ENV GO111MODULE=off
COPY . .
RUN CGO_ENABLED=0 go build -o /app .
ENTRYPOINT [ "/app" ]
```

Build & check memory
```bash
docker build -t go-app-multi .
docker images | head -4
```

Build as multi stage
```bash
# BASE IMAGE BUILD
FROM ubuntu AS build
RUN apt-get update && apt-get install golang-go -y
ENV GO111MODULE=off
COPY . .
RUN CGO_ENABLED=0 go build -o /app .

# MULTI STAGE BUILD
FROM scratch
# COPY THE COMPILED BINARY FROM BUILD STAGE
COPY --from=build /app /app
# SET ENTRYPOINT FOR CONTAINER TO RUN
ENTRYPOINT [ "/app" ]
```

Build & check memory
```bash
docker build -t go-app .
docker images | head -4
```

Push to Docker Hub
```bash
docker build -t jakirbd/docker-compress-app .
docker run -p --name docker-compress-app -d jakirbd/docker-compress-app:latest
docker push jakirbd/docker-compress-app
```