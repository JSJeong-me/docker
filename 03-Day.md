
# Docker Compose

도커를 커맨드라인에서 명령어로 작업은 간단한 작업만 했기 때문에 명령이 길지 않지만 컨테이너 조합이 많아지고 여러가지 설정이 추가되면 명령어가 금방 복잡해집니다. 그럼 도커 컴포즈에 대해 기본적인 사용법을 실습합니다.

## Task 1. Docker Compose 기본 실습

도커는 복잡한 설정을 쉽게 관리하기 위해 YAML방식의 설정파일을 이용한 Docker Compose라는 툴을 제공합니다. 깊게 파고들면 은근 기능이 많고 복잡한데 이번에는 아주 가볍게 다루고 지나가도록 하겠습니다.


### YAML

[YAML Syntax](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html)

### 기본 명령어

| 명령어  |  설명  |
|---|---|
| up | 시작 |
| stop | 중지 |
| down | 중지 후 삭제 |
| ps | 목록 |
| logs | 로그보기 |

### 도커 컴포즈 실습

`ex-03`라는 폴더를 만듭니다.



**wordpress 생성**

ex-03/wordpress/docker-compose.yml

```yml
version: '3'
services:
  wordpress:
    image: wordpress
    environment:
      WORDPRESS_DB_HOST: mysql
      WORDPRESS_DB_NAME: wp
      WORDPRESS_DB_USER: wp
      WORDPRESS_DB_PASSWORD: wp
    ports:
      - "8000:80"
    restart: always
  mysql:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: wp
      MYSQL_DATABASE: wp
      MYSQL_USER: wp
      MYSQL_PASSWORD: wp
```

```
docker-compose up -d
```

**목록**

```
docker-compose ps
```

**로그**

```
docker-compose logs -f
```

**종료 후 제거**

```
docker-compose down
```


### Python 실습
### https://docs.docker.com/language/python/build-images/

