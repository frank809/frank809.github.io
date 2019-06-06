---
layout: post
title: 在jenkins container里面运行docker 
---

>
    Docker里面的Docker


Jenkins运行在docker container里面是一个非常方便的用法。在docker pipeline用法中需要在Jenkins的job中运行docker container。 所以需要在jenkins的container里面运行docker命令。
运行jenkins container的时候需要映射宿主机的Docker文件使用下面的命令运行：
![netlayer]({{ site.url }}/images//docker1.jpg)

``` sh
docker run --restart=always -d --name jenkins_docker -p 8080:8080 -p 50000:50000 -v /home/frank/docker/jenkins:/var/jenkins_home -v /usr/bin/docker:/usr/bin/docker -v /var/run/docker.sock:/var/run/docker.sock jenkins/jenkins:lts
```
在Jenkins container运行成功以后，：
需要使用root用户修改。
``` sh
docker exec -it --user root jenkins_docker bash
```
需要配置对应的docker 用户组和赋权限：
![netlayer]({{ site.url }}/images//docker2.jpg)
``` sh
groupadd docker  # 创建Docker用户组，在jenkins的container里面不含docker组。
gpasswd -a jenkins docker # 添加jenkins用户到docker组中。
groupmod -g $(stat -c "%g" /var/run/docker.sock) docker   # 修改Docker用户组的Gid为将当前docker.sock的
```

重启jenkins container：
``` sh
docker restart jenkins_docker
```

Jenkins job测试下：
``` sh
pipeline {
    agent none 
    stages {
        stage('Example Test') {
            agent { docker 'ubuntu:latest' } 
            steps {
                echo 'Hello, JDK'
                sh 'ps aux'
            }
        }
    }
}
```
![netlayer]({{ site.url }}/images//docker3.jpg)
