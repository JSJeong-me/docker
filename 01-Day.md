# Docker

도커에 대해 기본적인 사용법을 실습합니다.

## Task 1. Docker 기본 실습

### 기본 명령어

| 명령어  |  설명  |
|---|---|
| run | 컨테이너 실행하기 |
| ps | 컨테이너 목록 확인하기 |
| stop | 컨테이너 중지하기 |
| rm | 컨테이너 제거하기 |
| logs | 컨테이너 로그보기 |
| images | 이미지 목록 확인하기 |
| pull | 이미지 다운로드하기 |
| rmi | 이미지 삭제하기 |

### 컨테이너 생성 실습

**ubuntu 컨테이너 생성**




```
docker run --rm -it ubuntu:20.04 /bin/sh
```

**간단한 웹 애플리케이션 생성**

```
docker run -d -p 4567:4567 subicura/docker-workshop-app:1
```

http://localhost:4567 접속

**MySQL 생성**

```
docker pull mysql:5.7

docker run -d -p 3306:3306 \
  -e MYSQL_ALLOW_EMPTY_PASSWORD=true \
  --name mysql \
  mysql:5.7
```

Database 생성

```
docker run -d --name wordpressdb \
-e MYSQL_ROOT_PASSWORD=pw \
-e MYSQL_DATABASE=wordpress mysql:5.7
```

**Wordpress 생성**

```
docker run -d -e WORDPRESS_DB_PASSWORD=pw \
-e WORDPRESS_DB_USER=root --name wordpress \
--link wordpressdb:mysql -p 80 wordpress
```

http://127.0.0.1:52841 접속  $ docker ps 로 확인

**컨테이너 목록 확인**

```
docker ps -a
```

**컨테이너 로그**

```
docker logs xxx
```

**컨테이너 종료**

```
docker stop xxx
```

**컨테이너 삭제**

```
docker rm xxx
```

**이미지 목록 확인**

```
docker images
```

**네트워크 생성**

```
docker network create app-network
```

**네트워크에 연결된 컨테이너 생성**

```
docker run -d \
  -e MYSQL_ALLOW_EMPTY_PASSWORD=true \
  --name mysql \
  --network=app-network \
  mysql:5.7
```

```
docker exec -it mysql mysql
create database wp CHARACTER SET utf8;
grant all privileges on wp.* to wp@'%' identified by 'wp';
flush privileges;
quit
```

```
docker run -d -p 8000:80 \
  --network=app-network \
  -e WORDPRESS_DB_HOST=mysql \
  -e WORDPRESS_DB_NAME=wp \
  -e WORDPRESS_DB_USER=wp \
  -e WORDPRESS_DB_PASSWORD=wp \
  wordpress
```

### Container Volume Mount


  볼륨 활용방법
  
  볼륨의 활용방법은 크게 두 가지로 나눌 수 있습니다.
  1. 호스트와 볼륨을 공유
  2. 볼륨 컨테이너 활용
  
  호스트와 볼륨을 공유
  호스트와 볼륨을 공유하는 방식의 경우 호스트의 공유 디렉토리와 컨테이너에서의 공유 디렉토리 경로를 지정하는 옵션을 입력함으로서 가능합니다.

  
```
  $ docker run -d -p 8080:80 --name my-nginx-web -v ~/Desktop/htdocs:/usr/share/nginx/html nginx
```

  -v  옵션이 새로 추가되었는데요, ':' 를 구분자로 왼쪽이 호스트, 오른쪽이 컨테이너의 디렉토리를 의미
  이 두개 경로를 공유함으로써 httpd container는 종료되더라도 데이터를 호스트  ~/Desktop/htdocs 볼륨을 저장


    볼륨 컨테이너 활용

    볼륨을 사용하는 또 다른 방법으로는 볼륨컨테이너 를 활용하는 것인데요, 컨테이너를 생성할 때 --volumes-from 옵션을 설정하면 -v or --volume 옵션을 적용한 컨테이너의 볼륨 디렉토리를 공유

```
  $ docker run -i -t --name volume_container --volumes-from volume_override ubuntu:14.04
```

  위 예시는 volume_override 이름으로 만들어 진 컨테이너에서 사용하고 있는 볼륨을 volume_container 이름의 컨테이너에서 다시 재 활용 하는 방식


```
docker stop xxx
docker rm xxx
docker system prune -a
```

