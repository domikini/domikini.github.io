* https://www.ecliptik.com/Cross-Building-and-Running-Multi-Arch-Docker-Images/#multiarch-on-linux
* http://budevg.github.io/posts/docker/2018/08/02/docker-cross-compile.html
* https://github.com/multiarch/qemu-user-static/blob/master/README.md

Run the following command to register qemu-user-static for running multiarch containers in Linux system:
`docker run --rm --privileged multiarch/qemu-user-static:register --reset`

You can then run any docker image with installed emu-user-static binary such as:
* balenalib docker images: https://hub.docker.com/r/balenalib/raspberry-pi
* multiarch docker images: https://hub.docker.com/u/multiarch/
 