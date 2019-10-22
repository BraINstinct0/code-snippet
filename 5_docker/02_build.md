## Docker File Build
> docker file: http://pyrasis.com/book/DockerForTheReallyImpatient/Chapter04/02
> build: http://pyrasis.com/book/DockerForTheReallyImpatient/Chapter04/03

#### 05. Dockerfile 작성
- directory를 만들고 그 안에 dockerfile 작성  
```bash
$ mkdir example
$ cd example
```

- dockerfile 내용
```
FROM ubuntu:16.04
MAINTAINER Jehyun Lee <jehyun.lee@gmail.com>

RUN apt-get update
RUN apt-get install --allow-unauthenticated -y nginx
RUN echo "\ndaemon off;" >> /etc/nginx/nginx.conf
RUN chown -R www-data:www-data /var/lib/enginx

VOLUME ["/data", "/etc/nginx/site-enabled", "/var/log/nginx"]

WORKDIR /etc/nginx

CMD ["nginx"]

EXPOSE 80
EXPOSE 443
```
* FROM: `[이미지 이름]:[태그]` 형식. 어떤 이미지를 기반으로 할지 설정.
* MAINTAINER: 메인테이너 정보
* RUN: Shell script나 명령을 실행  
  * 이미지 생성 중에는 입력을 받을 수 없으므로 `apt-get install`에 `-y` 옵션 포함.  
  * 나머지는 `nginx`설정
* VOLUME: 호스트와 공유할 디렉토리 목록. `run`에서 디렉토리 연결 설정 가능 [Link](https://github.com/jehyunlee/code-snippet/blob/master/5_docker/01_basic.md#04-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88-%EC%83%9D%EC%84%B1-docker-run)
* CMD: 컨테이너가 시작되었을 때 실행할 파일 또는 셸 스크립트.  
* WORKDIR: CMD에서 설정한 실행 파일이 실행될 디렉토리.  
* EXPOSE: 호스트와 연결할 포트.  

#### 06. Dockerfile Build: 이미지 생성
- directory 밖에서 build 실행.  
```bash
$ docker build example --tag hello:0.1.
```
- `docker build [옵션] [dockerfile 경로]` 형식.  
  - `--tag`형식으로 `[이미지이름]:[태그]` 설정 가능. `[이미지이름]`만 하면 `[태그]`는 자동으로 `latest`로 설정됨.  

- 이미지 목록 출력.  
```bash
$ docker images
```

- 방금 만든 `hello:0.1` 이미지 실행.  
```bash
$ docker run --name hello-nginx -d -p 80:80 -v /root/data:/data hello:0.1
```