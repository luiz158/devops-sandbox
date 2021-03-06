# Docker Toolbox

Docker for Windows provides native support for Windows built-in Hyper-V virtual machine. But for some ledacy systems, or in some cases, you can not give up VirtualBox, you could have to use Docker Toolbox.

Docker Toolbox supports Windows and Mac OS system, which ship with a Boot2Docker image, and run docker in VirtualBox. In your system, the `docker` command is connecting to the hosted docker daemon in the virtual machine.

## Docker machine

Docker Toolbox includes a series of tools, one is `docker-machine` which is use for creating new virtual machine.

Start up the Toolbox Quickstart shell prompt. By default, it will try to start up the default machine if it exists or create one for you.

```
docker-machine create -d virtualbox --engine-registry-mirror https://docker.mirrors.ustc.edu.cn meandev 
```

For all options, `docker-machine help` to get detailed help.

Now connect your shell to the new created machine.

```
eval "($docker-machine env meandev)"
```

Verify if you have selected the created machine.

```
$ docker-machine ls
NAME      ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER        ERRORS
default   -        virtualbox   Stopped                                       Unknown
meandev   *        virtualbox   Running   tcp://192.168.99.101:2376           v17.03.0-ce
```

Check the machine environment.

```
docker-machine env meandev
```

You will see the following info.

```
$ docker-machine env meandev
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.99.101:2376"
export DOCKER_CERT_PATH="E:\Users\hantsy\.docker\machine\machines\meandev"
export DOCKER_MACHINE_NAME="meandev"
export COMPOSE_CONVERT_WINDOWS_PATHS="true"
# Run this command to configure your shell:
# eval $("C:\Program Files\Docker Toolbox\docker-machine.exe" env meandev)
```

## Troubleshoots

### Port forwarding to localhost

Under Windows, when use Docker Toolbox and Virtualbox, you can not access the server in docker via *localhost*. You have to expose the virtualbox ports to host machine. 

Make sure the machine is stop. 

```
VBoxManage modifyvm "meandev" --natpf1 "tcp-port3306,tcp,,3306,,3306"
VBoxManage modifyvm "meandev" --natpf1 "udp-port3306,udp,,3306,,3306"
```

*meandev* is the machine name.

### Configure docker registry mirror

Docker pull is very slow and periodical breaking in China, luckily there are some companies or organisations provide docker hub mirror serivce.

* [NetEase][163]
* [DaoCloud][daocloud]
* [AliYun][alicloud]
* [USTC][ustc]

Helpful docs to configure docker hub mirrors.

* [国内 docker 镜像比较](http://www.datastart.cn/tech/2016/09/28/docker-mirror.html)
* [Docker 使用帮助](https://lug.ustc.edu.cn/wiki/mirrors/help/docker)
* [DockerHub 镜像加速](https://c.163.com/wiki/index.php?title=DockerHub%E9%95%9C%E5%83%8F%E5%8A%A0%E9%80%9F)

[163]:https://c.163.com/
[daocloud]:https://daocloud.io
[alicloud]:https://aliyuncs.com
[ustc]:https://ustc.edu.cn
