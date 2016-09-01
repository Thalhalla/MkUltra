# Prototype

I have a prototyping repo here:

https://github.com/joshuacox/docker-prototype

### Usage

Assuming you keep all your git repos in `~/git` make a new repo for your new docker container, then`cd` into this repo, next you can call the `mknewProto.sh` from the current working directory which is your new repo, like this:
```
 ../docker-prototype/mknewProto.sh 
 Making prototype docker in current working directory Ctrl-C now to exit!!!!!!
```

Don't worry if you already have some files in there, the script uses `cp -i` and will prompt you before overwriting.

