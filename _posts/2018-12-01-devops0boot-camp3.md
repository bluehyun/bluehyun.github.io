---
layout: post
title:  "패스트캠퍼스 DevOps BOOTCAMP 3기 3주차"
subtitle:   "DevOps 공부2"
description : "DevOps, devops, 패스트캠퍼스, 패스트캠퍼스 BOOTCAMP"
keywords : "devops, DevOps,"
categories: devops
comments: true
---


# DevOps 구축 BOOTCAMP - 3

## WAS(Web Application Server)

* 동적 데이터를 제공하기 위한 서버
* Nginx를 proxy 서버로 설정하여 WAS 영역과 연결하는 경우가 많음
* 개발팀이 사용하는 언어, 프레임워크가 제각각이라 DevOps에서 효율적 설계가 필요함
> node.js
> Chrome V8 엔진 기발으로 빠른 코드 실행 환경 제공
> 빌드 할 필요가 없어서 편하게 환경구축을 할 수 있다
>
> 전에 다른 수업처럼 주 가 될 내용이 아니라 서버를 띄우기 위한 도구로만 사용 할 예정
>
> 단일 쓰레드와 이벤트 루프(nonblocking I/O)를 사용하여 많은 connection을 처리하는 웹서비스 환경에서 유리함
>
> 서버 리소스의 효율성과 라우팅이 유리하여 MSA(MicroServer Architecture)에 적합함

서버에 프로세스를 설치시 안정된 버전을 설치해야 한다.
가장 최신의 버전이 아니라
stable 한 버전을 찾아서 설치하는것이 안정된 서버 환경을 구축 할 수 있다

어떤 프로젝트, WAS 든.. 프로세스를 관리 할 수 있는 프로그램이 있다.

패키지 매니저가 필요한 이유
was는 내부적으로 많은 코드, 모듈이 필요하는데 이걸 사람이 직접 설치하게 되면 실수를 하거나
시간을 소모하게 된다.
대부분 server언어 프레임워크에서는 패키지 매니저가 의존성 관리를 해준다

아마존 cli 를 통해 아마존 ui 에서 하는 작업을 할 수 있다.



지금 작업한 ec2 를 ami로 만들고 lb를 만들어 붙이자

ec2 를 만들때 public ip 가 아니라 private ip 를 부여 할 수 있다.
하여 lb 를 내부 lb로 만들고 lb만 노출하여 lb가 내부 ec2에 접근한다.

db 는 정말 stable 한 버전을 사용하여야 한다

운영서버 및 백업서버는 보안, 안전 상 이유로 외부에 노출되지 않아야 한다

외부와 차단된 공간에 있는 pc를 ip공인망을 설치하여 백업

cloude watch보다는
db를 sql query, trancstion log monitoring 을 하는 툴이 많으니 찾아보자.


db security group 생성시


## subsession
### git
