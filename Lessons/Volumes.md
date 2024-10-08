<!-- Run as a slideshow: reveal-md README.md -w -->
# 🐳 Docker Volumes

⭐️ **GOAL**: Learn about all the different ways you can persist data using Docker --- then try it on your own server!

<!-- omit in toc -->
## ⏰ Agenda

1. [[**15m**] Warm Up: Persistence & You](#%5B%2a%2a15m%2a%2a%5D-warm-up%3A-persistence-%26-you)
1. [[**10m**] Discuss: Persistence & You](#%5B%2a%2a10m%2a%2a%5D-discuss%3A-persistence-%26-you)
1. [[**30m**] TT: Docker Volumes](#%5B%2a%2a30m%2a%2a%5D-tt%3A-docker-volumes)
   1. [Getting to Know the Docker Filesystem](#getting-to-know-the-docker-filesystem)
   1. [Saving Data in the Filesystem](#saving-data-in-the-filesystem)
      1. [Say Hello to Docker Volumes](#say-hello-to-docker-volumes)
      1. [Creating a Volume](#creating-a-volume)
   1. [Final Tips & Tricks: Permissions](#final-tips-%26-tricks%3A-permissions)
      1. [Doesn't Work](#doesn%27t-work)
      1. [Works Great](#works-great)
      1. [Here's Why](#here%27s-why)
1. [[**10m**] BREAK](#%5B%2a%2a10m%2a%2a%5D-break)
1. [[**15m**] Instructor Does: Install MongoDB on CapRover](#%5B%2a%2a15m%2a%2a%5D-instructor-does%3A-install-mongodb-on-caprover)
1. [[**15m**] Students Do: Install MongoDB on CapRover](#%5B%2a%2a15m%2a%2a%5D-students-do%3A-install-mongodb-on-caprover)

<!-- > -->

<!-- omit in toc -->
## 🏆 Objectives

1. Define _volume_ in the context of Docker.
1. Determine when and when not to persist data in your production-ready applications.
1. Practice persisting data using CapRover.

<!-- > -->

## [**15m**] Warm Up: Persistence & You

Then, we'll break out into groups to discuss the following questions together:

- **What is _data persistence_?**
- Name **at least use cases** where data persistence is necessary.
- Name **at least two examples** of persistent data storage.
- Have you ever had any **scenarios where your data did not persist** the way you wanted it to? **How did you fix it?**

Be sure to write down your answers --- we'll discuss after the activity!

<!-- > -->


## [**10m**] Discuss: Persistence & You

Call on members from each group and discuss answers together as a class.

<!-- > -->

## [**30m**] TT: Docker Volumes

Docker Volumes can be confusing! The remedy? Learning about how container filesystems work!

### Getting to Know the Docker Filesystem

1. Docker images are stored as a series of read-only layers.
2. Starting a container takes the read-only image and adds a read-write layer on top!
3. When the container modifies an existing file, the file is copied out of the read-only layer and into the read-write layer.
4. The version in the read-write layer only hides the underlying file in the read-only layer. It doesn't destroy it, and it still exists in the read-only layer.
5. When a container is deleted, relaunching the image starts a fresh container without any of the changes made in the previously running container --- those changes are lost!

**Docker calls this layering technique the [Union File System](https://docs.docker.com/glossary/?term=Union%20file%20system)**

### Saving Data in the Filesystem

Keen eyes will notice that the fifth step "resets" the container! How do we prevent that, and actually persist our data?

#### Say Hello to Docker Volumes

Volumes are:

- Directories that live outside the default Union File System.
- Exist as normal directories and files on the host filesystem!

That's it! Docker Volumes are directories that live on your local machine. A container can talk to them, even if that container has been deleted before!

#### Creating a Volume

There are many different ways to create a volume.

The **most direct way** is to **declare a volume at runtime using the `-v` flag**:

```bash
$ docker run -it --name vol-test -h CONTAINER -v /data debian /bin/bash
root@CONTAINER:/# ls /data
root@CONTAINER:/#
```

This command makes the directory `/data` inside the container live outside the Union File System and directly accessible on your host machine.

Any files that the container image held inside `/data` will be copied into the volume when the container starts.

You can find out where the volume lives on the host by using `docker inspect` at the terminal, like so (_you may need to `brew install jq` first_!). You'll see output on your terminal similar to the following...

```bash
$ docker inspect -f "{{json .Mounts}}" vol-test | jq .
[
  {
    "Name": "8e0b3a9d5c544b63a0bbaa788a250e6f4592d81c089f9221108379fd7e5ed017",
    "Source": "/var/lib/docker/volumes/8e0b3a9d5c544b63a0bbaa788a250e6f4592d81c089f9221108379fd7e5ed017/_data",
    "Destination": "/data",
    "Driver": "local",
    "Mode": "",
    "RW": true,
    "Propagation": ""
  }
]
```

We can also get similar info from the `docker volume inspect` command, now that we know that the name of the volume is `8e0b3a9d5c544b63a0bbaa788a250e6f4592d81c089f9221108379fd7e5ed017`:

```bash
$ docker volume inspect 8e0b3a9d5c544b63a0bbaa788a250e6f4592d81c089f9221108379fd7e5ed017
[
    {
        "Name": "8e0b3a9d5c544b63a0bbaa788a250e6f4592d81c089f9221108379fd7e5ed017",
        "Driver": "local",
        "Mountpoint": "/var/lib/docker/volumes/8e0b3a9d5c544b63a0bbaa788a250e6f4592d81c089f9221108379fd7e5ed017/_data",
        "Labels": null,
        "Scope": "local"
    }
]
```

In both cases, the output tells us that Docker has mounted `/data` inside the container as a directory somewhere under `/var/lib/docker`.

You can also create volumes inside of a `Dockerfile`:

```Dockerfile
FROM debian:wheezy
VOLUME /data
```

Or using the `docker volume create` command:

```bash
$ docker volume create --name my-vol
my-vol
```

There is another major use case for volumes that can only be accomplished through the `-v` flag: mounting a specific directory from the host into a container.

**Example**:

```bash
$ docker run -v /Users/dani/data:/data debian ls /data
```

**What do you think this command does?**

This command mounts the directory `/Users/dani/data` on the host as `/data` inside the container.

Any files already existing in the `/Users/dani/data` directory will be available inside the container.

Why is this useful? For sharing files between the host and the container, for example mounting source code to be compiled.

Note that the host directory for a volume cannot be specified from a `Dockerfile`, in order to preserve portability (the host directory may not be available on all systems). When this form of the `-v` argument is used any files in the image under the directory are not copied into the volume. Such volumes are not “managed” by Docker as per the previous examples — they will not appear in the output of `docker volume ls` and will never be deleted by the Docker daemon.

<!-- > -->

### Tips & Tricks: Smaller Docker Images

Use the popular [slim](https://github.com/slimtoolkit/slim?tab=readme-ov-file#demo-steps) package to build a compressed image for distrubution to Docker Hub or to deploy on your CapRover server.

### Tips & Tricks: Permissions

Often you'll need to set permissions and ownership on a volume, or initialize a volume with some default data or config files. It's important to be aware that anything after the `VOLUME` instruction in a `Dockerfile` will not be able to make changes to that volume!

<!-- > -->

#### Doesn't Work

Now that you know the above trick, why do you think the following `Dockerfile`  wouldn't work as expected?

```Dockerfile
FROM debian:wheezy
RUN useradd foo
VOLUME /data
RUN touch /data/x
RUN chown -R foo:foo /data
```

<!-- > -->

#### Works Great

To contrast, why do you think the following `Dockerfile` will work?

```Dockerfile
FROM debian:wheezy
RUN useradd foo
RUN mkdir /data &amp;&amp; touch /data/x
RUN chown -R foo:foo /data
VOLUME /data
```

<!-- > -->

#### Here's Why

**Docker is clever enough to copy any files that exist in the image under the volume mount into the volume and set the ownership correctly**. This won’t happen if you specify a host directory for the volume (so that host files aren’t accidentally overwritten).

If you can’t set permissions and ownership in a `RUN` command, you will have to do so using a `CMD` or `ENTRYPOINT` script that runs when the container is started.

<!-- > -->

## [**10m**] BREAK

<!-- > -->

## [**15m**] Instructor Does: Install MongoDB on CapRover

The instructor will demonstrate how to install MongoDB on CapRover.

**PROTIPS**:

- The MongoDB docker image uses volumes to store data that persists even if the server restarts.
- One database server is meant to serve many applications
- Do not create a new database server each time you create a new app; use the existing one.

<!-- > -->

## [**15m**] Students Do: Install MongoDB on CapRover

Following the demonstration, install MongoDB on your own CapRover instance.

1. Write down the **connection string** that is provided at the end of setup. Not sure what a **connection string** is? Investigate with your breakout buddies!
1. What **changes** would you need to make to an application in order to **use this connection string in production**?

<!-- > -->

<!-- omit in toc -->
## 📚 Resources & Credits

- https://docs.docker.com/storage/volumes/
- https://docs.docker.com/storage/bind-mounts/
