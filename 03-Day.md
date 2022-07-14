
# Docker Compose

도커를 커맨드라인에서 명령어 작업은 간단한 작업만 했기 때문에 명령이 길지 않지만 컨테이너 조합이 많아지고 여러가지 설정이 추가되면 명령어가 금방 복잡해집니다. 그럼 도커 컴포즈에 대해 기본적인 사용법을 실습.


### Docker Compose 란?

    Docker Compose란 여러 개의 컨테이너들을 관리, 실행하기 위한 "툴"로, 각각 독립된 컨테이너의 실행을 정의합니다.




도커는 복잡한 설정을 쉽게 관리하기 위해 YAML방식의 설정파일을 이용한 Docker Compose라는 툴을 제공합니다. 깊게 파고들면 은근 기능이 많고 복잡한데 간단한 기능만 다루어 보겠음.


### YAML

    여러 컨테이너들을 한 번에 관리를 할 수 있게 도와주는 역할을 함

### Docker Compose 작성하기

    docker-compose.yml로 작성하여, 실행할 수 있습니다.
    프로젝트 루트에 파일을 만들고, 실행 설정을 적어둡니다.
    

### 기본구조

    version: "3"
    services:
        redis:
            # 레디스 설정
        db:
            # 데이터베이스 설정
        web:
            # 웹 애플리케이션 설정
    networks:
        # 네트워크 설정
    volumnes:
        #볼륨 설정



[YAML Syntax](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html)


### Compose file versions and upgrading

    https://docs.docker.com/compose/compose-file/compose-versioning/


### 컨테이너를 올리기 위한 명령어와 docker-compose로 작성 비교
```
    $ docker run -d -p 8080:80 \
      --link mysql:mysql \
      -e WORDPRESS_DB_HOST=mysql \
      -e WORDPRESS_DB_NAME=wp \
      -e WORDPRESS_DB_USER=wp \
      -e WORDPRESS_DB_PASSWORD=wp \
      wordpress
```

```
    version: '2'

    services:
        db:
            image: mysql:5.7
            volumnes:
                - db_data:/var/lib/mysql
            restart: always
            environment:
                MYSQL_ROOT_PASSWORD: wordpress
                MYSQL_DATABSE: wordpress
                MYSQL_USER: wordpress
                MYSQL_PASSWORD: wordpress

        wordpress:
            depends_on:
                - db
            image: wordpress:latest
            volumnes:
                - wp_data:/var/www/html
            ports:
                - "8000:80"
            restart: always
            environment:
                WORDPRESS_DB_HOST: db:3306
                WORDPRESS_DB_PASSWORD: wordpress
    volumnes:
        db_data:
        wp_data:
```


### 기본 명령어

| 명령어  |  설명  |
|---|---|
| up | 시작 |
| stop | 중지 |
| down | 중지 후 삭제 |
| ps | 목록 |
| logs | 로그보기 |


## Task 1. Docker Compose 기본 실습

```
  mysql 실행 docker-compose.yml 파일


  version: '3'
  services:
    mysql:
      image: mysql:5.7
      restart: always
      environment:
        MYSQL_ROOT_PASSWORD: wp
        MYSQL_DATABASE: wp
        MYSQL_USER: wp
        MYSQL_PASSWORD: wp


   docker-compose up 명령어를 통해 컨테이너를 생성 및 실행

    docker-compose up -d
```

### Task 2. Docker Compose 기본 실습


**wordpress 생성**

./wordpress/docker-compose.yml

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

### up

    서비스를 띄울 네트워크 설정
    필요한 볼륨 생성(혹은 이미 존재하는 볼륨과 연결)
    필요한 이미지 풀(pull)
    필요한 이미지 빌드(build)
    서비스 의존성에 따라 서비스 실행
    
    options
        -d: 서비스 백그라운드로 실행. (docker run에서의 -d와 같음)
        --force-recreate: 컨테이너를 지우고 새로 생성.
        --build: 서비스 시작 전 이미지를 새로 생성
        -f: 기본으로 제공하는 docker-compose.yml이 아닌 별도의 파일명을 실행할 때 사용
        docker-compose -f docker-compose.yml -f docker-compose-test.yml up 형태로 두 개의 파일 실행도 가능
        YAML을 두 개 이상 설정할 경우 앞에 있는 설정보다 뒤에 있는 파일이 우선
        
        
### down

    실행 중인 서비스를 삭제합니다.
    컨테이너와 네트워크를 삭제하며, 옵션에 따라 볼륨도 같이 삭제할 수 있습니다.
    
    options
        -v, --volume: 볼륨까지 같이 삭제
        DB 데이터 초기화하는데 용이함
        모든 설정을 초기화하고 새로 시작하는 데 사용

### ps

    현재 환경에서 실행 중인 각 서비스의 상태를 표시합니다.
    
### logs

    output으로 나온 log들을 확인할 때 사용합니다.
    docker의 logs와 마찬가지로 --follow(혹은 -f)를 하여 실시간으로 나오는 로그 확인이 가능합니다.
    
    options
        -f, --follow: 실시간 로그 출력
        
### config

    docker-compose에 최종적으로 적용된 설정을 보여줍니다.
    docker-compose -f docker-compose.yml -f docker-compose-test.yml up 형태처럼 여러 개의 YAML파일을 실행시켰을 경우
    최종적으로 적용된 설정을 확인하기 좋은 옵션입니다.
    
```

### 단일 명령을 사용하여 모두 실행 또는 종료
```
    docker-compose up이라는 명령어 하나로 문서에 정의한 서비스들이 한꺼번에 컨테이너로 실행되고
    반대로 docker-compose down이라는 명령어로 정의한 모든 서비스를 내릴 수도 있습니다.
    물론 먼저 실행되어야 하는 순서까지 지정이 가능합니다.
    이렇게 단일 명령을 사용하여 편하게 서비스를 실행 또는 종료시킬 수 있는 장점이 있습니다.
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


    $ sudo apt install python3-pip

    $ pip3 install Flask

    $ pip3 freeze | grep Flask >> requirements.txt

    $ touch app.py

    $ python3 -m flask run



