
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



### 브리지 네트워크(Bridge Network)

 

    - docker0이 아닌 사용자 정의 브리지를 새로 생성해 각 컨테이너에 연결하는 네트워크 구조

    - 컨테이너는 연결된 브리지를 통해 외부와 통신
    
    




### tensorflow

    docker run -p 10000:8888 jupyter/scipy-notebook:0fd03d9356de
    
    https://jupyter-docker-stacks.readthedocs.io/en/latest/

    tensorflow는 python으로 만들어져 python과 관련 패키지를 설치해야 합니다. 이번에 설치하는 이미지는 python과 함께 numpy, scipy, pandas, jupyter, scikit-learn, gensim, BeautifulSoup4, Tensorflow가 설치되어 있습니다. 뭔가 복잡해 보이지만 도커라면 손쉽게 실행
    
    
    
    $ docker run -d -p 8888:8888 -p 6006:6006 teamlab/pydata-tensorflow:0.1


### Jupyter 노트북 실행 --->    http://localhost:8888/


### tf_test.py

    import tensorflow as tf

    hello = tf.constant('Welcome,Tensorflow!!')
    sess = tf.Session()
    sess.run(hello)

###






















Docker commit
```
$ docker run -it ubuntu:20.04 bash
# apt-get update
# apt-get install -y net-tools
# ifconfig
```

```
$ docker ps
$ docker diff <CONTAINER_ID>
$ docker commit <CONTAINER_ID> ubunut:net
$ docker run -it ubuntu:net bash
# ifconfig
```

###

![제목 없음](https://user-images.githubusercontent.com/54794815/178501825-207e4f0e-385d-4849-9163-952469e2739b.png)



###





### Dockerfile
```
FROM ubuntu:20.04

RUN apt-get update
RUN apt-get install -y net-tools
```

```
$ docker build -t ubuntu:net02 .
$ docker run -it ubuntu:net02 bash
# ifconfig
```


### https://hub.docker.com/ 에서 계정을 생성합니다.
### 로그인을 하고, 첫 화면의 오른쪽 위에서 Create Repository +를 선택합니다.
### 이미지 이름과 설명을 입력합니다. my_ubuntu 이미지를 생성합니다.


    $ docker login

    $ docker tag ubuntu:net02 drjsjeong/NAME:<tag>

    $ docker push [OPTIONS] NAME[:TAG]

