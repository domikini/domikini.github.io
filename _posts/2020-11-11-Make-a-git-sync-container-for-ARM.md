## Make a git-sync container for ARM architecture

Download git-sync repo
`git clone https://github.com/kubernetes/git-sync.git`

Make sure you have buildx installed. If not, use the following steps:
```
export DOCKER_BUILDKIT=1
docker build --platform=local -o . git://github.com/docker/buildx
mkdir -p ~/.docker/cli-plugins
mv buildx ~/.docker/cli-plugins/docker-buildx
```

Make sure you have a multiarch bulder. Follow the guide 2020-11-11-Create-multiarch-docker-buildx-builder.

Go to the git-sync folder. Type the following to start the build process.
`make container REGISTRY=registry VERSION=tag GOOS=linux GOARCH=arm`
