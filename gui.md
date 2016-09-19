# GUI in docker

### [Subuser](http://subuser.org/)

Subuser is a great project that will automate much of this stuff

### [Jessie Frazelle](https://blog.jessfraz.com/)

Jessie introduced me to this topic with her blog [article here](https://blog.jessfraz.com/post/docker-containers-on-the-desktop/)

### Discussion

Stackexhange [issue](http://unix.stackexchange.com/questions/118811/why-cant-i-run-gui-apps-from-root-no-protocol-specified#answer-118826) discussing some of this

Another github [issue](https://github.com/docker/docker/issues/18361) hilighting arduino issues

### manually

My [mkarduino](https://github.com/joshuacox/mkarduino) is a decent example of getting a gui app to work on the desktop from docker.  You can see that in my `docker run` command I have volume mounts where we mount in a `.Xauthority` file, and `/tmp/.X11-unix:rw`, and we have the environmanet variables DISPLAY and XAUTHORITY, and finally the `--device /dev/dri`

```
rundocker:
        $(eval TMP := $(shell mktemp -d --suffix=DOCKERTMP))
        $(eval NAME := $(shell cat NAME))
        $(eval TAG := $(shell cat TAG))
        $(eval PWD := $(shell pwd ))
        chmod 777 $(TMP)
        @docker run --name=$(NAME) \
        --cidfile="cid" \
        -v $(TMP):/tmp \
        -d \
        -P \
        --device /dev/dri \
        -e DISPLAY=unix:0 \
        --volume=/tmp/.X11-unix:/tmp/.X11-unix:rw \
        --volume=$(PWD)/.Xauthority:/home/arduino/.Xauthority:ro \
        -e XAUTHORITY=/home/arduino/.Xauthority \
        --privileged \
        -v /var/run/docker.sock:/run/docker.sock \
        -v $(PWD)/local-preferences:/home/arduino/.arduino \
        -v $(shell which docker):/bin/docker \
        -v $(shell cat GIT_DATADIR):/home/git \
        -v $(shell cat SKETCHBOOK):/home/arduino/Arduino \
        -t $(TAG)
```

To make the `.Xauthority` file I used this:

```
.Xauthority:
        xauth extract .Xauthority :0
        sudo chown 1001:1001 .Xauthority
```

