# Basic Docker command line

This is similar to: https://confluence.ris.wustl.edu/display/~mcallawa/Getting+Started+With+Docker

1. Start up Google Cloud Shell

2. Type `docker`

3. Grab an image from Docker Hub:
```
$ docker pull ubuntu:18.10
18.10: Pulling from library/ubuntu
72f45ff89b78: Pull complete
9f034a33b165: Pull complete
386fee7ab4d3: Pull complete
f941b4ac6aa8: Pull complete
Digest: sha256:ef5d78b10fcada65484b2dd9cd6e0704fba36021a101b2b52023a1566c4ebad4
Status: Downloaded newer image for ubuntu:18.10
```

4. List your local images:
```
docker images
```

5. Inspect the container:
```
$ docker inspect ubuntu:18.10
[
    {
        "Id": "sha256:7e10e8cb09ba03e392d4546a631a8479e193e2f8fb68c2f95405cf39f0a59322",
        "RepoTags": [
            "ubuntu:18.10"
        ],
        "RepoDigests": [
            "ubuntu@sha256:ef5d78b10fcada65484b2dd9cd6e0704fba36021a101b2b52023a1566c4ebad4"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2018-10-19T00:48:20.982051297Z",
        "Container": "d6d3c3f3d3e39266393709f540cb9d204566406795a28c61a2d6f0cbec62966b",
        "ContainerConfig": {
            "Hostname": "d6d3c3f3d3e3",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "CMD [\"/bin/bash\"]"
            ],
            "ArgsEscaped": true,
            "Image": "sha256:851cc5a1b4b2d566878c652e7ad2efa75d622aedfc58c66890e1199159def502",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {}
        },
        "DockerVersion": "17.06.2-ce",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/bash"
            ],
            "ArgsEscaped": true,
            "Image": "sha256:851cc5a1b4b2d566878c652e7ad2efa75d622aedfc58c66890e1199159def502",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": null
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 73681064,
        "VirtualSize": 73681064,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/569890a70ec7f5208e7e03f331d2aa89019ff9c30637ca54e943a41da9a36686/diff:/var/lib/docker/overlay2/06ea13088c37df363947f442742a46f7a03624c33755e2e0b173364ae9ba5a1d/diff:/var/lib/docker/overlay2/fcac075f0f78aa1c15100431f56f6745547a7366fdda481f324e6400c011d470/diff",
                "MergedDir": "/var/lib/docker/overlay2/9166e2e5345dcd69045a5ce54fa9dc93cb6fbc9b886d32393f680c21aa8fffbf/merged",
                "UpperDir": "/var/lib/docker/overlay2/9166e2e5345dcd69045a5ce54fa9dc93cb6fbc9b886d32393f680c21aa8fffbf/diff",
                "WorkDir": "/var/lib/docker/overlay2/9166e2e5345dcd69045a5ce54fa9dc93cb6fbc9b886d32393f680c21aa8fffbf/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:f8aa66f3fe0dc030ea829621cf583caf7d5f2b19aaf57ff55b9898f0a5c2dc94",
                "sha256:74d6dbd8585893791efffc6c39f53fb85d1648f92bd5c2f2e72b09b7a86d5b61",
                "sha256:337708522f4e4748322974b4bac333e61ce93f91b28e4e8be4e60a4819864cb4",
                "sha256:9865014da401028a08e41bda5ec077e0bb61846e393cfd750313fc4351cae54a"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]
```

6. Run an interactive container using that image:
```
$ docker run --it ubuntu:18.10
root@353cdb5ba500:/# whoami
root
root@353cdb5ba500:/# exit
```

7. Now you are back on the host machine. The container still exists though:
```
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS                     PORTS               NAMES
353cdb5ba500        ubuntu:18.10        "/bin/bash"         About a minute ago   Exited (0) 8 seconds ago                       boring_kowalevski
```

8. Remove the container:
```
$ docker rm 353cdb5ba500
```

8. Launch a container that cleans itself up when you exit:
```
$ docker run --rm -it ubuntu:18.10
exit
```

9. Install some software!
```
$ docker run -it ubuntu:18.10
root@cc17672ed879:/# apt-get install vim
root@cc17672ed879:/# apt-get update
root@cc17672ed879:/# apt-get install vim
root@cc17672ed879:/# exit
```

10. Commit your addition. Note that you shouldn't generally do this as your method to create images. Making Dockerfiles is the way to go for that. This is a nice example though.
$ docker commit 8957acebc6f4
sha256:dbd5a62e1861035c7f4438e6641f3d81f03818968d60713084f24b682dca8220
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
<none>              <none>              dbd5a62e1861        46 seconds ago      156MB
ubuntu              18.10               7e10e8cb09ba        12 days ago         73.7MB
```

11. You can make a tag
```
$ docker tag dbd5a62e1861 ernfrid/test:thing
```

12. Let's pretend we messed up
```
$ docker run -it ubuntu:18.10
root@49a72e568b5b:/# apt-get update
root@49a72e568b5b:/# apt-get install emacs
root@49a72e568b5b:/# exit
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                       PORTS               NAMES
49a72e568b5b        ubuntu:18.10        "/bin/bash"         2 minutes ago       Exited (127) 6 seconds ago                       elated_kare
8957acebc6f4        ubuntu:18.10        "/bin/bash"         11 minutes ago      Exited (0) 9 minutes ago                         nervous_roentgen
$ docker commit 49a72e568b5b
sha256:1922b6eb2e1341ea7658f4e0a2649970a4f9eaa0d9e8fdcecdc03552b20882a5
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
<none>              <none>              1922b6eb2e13        5 minutes ago       422MB
ernfrid/test        thing               dbd5a62e1861        15 minutes ago      156MB
ubuntu              18.10               7e10e8cb09ba        12 days ago         73.7MB
$ docker tag 1922b6eb2e13 ernfrid/test:thing
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ernfrid/test              thing              1922b6eb2e13        5 minutes ago       422MB
<none>        <none>               dbd5a62e1861        15 minutes ago      156MB
ubuntu              18.10               7e10e8cb09ba        12 days ago         73.7MB
```

TAGS CAN CHANGE!!!

9. Clean up your images (Note that quicker way to do this would be to run `docker system prune` - Tip courtesy of Susanna Kiwala):
```
$ docker rmi ubuntu:18.10
Untagged: ubuntu:18.10
Untagged: ubuntu@sha256:ef5d78b10fcada65484b2dd9cd6e0704fba36021a101b2b52023a1566c4ebad4
Deleted: sha256:7e10e8cb09ba03e392d4546a631a8479e193e2f8fb68c2f95405cf39f0a59322
Deleted: sha256:226e715931cfccf71b326052ece9d4804c553aa6537f72acd124504ee905ec74
Deleted: sha256:ddc270d77a832a21b72b8e5f3514f1afbbfc3124db02571543f94481f84a6837
Deleted: sha256:3117fee3851c7ff4e7cab0381d30943277e2a7f16454854814bd4308e2c1ed9d
Deleted: sha256:f8aa66f3fe0dc030ea829621cf583caf7d5f2b19aaf57ff55b9898f0a5c2dc94
$ docker rm 1922b6eb2e13
$ docker rm dbd5a62e1861
```

# Volumes

Volumes can be used to make data from the host system available inside your container AND/OR to let your container write to the disk of the host system.

```
$ docker run -v `pwd`:/hostdata --rm -it ubuntu:18.10
root@b378e98ddd94:/# ls /hostdata/
root@b378e98ddd94:/# touch /hostdata/made_in_container
root@b378e98ddd94:/# exit
$ ls
made_in_container
```

They can also be used on the cluster!

```
LSF_DOCKER_PRESERVE_ENVIRONMENT=false LSF_DOCKER_VOLUMES="/gscmnt/gc2801/analytics:/volume1 /gscmnt/temp403/systems:/volume2" bsub -Is -q research-hpc -a 'docker(registry.gsc.wustl.edu/genome/genome_perl_environment)' /bin/bash
```

# Ways in which our cluster implementation of Docker is going to mess you up!
1. You aren't root on the cluster!
Things like pyenv that are installed in a user directory by default, may not be accessible on the cluster. This is true of containers made by other people as well. They may assume the container will be run as root.

2. You can't install anything!
In a regular docker container, you can install software while it's running. This can't be done locally.

3. You can't use ports!
You may encounter applications that expect that you can expose ports inside the container (e.g. a web server of some sort). This is forbidden internally. You can't do it. It may be opened up later if/when disk permissions are tighter.

4. You'll see some funky errors about groups!
We talked about this already. It is OK that these happen. You don't need to have the group names to perform operations in the container. If you want these errors to go away, you need to include libnss-sss on Debian-based systems.

5. You'll inherit the local environment unless you turn it off!
This generally means that your PATH is set up wrong. You may see errors about software in /gsc/ not being available (e.g. grep). Reference by full path or use LSF_DOCKER_PRESERVE_ENVIRONMENT=false to ensure that you use the environment set by the container.

6. Even if PATH is set in your container. It may be cleared!
This is a corollary to #5 above. Even if you don't inherit the local environment, if you specify a login shell (bash -l) then anything added to PATH or other environment variables in the container, may be cleared out. Note that NOT running a login shell typically results in files that aren't group writable (at least for me. YMMV.)

# Tips discussed during the workshop
1. If you've pushed your container to a repo, then it gets a `RepoDigests` entry in the output of `docker inspect`. You can use this string to uniquely identify your container. This is more reproducible than a tag as tags can change and this hash does not.

2. You can use `docker history` to see commands run during the building of an image. This isn't guaranteed to give you enough information to make an identical container, but it may give you a hint as to what was run.
