---
layout: post
title:  "Python (Django) , Docker 환경에서 구축"
subtitle:   "Python ,Django , Docker "
description : "Python, Django , Docker "
keywords : "python, django, docker, docker-compose, "
categories: dev
comments: true
---



# Python (Django) , Docker 환경에서 구축
<pre>
공부삼아 장고를 보고 있는데...

패캠에서 들었던 DevOps 수업을 적용해보고자 했는데

스터디 모임에서도!! 계속 도커에 대한 이야기가 계속 나오고 있어서..  
한번 검색해봤다..
검색하느라 진 빠져서 포스팅 이 더뎌진건 함정..

검색하는게 힘들어서가 아니라 그냥 찾고 나니 귀찮아짐..

미루고 미루다가 이제사 다시 쓴다..

</pre>

#python

```
django-admin startproject <project_name>
```

장고 앱으로 기본 프로젝트를 생성하자

생성한 프로젝트 dir 에 dockgrfil 을 만들자.
도커 이미지를 빌드 하기 위해선 Dockerfile 이 필요하다

>장고 이미지는 2016-12-31 deprecated 됨 [Docker Hub](https://hub.docker.com/_/django)


docker file  간략 설명

**FROM**
base image 를 지정 하는 경우 사용한다

**RUN**
package 를 인스톨 한다던가 shell 명령어를 실행시킬때 사용한다.
예를 들어 nginx를 설치한다면 아래와 같이 사용한다.
`RUN ["apt-get", "install" ,  "-y", "nginx"]`

**CMD**
docker container 가 시작할때 실행할 커맨드를 지정 할 수 있다. RUN 과 비슷하지만 차이점은 docker image를 빌드할때 실행하는 것이 아니라 docker container 가 시작할때 실행 되는점.
docker image 로 빌드된 application 을 실행할때 사용한다.

**EXPOSE**
외부에 노출할 포트를 지정할때 사용된다.
예를 들어 8080 포트를 외부에 노출시 아래와 같이 사용한다.
`EXPOSE 8080`

**WORKDIR**
작업 디렉토리를 지정 한다

**ADD**
파일과 디렉토리를 호스트에서 docker image 로 copy한다
파일 소스는 파일 이름 및 디렉토리 , url 이 될수도 있다. 디렉토리를 추가시  / 로 끝나야 한다.
만일 추가하려는 디렉토리가 image 에 없으면 docker가 자동으로 생성하며 755 로 생성된다. 또한 tar 압축 파일이면 자동으로 압축을 풀어서 추가한다.
*빌드 디렉토리 외부의 파일은 ADD 할 수 없다.*


**COPY**
ADD 와 기본적인 내용은 동일하나 url을 지정 할 수 없고, 압축 파일을 자동으로 풀어주지 않는다.
*빌드 디렉토리 외부 파일은 copy 및 add 할 수 없다. 호스트에서 이루어지는것이 아니라 현재 빌드 디렉토리 가 docker daemon 에 업로드 된 후 작동하기 때문이다.*

```
# 자신의 파이썬 버전을 적는다
# 버전을 적으려면
# FROM python:3.5.7-stretch 으로 작성한다
FROM python 3.6

# 필요한 것들
RUN apt-get update \
		&& apt-get install -y --no-install-recommands \
			postgresql-client \
		&& rm -rf /var/lib/apt/lists/*
# WORKDIR 은 django-admin startproject 로 만든 django 프로젝트 경로를 입력한다.
# requirements.txt 를 복사해서 pip install
# 본인의 requirements 에 맞게 수정함


WORKDIR /Users/hyeonju/python_withhyun/trollcamp/

COPY requirements.txt ./
RUN pip install -r requirements.txt
# COPY . . 를 통해 코드 복사
COPY . .

#port oepn , runserver 가 8000이 default
EXPOSE 8000

#manage.py 가 있는 dir 로 이동후 runserver 실행
#magage.py 가 dockerfile 과 같은 dir 이면 따로 WORKDIR을 지정할 필요 없다.
WORKDIR /Users/hyeonju/python_withhyun/trollcamp/

CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```


여기서 도커 이미지를 빌드 하자

```
docker image build -t django
-t 옵션은 네이밍 태그 옵션이다.
```
django 란 이름으로 이미지를 빌드한다.

빌드가 끝나면

docker container 를 실행한다 …
`docker container run -p 80:8000 -rm django`

***~안되더라~망했어요***
## **도커 문서에 장고와 postgreSQL 과 같이 연동하는 방법이 설명되어있다.**
이거 하니깐 된다..허허허허허허허허허허허허

1. 프로젝트 폴더를 만들고

2. 아래내용으로 Dockerfile 을 간략히 만들고

	```
	FROM python:3
	ENV PYTHONUNBUFFERED 1
	RUN mkdir /code
	WORKDIR /code
	COPY requirements.txt /code/
	RUN pip install -r requirements.txt
	COPY . /code/

	```


3. requirements.txt 도 생성한다

	기본 requirements.txt

	Django>=2.0,<3.0

	psycopg2>=2.7,<3.0

	```
	requirements.txt 생성은
	#pip freeze > requirements.txt
	#설치되어있는 패키지들이 알파벳순으로 저장됨.
	```

4. docker-compose.yml 를 만들어준다


	```
	version: '3'

	services:
	  db:
	    image: postgres
	  web:
	    build: .
	    command: python manage.py runserver 0.0.0.0:8000
	    volumes:
	      - .:/code
	    ports:
	      - "8000:8000"
	    depends_on:
	      - db

	```

	web 과 db 에 대한 service 를 정의한다.

5. 터미널에서 프로젝트 폴더로 이동하여 docker-compose 를 기반으로 django 프로젝트를 생성한다.
예제니깐 compose example 이란 프로젝트 명으로 생성함.

	`sudo docker-compose run web django-admin startproject composeexample .`

6. 프로젝트가 정상적으로 생성되면 composeexample/settings.py 에서  DATABASES 수정한다

	```
	DATABASES = {
	    'default': {
	        'ENGINE': 'django.db.backends.postgresql',
	        'NAME': 'postgres',
	        'USER': 'postgres',
	        'HOST': 'db',
	        'PORT': 5432,
	    }
	}

	```

7. docker-compose up 명령어를 통해 docker 를 실행한다.
8. http://localhost:8000 통해 접근하면 장고 프로젝트가 보인다!!!

**만세에에에에**



참고

#### Compose and Django ####
[Quickstart: Compose and Django | Docker Documentation](https://docs.docker.com/compose/django/)

도커 관한 설명은 아래 링크를 참고하자.

#### subicura 님의 초보를 위한 도커 안내서
1. [도커란 무엇인가?](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)
2. [설치하고 컨테이너 실행하기](https://subicura.com/2017/01/19/docker-guide-for-beginners-2.html)
3. [이미지 만들고 배포하기](https://subicura.com/2017/02/10/docker-guide-for-beginners-create-image-and-deploy.html)


#### raccoonyy 님의 docker compose 활용법
[Docker(compose)활용법 - 개발환경 구성하기](http://raccoonyy.github.io/docker-usages-for-dev-environment-setup/)


#### 장고걸스 django tutorial ####
[장고걸스](
https://tutorial.djangogirls.org/ko/django_start_project/)
