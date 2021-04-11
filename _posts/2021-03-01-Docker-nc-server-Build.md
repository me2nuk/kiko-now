---
layout: post
title:  "도커(docker)를 이용하여 nc 서버 구축하는법"
tags: [docker,pwnable]
excerpt: "도커를 이용하여 nc 서버 구축하기"
---

## 도커(docker)를 이용하여 nc 서버 구축하는법

+ ### 도커(docker)란?

    도커(docker)는 어플리케이션을 쉽게 구축하고 테스트등의 다양한 기능을 사용할 수 있는 플랫폼입니다.

    쉽게 말해서 도커로 한 운영체제안에 여러개의 운영체제를 쉽게 구축하고 배포할 수 있습니다.

    [docker 설치하는법(아직 작성중)]()

* * *

+ ### 도커파일 빌드

    + #### 도커파일이란?

        도커파일을 작성한 다음 빌드하게되면 도커 파일안에 있었던 설정대로 이미지가 만들어지게 됩니다.

        작성된 도커파일을 빌드하면 Dockerfile에 작성된 명령어들을 한줄씩 실행하는것을 실시간으로 볼 수 있습니다.

    + #### nc 서버를 구축하기 위한 도커파일 작성

        | 파일 이름 | 설명 | 
        | - | - |
        | Dockerfile | Dockerfile를 이용하여 앱의 환경을 정의합니다. |
        | docker-compose.yml | 격리 된 환경에서 함께 실행할 수 있도록 앱을 구성하는 서비스를 정의 합니다. |

        [도커파일 명령어는 공식 문서를 참고하면 됩니다.](https://docs.docker.com/reference/)

        + __Dockerfile__

            ```docker
            {% raw %}FROM ubuntu:16.04

            RUN apt-get update
            RUN apt-get upgrade -y
            RUN apt-get install -y xinetd
            ENV TERM=linux

            RUN useradd -m attack
            WORKDIR /home/attack

            ADD nctest /home/attack
            ADD attack /etc/xinetd.d

            RUN chmod 460 /home/attack/*
            RUN chown attack:root /home/attack/*
            RUN chmod +x /home/attack/nctest

            RUN echo "attack 10001/tcp" >> /etc/services

            CMD ["/usr/sbin/xinetd","-dontfork"]{% endraw %}
            ```

        + __docker-compose.yml__

            ```yml
            version: '2.2'

            services:
                    nctest:
                            build: ./
                            ports:
                                    - "10001:10001"
            ```

        + __attack__

            ```d
            service attack
            {
               disable      = no
               flags      = REUSE
               socket_type   = stream
               protocol   = tcp
               user      = attack
               wait      = no
               server      = /home/attack/nctest
            }
            ```

        + __nctest__

            ```c
            #include <stdio.h>

            void init(){
                setvbuf(stdin, (char *)NULL, _IONBF, 0);
                setvbuf(stdout, (char *)NULL, _IONBF, 0);
                setvbuf(stderr, (char *)NULL, _IONBF, 0);
            }

            int main(){
                init();
                printf("Hello Guest\n");
                printf("nc Server Test\n");
            }
            ```

            nc 서버 실행할때 사용되는 프로그램을 c언어로 작성한 다음 밑의 명령어를 실행하면 된다.

            ```sh
            root@root:~$ gcc -o nctest nctest.c
            ```

        Dockerfile, docker-compose.yml, attack, nctest 총 4개의 파일을 만든 다음

        ```bash
        docker-compose up -d
        ```

        명령어를 실행하면 정상적으로 도커가 빌드될시에

        ![docker-compose-up-d](/post-images/Server-Build/nc-server/nc-server-structure.png)

        이와 비슷한 구조로 돌아가고있다고 보면 된다.

        이제 nc 0.0.0.0 10001 으로 접속하면 성공적으로 결과가 나오게된다.

        ```bash
        root@ip-10-0-2-102:/home/ubuntu/nctest# nc 0.0.0.0 10001
        Hello Guest
        nc Server Test
        ```

## nc 서버 연결 안됨

만약 위와 비슷하게 도커(docker)를 이용하여 nc 서버를 만들었는데
연결이 안되는 경우는 대표적으로

1. /etc/services에 서비스 이름과 port를 추가하지 않은 경우
2. /etc/xinetd.d/ 에 적어놓은 서비스 이름과 /etc/services에 적은 서비스 이름이 다를 경우
3. /etc/xinetd.d/ 에 잘못된 실행파일 경로를 넣은 경우

크게 3가지로 꼽히지만 구축하는 환경이 어떻게 되어있냐에 따라 달라질것같다.

## 마무리

만약 구축에 어려움이 있다면 미리 적어놓은 파일 [다운로드](/files/Server-Build/nc-server/nc-server.zip)한 다음 위에서 입력한 명령어들과 동일하게 실행하면 된다.
