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
docker build -f Dockerfile-2-multistage-build -t node-app:2.0.0 --no-cache .

// inspec image size in decimal units (1 MB = 1,000,000 bytes)
// 170M
docker image inspect node-app-2:2.0.0 --format='{{.Size}}' | numfmt --to=si

// run container
docker run -p 8080:8080 node-app-2:2.0.0

// check container run success
curl 127.0.0.1:8080
```
