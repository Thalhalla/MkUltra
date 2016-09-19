# Security

Some security notes on docker in general and some specifics to my philoshophy on what is done with these Makefiles.

### docker group

##### Anyone in the '*docker*' group essentially has '**root**'.

With the docker group you can create a container that can read and write to `/etc/shadow`,  therefore anyone who has the *docker* group can obtain **root**.  This leads me to the conclusion that a VM with docker on it has a singular domain of trust.  Meaning you should only let people into the docker group that are trusted administrators for **everything** on that server.

Given that I'm of the opinion that local information specific to a docker container can be written to disc.  In my Makefiles many times I will ask the user for info, this is merely echoed out  into local files that are in the `.gitignore` file to prevent their contents from being committed into git.

Again, anyone with the *docker* group can obtain root and read these files.  But, in my experience, I do not have 'regular' users on a docker host.  Docker hosts are very singular in purpose, they run docker images, not much else.  And the only people logging into these things are trusted admins.

