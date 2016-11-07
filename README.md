# Dubbo Demo from Docker

#### Maven build
```shell
mvn clean package -P release
```

#### Dockerfile
```Dockerfile
FROM java:8u92-alpine

RUN apk update && apk add socat

ADD ruleng_linux_amd64.tar.gz /

COPY entrypoint.sh /

ADD dubbo-test-webapp/target/dubbo-test-webapp-0.0.1-SNAPSHOT.tar.gz /

EXPOSE 20880

CMD ["/bin/sh", "-c", "/entrypoint.sh"]
``` 

#### Build container
```shell
docker build -t riveryang/dubbo-test .
```

#### Run Dubbo Demo
```shell
# set HOSTIP env
HOSTIP=`ifconfig eth0 |grep "inet addr"| cut -f 2 -d ":"|cut -f 1 -d " "`
# or
HOSTIP=192.168.8.158

# Run container
docker run -it --rm -P -v /var/run/docker.sock:/var/run/docker.sock -e PROXY_HOST=$HOSTIP riveryang/dubbo-test
```

#### Registry
	e.g. URL:
	dubbo://192.168.8.158:32798/org.dubbo.demo.service.HelloWorldService?.....
