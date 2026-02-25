# How to Reduce Docker Image Size: 6 Optimization Methods

- Reference: https://devopscube.com/reduce-docker-image-size/

## Method 1: Use Minimal Base Images

## Method 2: Use Docker Multistage Builds

```sh
// use the current directory as the build context
docker build -f Dockerfile-2 -t node-app-2:1.0.0 --no-cache .

// inspec image size in decimal units (1 MB = 1,000,000 bytes)
// 917M
docker image inspect node-app-2:1.0.0 --format='{{.Size}}' | numfmt --to=si
```

- Use multistage build

```sh
// use the current directory as the build context
docker build -f Dockerfile-2-multistage-build -t node-app-2:2.0.0 --no-cache .

// inspec image size in decimal units (1 MB = 1,000,000 bytes)
// 170M
docker image inspect node-app-2:2.0.0 --format='{{.Size}}' | numfmt --to=si

// run container
docker run -p 8080:8080 node-app-2:2.0.0

// check container run success
curl 127.0.0.1:8080
```

## Method 3: Minimize the Number of Layers

```sh
docker build -f Dockerfile-3 -t update-app-3:1.0.0 --no-cache .

// 264M
docker image inspect update-app-3:1.0.0 --format='{{.Size}}' | numfmt --to=si
```

- Minimize the number of layers (reduce COPY command)

```sh
docker build -f Dockerfile-3-minimize-the-number-of-layers -t update-app-3:2.0.0 --no-cache .

// 261M
docker image inspect update-app-3:2.0.0 --format='{{.Size}}' | numfmt --to=si
```

## Method 4: Understading Caching

```sh
docker build -f Dockerfile-4 -t update-app-4:1.0.0 --no-cache .

// 261M
docker image inspect update-app-4:1.0.0 --format='{{.Size}}' | numfmt --to=si
```

- Understading Caching (Install dependencies before COPY command)

```sh
docker build -f Dockerfile-4-understanding-caching -t update-app-4:2.0.0 --no-cache .

// 261M
docker image inspect update-app-4:2.0.0 --format='{{.Size}}' | numfmt --to=si
```

## Method 5: Use Dockerignore

## Method 6: Keep Application Data Elsewhere

## Docker Image Optimization Tools

### 1. [dive](https://github.com/wagoodman/dive):

- It is an image explore tool that helpes you discover layers in the Docker & OCI containers images.

- Using Five, you can fond ways to optimize your Docker images.

```sh
brew install dive
dive <image-name:tag>
// Ex: dive node-app-2:2.0.0
```

### 2. [mintoolkit/mint](https://github.com/mintoolkit/mint):

- It helps you optimize your Docker images for security and size.

```sh
docker run --rm \
  -v /var/run/docker.sock:/var/run/docker.sock \
  mintoolkit/mint slim \
  --tag node-app-2.slim:2.0.0 \
  node-app-2:2.0.0

// 172M
docker image inspect node-app-2:2.0.0 --format='{{.Size}}' | numfmt --to=si
// 142M
docker image inspect node-app-2.slim:2.0.0 --format='{{.Size}}' | numfmt --to=si
```

## Scan Docker Images

- [trivy](https://github.com/aquasecurity/trivy)

```sh
brew install trivy
trivy image node-app-2.slim:2.0.0
```

### [hadolint](https://github.com/hadolint/hadolint)

- Dockerfile linter, validate inline bash

```sh
brew install hadolint
hadolint Dockerfile-2
```
