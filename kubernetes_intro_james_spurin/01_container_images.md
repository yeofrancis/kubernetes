Challenge 1: Pull the latest version of the alpine image using the traditional docker command format

Challenge 2: Compare the digest in the pull command output to that on Docker Hub. You may be surprised to see that they don’t actually match!

The reason being, in the video when we used the funbox image which is an image with a single architecture only. The alpine image covers many different variants such as amd64 and arm and hence, the digest shown in the ‘docker pull’ output is a top level digest that references all platform variants that you see individually listed in the Docker Hub output.

There’s a convenient tool that you can use that will show the top level digest, compare the top of this report to what you see in the docker pull output - https://explore.ggcr.dev/?image=alpine

Challenge 3: Use the docker history command to inspect the layers that comprise of this image

Challenge 4: Compare this to the layers listing on Docker Hub

Challenge 5: Identify the image id and digest

Challenge 6: Remove the alpine image and confirm that it is removed

Challenge 7: Capture a specific version of ubuntu by using the tag 20.04 and the alternative command syntax in the format of docker noun verb

Challenge 8: Check the digest, using the alternative command syntax

Challenge 9: Inspect the layers of this image using docker history, via the alternative command syntax

Challenge 10: Remove the ubuntu:20.04 image using the alternative command syntax and confirm that it is removed, again using the alternative command syntax

Solution:

docker pull alpine:latest
latest: Pulling from library/alpine
fe07684b16b8: Pull complete 
Digest: sha256:8a1f59ffb675680d47db6337b49d22281a139e9d709335b492be023728e11715
Status: Downloaded newer image for alpine:latest
docker.io/library/alpine:latest

Challenge 3: Use the docker history command to inspect the layers that comprise of this image

docker history alpine:latest
IMAGE          CREATED      CREATED BY                                      SIZE      COMMENT
cea2ff433c61   9 days ago   CMD ["/bin/sh"]                                 0B        buildkit.dockerfile.v0
<missing>      9 days ago   ADD alpine-minirootfs-3.22.0-x86_64.tar.gz /…   8.31MB    buildkit.dockerfile.v0

It is showing as:
DD alpine-minirootfs-… /… means Docker unpacked the mini root filesystem tarball into the container’s root, creating an 8.31 MB layer.

CMD ["/bin/sh"] sets the default command but doesn’t change the filesystem, so it’s 0 B. 

Challenge 5: Identify the image id and digest

docker images
REPOSITORY   TAG       IMAGE ID       CREATED      SIZE
alpine       latest    cea2ff433c61   9 days ago   8.31MB

docker images --digests
REPOSITORY   TAG       DIGEST                                                                    IMAGE ID       CREATED      SIZE
alpine       latest    sha256:8a1f59ffb675680d47db6337b49d22281a139e9d709335b492be023728e11715   cea2ff433c61   9 days ago   8.31MB

Challenge 6: Remove the alpine image and confirm that it is removed

docker images -a
REPOSITORY   TAG       IMAGE ID       CREATED      SIZE
alpine       latest    cea2ff433c61   9 days ago   8.31MB

docker rmi cea2
Untagged: alpine:latest
Untagged: alpine@sha256:8a1f59ffb675680d47db6337b49d22281a139e9d709335b492be023728e11715
Deleted: sha256:cea2ff433c610f5363017404ce989632e12b953114fefc6f597a58e813c15d61
Deleted: sha256:fd2758d7a50e2b78d275ee7d1c218489f2439084449d895fa17eede6c61ab2c4

docker images -a
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE

Challenge 7: Capture a specific version of ubuntu by using the tag 20.04 and the alternative command syntax in the format of docker noun verb

docker pull ubuntu:20.04
20.04: Pulling from library/ubuntu
13b7e930469f: Pull complete 
Digest: sha256:8feb4d8ca5354def3d8fce243717141ce31e2c428701f6682bd2fafe15388214
Status: Downloaded newer image for ubuntu:20.04
docker.io/library/ubuntu:20.04

Challenge 8: Check the digest, using the alternative command syntax

docker images --digests ubuntu:20.04
REPOSITORY   TAG       DIGEST    IMAGE ID       CREATED        SIZE
ubuntu       20.04     <none>    b7bab04fd9aa   2 months ago   72.8MB

Challenge 9: Inspect the layers of this image using docker history, via the alternative command syntax

docker image history ubuntu:20.04
IMAGE          CREATED        CREATED BY                                      SIZE      COMMENT
b7bab04fd9aa   2 months ago   /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B        
<missing>      2 months ago   /bin/sh -c #(nop) ADD file:f9ee450324e6ff2c9…   72.8MB    
<missing>      2 months ago   /bin/sh -c #(nop)  LABEL org.opencontainers.…   0B        
<missing>      2 months ago   /bin/sh -c #(nop)  LABEL org.opencontainers.…   0B        
<missing>      2 months ago   /bin/sh -c #(nop)  ARG LAUNCHPAD_BUILD_ARCH     0B        
<missing>      2 months ago   /bin/sh -c #(nop)  ARG RELEASE                  0B        

Challenge 10: Remove the ubuntu:20.04 image using the alternative command syntax and confirm that it is removed, again using the alternative command syntax

 docker image remove ubuntu:20.04
Untagged: ubuntu:20.04
Untagged: ubuntu@sha256:8feb4d8ca5354def3d8fce243717141ce31e2c428701f6682bd2fafe15388214
Deleted: sha256:b7bab04fd9aa0c771e5720bf0cc7cbf993fd6946645983d9096126e5af45d713
Deleted: sha256:470b66ea5123c93b0d5606e4213bf9e47d3d426b640d32472e4ac213186c4bb6

docker image list
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE