---
layout: post
title:  "데이터 엔지니어 수업 정리 "
subtitle:   "패캠 데이터엔지니어 수업"
categories: data
tags: fcdes
comments: true
---
수업 내용을 정리 한 것

## python

* NumPy , Pandas 는 머신러닝에서 많이 사용되므로 알아둘 필요가 있다!!!!!!!
* pySpark 실습
* TensorFlow 실습
* zepplin 으로 pySpark 실습
* 딥러닝 분야로 많은 프레임워크가 나왔지만 TensorFlow로 모이는 추세
* pandas
 - dataFrame 으로 spark 에서 dataframe 개념을 참조 함
* Matplotlib
 - Python 코드를 사용하여 시각화 함
* **jupyter** vs **zepplin**
 - 빅데이터 분야에서는 spark 를 사용하여 zepplin 시각화를 많이 하지만 아직까지 머신러닝 분야에서는 jupyter 를 좀 더 많이 쓰며 대중적이다
 - jupyter 는 github 과 공유 가능

> ```연산자 기호들이 ! = not , == = is 같이 문자로 대체되기도 한다```

* 모듈화
	* file명 으로 import 가능 	
	* sql 처럼 import 할때 별칭으로 이름을 변경 할 수 있다

	> import draw_visual as draw 	
	draw_visaul 이 draw 로 변경됨

* package
	* 다른 언어들과 비슷하게, "." 으로 분류된 모듈 체계
	* ***__init__.py*** 파일이 있어야 한다
	* **Virtual environment**
		* 패키지 설치를 하면 보통 전역적으로 설치됨
		* 프로젝트 별로 다른 버전의 패키지들을 설치 할 경우
		* 골치가 아픈... 			

## jupyter notebook
>머신러닝에서는 파이썬이 강세라 jupyter notebook 을 자주 사용 할 예정

```남은 수업이 6번이라니?ㅠㅠ```
```python string index func 에서 검색이 안되면 에러난다```

> ***scala hashmap*** 꼭 알아둬라!***

## NumPy
>Python list 대체, 빠르고 기능이 많다.
>행렬 계산이 쉽게 된다

>남은 수업시간 동안 공부해도 마스터 하긴 힘드니 머신러닝에서 어떻게 사용되는지 감을 알아가자

## Pandas##
>spark dataFrame 과 연관지어 설명.
>import pandas as pd 로 호출 일일히 pandas 로 치기 귀찮;;;
>
>spark dataFrame 과 매우 유사함

## PySpark##
>기존 spark 의 대부분 기능을 python 으로 사용 가능함

>rdd api 에서는  Python -> JVM 으로 object copy 가 일어나서 PySpark 의 실행성능이 떨어졌음

>현재 dataFrame api는 scala와 동일한 성능을 가짐

>zeppelin 연동도 가능하고 terminal 에서 spark-shell 말고 pyspark 실행하면 된다
```zepplin 에서......................문제가 많아서 zeppelin 다운 받아서 직접 빌드하여 쓰자```



## zeppelin bulid 방법##
zepplin git hub 가서 tag에 v0.8.0-rc5를 다운 받아서

git hub 하단에 빌드 방법을 참고 하여 빌드
다음 포스트에 zeppelin 빌드 방법을 쓰겠음.
```
mvn clean package -DskipTests [Options]
```



##차회예고##
**2학기의 중반이 끝남.....
하지만 후다다닥 배운지라 아직 소화는 안됨 이것저것 좀 해봐야 함**
