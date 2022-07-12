
### 도커 네트워크 구조

![다운로드](https://user-images.githubusercontent.com/54794815/178488864-5d80e34a-3d08-40f1-a3e5-409f32d24fd3.png)





    - 도커는 컨테이너에 내부 IP를 순차적으로 할당

    - 내부 IP는 컨테이너를 재시작할 때마다 변경될 수 있음

    - 내부 IP는 도커가 설치된 호스트, 즉 내부망에서만 쓸 수 있는 IP이므로 외부와 연결될 필요가 있음

    - 컨테이너를 시작할 때마다 호스트에 veth(Virtual Ethernet) 라는 네트워크 인터페이스를 생성

    - 도커는 각 컨테이너에 외부와의 네트워크를 제공하기 위해 컨테이너마다 가상 네트워크 인터페이스를 호스트에 생성하며 이 인터페이스의 이름은 veth로 시작

    - veth 인터페이스는 사용자가 직접 생성할 필요는 없으며 컨테이너가 생성될 때 도커 엔진이 자동으로 생성

    - veth 인터페이스는 호스트가 갖고 있는 eth0, eth1 등과 연결되어 있음

    - docker0 브리지는 각 veth 인터페이스와 바인딩돼 호스트의 eth0 인터페이스와 이어주는 역할
    
###


### Network Create - Wordpress 실습

1. MySQL

    $ docker pull mysql:5.7
    
    $ docker run -d -p 3306:3306 \
    
      -e MYSQL_ALLOW_EMPTY_PASSWORD=true \
      
      --name mysql \
      
      mysql:5.7

2. DATABASE 생성

    $ docker exec -it mysql mysql

    sql> create database wp CHARACTER SET utf8;
    
    sql> grant all privileges on wp.* to wp@'%' identified by 'wp';
    
    sql> flush privileges;
    
    sql> show DATABASES;
    
    sql> quit
    
3. network create(네트워크 생성)

    $ docker network create app-network

    $ docker network connect app-network mysql

    $ docker run -d -p 8080:80 \
    
    --network=app-network \
    
    -e WORDPRESS_DB_HOST=mysql \
    
    -e WORDPRESS_DB_NAME=wp \
    
    -e WORDPRESS_DB_USER=wp \
    
    -e WORDPRESS_DB_PASSWORD=wp \
    
    wordpress




    
### docker network 확인

    $ docker network ls
    
    
### docker network 상세 정보 확인

    $ docker network inspect bridge

    $ docker network inspect app-network



### client & server process구성확인

    $ docke version

    $ ps -aef




###




    
    




























Docker commit
```
$ docker run -it ubuntu:16.04 bash
# apt-get update
# apt-get install -y git
# git version
```

```
$ docker ps
$ docker diff <CONTAINER_ID>
$ docker commit <CONTAINER_ID> ubunut:git
$ docker run -it ubuntu:git bash
# git
```

Dockerfile
```
FROM ubuntu:16.04

RUN apt-get update
RUN apt-get install -y git
```

```
$ docker build -t ubuntu:git02 .
$ docker run -it ubuntu:git02 bash
# git version
```
