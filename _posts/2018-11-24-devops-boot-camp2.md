---
layout: post
title:  "패스트캠퍼스 DevOps BOOTCAMP 3기 2주차"
subtitle:   "DevOps 공부2"
description : "DevOps, devops, 패스트캠퍼스, 패스트캠퍼스 BOOTCAMP"
keywords : "devops, DevOps,"
categories: devops
comments: true
---

# DevOps 구축 BOOTCAMP - 2

## Git 활용
늦어서 30분 정도 없다.
뭐 대충 git 설정이다..

## nginx 활용
nginx.conf 파일을 nginx_copy.conf 로 복사해놓고
수정하자

기본 root 상태
root 위치를 재 지정 함으로
기능 apache document root 와 비슷하다


수정한 내용


* location 지정

nginx 에 요청된 url 별 특정 위치에 적용할 설정
laravel 에서 route 와 유사하다
요청이 들어오면 root 위치를 재지정한다
location ~* /(album|signin){
	root blahblah~~
}

- ~ / ~*
	- 들어오는 url 을 정규표현식으로 처리를 위해 사용한다
	- ~ : 대소문자 구분
	- ~* : 대소문자 구분 없이 적용



### 여튼 root , location 의 내용을 바꿨다면
### nginx -s reload , 또는 systemctl start nginx 로 서버를 재시작 한다

### proxy_pass
> 들어온 요청을 다른 웹 서버 or WAS로 전달(revers proxy)
> DB 서버와 연결되 있는 WAS 의 ip 와 포트번호를 감출 수 있음

public zone, dmz(비무장지대??) 외부에서 받은 요청을 내부로 전달 하여
내부망은 nginx 말고는 받지 않는다

* worker_processes auto(or number)
	* 사용할 thread 수를 정의한다 cpu core 수를 입력한다
	* auto 시 해당 머신의 cpu core 수 만큼 자동 설정됨
* worker_connections 1024;
	* worker thread 당 최대 몇개의 connection를 처리 할 것인지 정의
	* 최대 처리량 = worker_processes * worker_connection (이론적으론)


#현재 nginx 설치 끝난 환경까지 AMI 로 구울꺼다
## AMI
ec2 instances 에서 지금 instance를 선택하여 Actions->image->create image 를 선택

IMAGES 에 AMIs 에 이미지가 만들어지고 있다
available 이 됐을경우 instance를 만들자

instance를 만들때 My AMIs에 해당이미지가 있다
instance의 장비, 보안그룹 설정하고 run
ami 로 만들 instance 로 접근하면 기존에 설정한 작업 내용 그대로 보인다

Auto scaling 을 위한 새로운 ec2 instance를 만들기 위한 image를 만들었다

## Load balancer

* Health-check
	* 각 인스턴스들에게 주기적으로 정상 작동중인지 물어봄
	* 확인 요청을 보냈을때 200번대 http request 면 정상
	* 그 외 http request 면 장애 서버라 판단하여 정상적인 인스턴스에게만 요청을 전달함
* application Load Balancer
* Network Load Balancer
* Classic Load Balancer(이전 세대 http, https, tcp용)


연결 드레이닝 활성화 lb가 서버에 요청을 보낼 상황에 scale in 이 발생할 순간이 오는데 대기시간을 의미한다 입력 값 만큼 대기 하고 서버를 내린다

## auto scaling
auto scaling 시 lb 에 붙을때 Health Check Grace Period 에 입력값 만큼 health check를 하지 않는다.
어찌 보면 보안적이지 않다. 모르는 사이에 만들어지고 사라지므로 auto scaling을 사용할때 AWSServiceRoleForAutoScaling 으로 iam 권한을 가진다.
최소, 최대 n 개의 인스턴스를 정한다.
cpu 사용량으로 늘리고 줄이기에 최소, 최대값이 필요하다

scale group size를

Scale the Auto Scaling group using a target tracking scaling policy

클릭하여 increase Group Size 의 경보 알림을 설정한다.
사용중인 instances 의 cpu 사용량을 70%,  70% 이상 지속된 시간이 5분 이상이면 알림을 주도록 설정하면 자동으로 설정된다.

마찬가지로 Decrease Group Size를 설정하자

notification 알림에 대해서 방금 만든 두개의 알림을 등록하자.

등록하면 입력한 이메일로 해당 알림이 등록됐다고 온다
**이메일로 등록하기도 하지만 slack bot, sns push ... 바로 받을수있는걸로 등록하자**
설정이 끝나고 생성된 auto scaling group 을 보면 해당 instance가 하나 떠 있고
load balance 탭을 가면 auto scaling group 으로 떠있는 instance 가 있을것이다



#마이크로서비스 아키텍처
#모노리틱 아키텍처



### Scale-up , Scale-out
* Scale-up (사양추가)
	* vm, container 등 os 가 작동하는 물리적 논리적 머신의 장비빨, 물리 장비를 추가한다
	* AI, Big data 등 고성능 머신이 필요할때

*  Scale-out (서버추가)
	* 하나의 서버가 처리 할 일을 같은 서버가 나누어서 처리함
	* 보통의 웹서비스에 적합



## sub session

### vim

간단한 문서 편집기


```
vimtutor -g ko
```
간단한 vim 튜토리얼

h : left
j : down
k : up
l : right

* x
> 입력 상태가 아닌 경우 커서가 해당하는 글자를 지운다



* i
> 입력 상태로 전환, 이 상태에서 나오려면 esc


* dw
	> 명령어 상태에서 커서가 해당한 위치의 단어를 지운다

	> 약간 단어인식이 구리다.

	> don't 의 경우 don 까지만 지워짐

* d$ , d^
> $ , ^ 는 정규식으로
>
> $ 마지막 라인
>
>^ 라인 시작점
>
>d$ 는 커서의 위치부터 라인 끝
>d^ 는 커서의 위치부터 라인 시작

* dd
> [횟수]dd
> 횟수만큼 라인을 지운다
>
> 줄전체를 지움

* u, U
> undo
> 소문자는 직전 취소, 대문자는 그 라인 취소

* p
> 붙여넣기
>
> dd 를 사용하여 지운 라인을 vim 에서 버퍼로 잡고 있다.
>
> dd 는 삭제 , 또는 잘라내기 의 역할을 한다

* r
> replace 커서의 위치에 있는 단어를 입력모드(i) 를 입력하지 않고 단어가 변경된다
>
> i 로 입력모드가 되면 단어를 끼워넣는다.
>

* cw
> 단어를 수정함 dw 와 반대되는 기능이다.
>
> 커서의 위치에서 단어의 끝까지 삭제가 되며 입력모드로 변경된다
>
> c$ , c^
> d$ , d^ 와 같다 라인 끝, 라인 시작 까지 삭제후 입력모드가 된다

* y
> 복사
> y$, y^ 는 알꺼라 생각한다.
> yy 는 전체 라인 복사
> [횟수]yy 는 횟수만큼 라인 복사

* ctrl + g
> 현재 파일의 상태
>
> 현재 위치한 라인번호, 파일 끝 라인 번호, 가 나타남

* shift + g
> 파일 끝으로 감
> [라인번호] + shift+g 해당라인으로 간다
> 명령어 모드에서 [라인번호] 면 해당 라인으로 간다

* gg
> 파일 처음으로 감

* /
> 커서의 위치 에서 아래로 검색
>
> 다음 검색은 n으로 N 은 역방향 검색임
>
> ? 는 반대로 위치에서 위로 검색함

* %
> 커서가 (,[,{ 짝으로 해당하는 닫는 기호에 이동한다

* :s/변경대상언어/바뀔값
> 커서가 위치에서 부터 단어를 검색 한개만 찾아 해당어를 바뀔값으로 변경함
>
> :s/변경대상언어/바뀔값/g 를 하면 해당 라인 전체를 바꿔준다
<img 를 넣자>


* o, O
> o 는 아랫줄에 입력모드가 되며
>
> O는 커서가 윗라인으로 가며 입력모드가 되고 개행한다.


* option
* :set hls
> 검색된 단어를 하이라이트
* :nohl
> 하이라이트 삭제

* ctrl + z , fg
> ctrl + z 백그라운드 작업으로 돌리기
> fg 백그라운드로 돌린 작업 앞단으로 돌리기
