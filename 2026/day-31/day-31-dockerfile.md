# Day 31 – Dockerfile: Build Your Own Images

##  What I Learned

* Dockerfile is used to create custom images
* Images are built using layered architecture
* Each instruction creates a layer
* Docker uses caching to speed up builds


##  Key Instructions

* FROM → base image
* RUN → execute commands
* COPY → copy files
* WORKDIR → set working directory
* EXPOSE → document port
* CMD → default command
* ENTRYPOINT → fixed command


##  CMD vs ENTRYPOINT

* CMD can be overridden
* ENTRYPOINT is fixed
* CMD is used for defaults
* ENTRYPOINT is used for tools


##  Build Command

docker build -t image-name:tag .


##  Run Command

docker run image-name

## Key Concepts

* Docker caching improves build speed
* Layer order matters
* Frequently changing layers should be last

##  Example Use Cases

* Custom Ubuntu image with tools
* Static website using Nginx
* Application containerization


##  Real-World Understanding

Dockerfile helps developers:

* Standardize environments
* Ship applications faster
* Avoid "works on my machine" problems

##  Practice Done
* Built custom Ubuntu image
* Used COPY and WORKDIR
* Understood CMD vs ENTRYPOINT
* Deployed static website

##  Final Thought

Dockerfile is the heart of Docker.
If you master Dockerfile, you can build anything with Docker.
