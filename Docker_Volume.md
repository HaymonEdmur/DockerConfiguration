# Docker Volume 

---
## Mounting a native host directory as a volume in docker container
A native host directory containing files and folders can be mounted into a docker container. In container, this directory can be modified.
New files and folders can be added from container. Files and folders created from a container in this directory are retained after execution of image.

| Native Directory       | Volume within container  | Command line option  |
| ------------- |:-------------:| -----:|
| /usr/hemant/configuration | /config | -v /usr/hemant/configuration:/config  |

## Mounting a docker volume to a container 
Let us create a new volume
```
$ docker volume create configuration
configuration

$ docker volume ls
DRIVER              VOLUME NAME
local               configuration
local               oracle
```
The volume created in above command can be accessed on native file system. See our docker configuration 
>Local Docker configuration
```
$ cat /etc/docker/mydocker.json
{
   "pidfile"                 : "/var/run/docker.pid",
   "storage-driver"          : "overlay2",
   "data-root"               : "/var/DockerData",
   "ipv6"                    : false,
   "hosts"                   : ["tcp://0.0.0.0:2375", "unix:///var/run/docker.sock"]
}
```
See the directory for <code>configuration</code> volume 
```
$ sudo ls -l /var/DockerData/volumes/configuration/_data
total 0
```
We can populate above directory. Let us copy some files to above directory for <code>configuration</code> volume.
```
$ sudo cp /bin/a* /var/DockerData/volumes/configuration/_data
$ sudo ls -x /var/DockerData/volumes/configuration/_data
a2p        addr2line  alias   apropos  ar     arch  as  aserver  audit2allow  audit2why  aulast
aulastlog  ausyscall  auvirt  awk
```
# Mount <code>configuration</code> volume 
```
$ docker pull centos:latest
$ docker container run --rm -it -h dbserver -v configuration:/config centos
# ls /config
a2p        alias    ar    as       audit2allow  aulast     ausyscall  awk
addr2line  apropos  arch  aserver  audit2why    aulastlog  auvirt
```
>Volume Mounting
Document recomends --mount option
```
$ docker container run --rm -it -h dbserver --mount source=configuration,target=/config centos
```

