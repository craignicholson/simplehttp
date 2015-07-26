# Deploy a Go Server with Docker
Craig Nicholson
2015-07-26

## Goal
We will walk through a series of tasks for creating a Docker container
for a simple Go web application and deploying the container to Google
Compute Engine.

You need be familiar with Go and Docker before continuing with this example.

## simplehttp
Simple http server to test docker and golang deployment.  

main.go
```go
package main

import (
    "fmt"
    "net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hi there, I love %s!", r.URL.Path[1:])
}

func main() {
    http.HandleFunc("/", handler)
    http.ListenAndServe(":8080", nil)
}

```

## Dependencies
* Docker - 1.7.1 
* Git - git version 1.9.1
* Go

### Setup Docker
* Linux
* MacOSX
* Windows

### Setup Git
* Linux
* MacOSX
* Windows

### Setup Go
You can run this example without installing go locally.  

If you run this without installing go you will need to adjust the file paths in the Dockerfile

Follow and setup go using the documentation/blog 
http://golang.org/doc/code.html#Workspaces

Screen cast https://www.youtube.com/watch?v=XCsL89YtqCs

### Test Setup Results
```
$ docker version 
Client version: 1.7.1  
Client API version: 1.19  
Go version (client): go1.4.2  
Git commit (client): 786b29d  
OS/Arch (client): linux/amd64  
Server version: 1.7.1  
Server API version: 1.19  
Go version (server): go1.4.2  
Git commit (server): 786b29d  
OS/Arch (server): linux/amd64  

git version 1.9.1 

```

## Clone the existing repository to your environment
 
Use git clone this repository locally
https://github.com/craignicholson/simplehttp

```
$ git clone https://github.com/craignicholson/simplehttp

```

## Edit the Dockerfile
Edit the docker file so the source code and paths match your exiting
setup

Example:
If you downloaded the code to a folder test

## Build the image
Run Docker build from within the folder with the code and Dockerfile

The docker build command is run against the docker files to build the image
* -t, terminal
* cnicholson/simplehttp:1.0 [repository/yourImageName:Version]
* .  [the dot, specifies we are in current folder]  

```
$ docker build -t cnicholson/simplehttp:1.0 .
Sending build context to Docker daemon 63.49 kB
Sending build context to Docker daemon 
Step 0 : FROM golang
latest: Pulling from golang
607e965985c1: Pull complete 
0f5121dd42a6: Pull complete 
8d38711ccc0d: Pull complete 
8ddc08289e1a: Pull complete 
d86979befb72: Pull complete 
b279b4aae826: Pull complete 
63e9d2557cd7: Pull complete 
8fb45e60e014: Pull complete 
141b650c3281: Pull complete 
69c177f0c117: Pull complete 
124e2127157f: Already exists 
902b87aaaec9: Already exists 
9a61b6b1315e: Already exists 
1ff9f26f09fb: Already exists 
golang:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
Digest: sha256:c5475cd573c185466850411aa59b0b2b3dc9ad944b3a66f13986cf51db5ea463
Status: Downloaded newer image for golang:latest
 ---> 124e2127157f
Step 1 : ADD . /go/src/github.com/craignicholson/simplehttp
 ---> 6c042b81d109
Removing intermediate container 5e9a12a558c3
Step 2 : RUN go install github.com/craignicholson/simplehttp
 ---> Running in 1191d0d8b8a7
 ---> b30d041310ce
Removing intermediate container 1191d0d8b8a7
Step 3 : ENTRYPOINT /go/bin/simplehttp
 ---> Running in a16b60af5b49
 ---> a0332a7b4c0a
Removing intermediate container a16b60af5b49
Step 4 : EXPOSE 8080
 ---> Running in 01a6379a6c16
 ---> 58317fddc39b
Removing intermediate container 01a6379a6c16
Successfully built 58317fddc39b
```

## Check to see the image we just built

```
$ docker images
REPOSITORY                 TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
cnicholson/simplehttp      1.0                 58317fddc39b        57 seconds ago      523.1 MB

```

## Run the image
Run the containter as a daemon
* -d, run headless, as a daemon
* -p, map the local ports to the container's ports
* Run the containter as a daemon
* repository/imagename:version

```

$ docker run -d -p 6060:8080 cnicholson/simplehttp:1.0

```
## Test the url on you local environment
Test the url on your local system
http://127.0.0.1:6060/gophers

Stop the container

```

$ docker ps
CONTAINER ID        IMAGE                       COMMAND                CREATED             STATUS              PORTS                    NAMES
4d39716a3281        cnicholson/simplehttp:1.0   "/bin/sh -c /go/bin/   About an hour ago   Up About an hour    0.0.0.0:6086->8080/tcp   reverent_perlman    
$ docker stop 4d39716a3281
4d39716a3281

```

## Docker Hub repository

## Deploy the container to Google Compute Engine






