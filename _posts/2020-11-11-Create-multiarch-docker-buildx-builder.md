## Create a multiarch Docker Buildx builder

Run the following commands:
```
docker run --rm --privileged multiarch/qemu-user-static --reset -p yes
docker buildx rm builder
docker buildx create --name builder --driver docker-container --use
docker buildx inspect --bootstrap
```
Inspect the builder by using the command:

`docker buildx ls`

Remove builder:
`docker buildx rm builder`

