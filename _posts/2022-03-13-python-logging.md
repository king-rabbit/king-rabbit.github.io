---
title: "파이썬으로 logging 하기"
excerpt: "파이썬 logging 레벨 등 기초 개념부터 주요 메소드까지 알아봅니다."
author_profile: true
search: true
categories:
  - python
tags: 
  - python
toc : true
toc-sticky : true
layout: single
date: 2022-03-13
---



안녕하세요, 킹래빗입니다.    

이번 포스팅에서는 파이썬으로 logging을 하는 방법을 알아보겠습니다.         

​    

## 파이썬 logging 라이브러리

로깅이란 소프트웨어를 실행했을 때 발생하는 다양한 이벤트를 추적하는 수단입니다.    

파이썬에서는 로깅을 할 수 있게 logging 라이브러리를 제공하고 있습니다.    

여기서 파이썬 로깅 시 표시되는 다음 두 가지 기본 개념을 알고 가시면 되는데요,    

- 이벤트 : 변수를 포함할 수 있는 설명 메시지입니다.    
- 로깅 레벨 : 이벤트에 부여되는 중요도를 의미합니다.     

​    

## 로깅 레벨

파이썬 로깅 레벨은 중요도에 따라 다섯가지 단계로 나뉩니다.    

중요도가 낮은 순으로 DEBUG - INFO - WARNING - ERROR - CRITICAL 이라고 보시면 됩니다.    

다음처럼 이벤트 상황에 따라 로깅 레벨을 다르게 설정하게 됩니다.    

| 레벨     | 설명                                                         |
| -------- | ------------------------------------------------------------ |
| DEBUG    | 가장 중요도가 낮은 레벨.<br />프로그램이 작동하는 지 진단할 때 사용함.   <br />INFO보다 상세한 정보를 출력함. |
| INFO     | 프로그램이 예상한대로 작동하는지 확인할 때 사용함. <br />프로그램이 정상적으로 작동할 때 발생하는 이벤트를 추적함. |
| WARNING  | 예상하지 못한 일이 발생했거나 가까운 미래에 발생할 문제가 있을 때 보고. <br />(프로그램은 계속해서 작동함) |
| ERROR    | 심각한 문제로 소프트웨어 일부 기능이 작동하지 못할 때. <br />예외를 일으키지 않으면서 에러를 보고함. |
| CRITICAL | 심각한 에러로 프로그램 자체가 계속 실행될 수 없을 때. <br />예외를 일으키지 않으면서 에러를 보고할 때. |

로깅 레벨의 기본값은 WARNING으로 설정됩니다.    

즉, 기본적으로는 WARNING 수준 이상의 이벤트만 추적합니다.    





## logger 생성 및 메시지 출력

먼저 라이브러리를 불러옵니다.

```python
import logging
```

로그를 생성할 수 있는 logger를 만들어줍니다.

```python
logger = logging.getLogger('first_logger')  # first_logger는 로거 이름을 의미
```

이처럼 logger 생성 시에 이름을 부여하는데요,    

다른 모듈에서 똑같은 이름으로 logger를 선언하는 경우 같은 로거로 인식하게 됩니다.    

참고로 이름을 주지 않으면 root 로거를 반환합니다.     

또 아래처럼 로깅을 사용하는 모듈마다 모듈의 수준에서 로거를 사용하는 것이 좋습니다.   

이렇게 하면 로거 이름이 이벤트가 기록되는 위치를 나타내게 됩니다. 

``` python
logger = logging.getLogger(__name__)
```

​     

그럼 다음으로 로그 메시지를 출력해보겠습니다.     

레벨별로 메시지를 출력할 수 있는데요, 기본 수준이 WARNING이기 때문에 INFO 메시지는 콘솔에 출력되지 않는다는 점을 알 수 있습니다.    

```python
logging.warning("minor error") #콘솔에 출력됨
logger.critical('error occurred.') #콘솔에 출력됨
logging.info("operating") #콘솔에 출력되지 않음.
```





## logger 설정하기

logging  라이브러리에서는 basicConfig() 메소드를 이용해 Logger의 각종 설정값을 정할 수 있습니다.    

예를 들어 로그 메시지를 어떤 포맷으로 출력할지, 로깅 레벨은 어느 수준으로 할지, 저장할 파일은 무엇으로 할지 등을 정할 수 있습니다.    

메소드의 주요 매개변수는 다음과 같습니다.    

- format : 로그 메시지의 출력 포맷을 설정합니다. 다음과 같은 값을 포함시킬 수 있습니다.    
  - asctime: 이벤트 시간
  - levelname: 로깅 레벨
  - message: 메시지
  - filename: 모듈 파일명
  - lineno: 모듈 파일에서 로그 코드라인이 몇번째 줄인지 표시
- datefmt : 로그 ''시간''이 표시되는 포맷을 설정합니다. 
- level : 가장 낮은 수준의 로깅 레벨을 설정합니다. 이 레벨 이상의 메시지를 모두 저장하게 됩니다.
- filename : 저장할 파일명을 설정합니다.    



다음 예시를 보면 더 이해하기 쉬우실 것 같습니다. 

```python
logging.basicConfig(
		format='%(asctime)s %(levelname)-8s [%(filename)s:%(lineno)d]  %(message)s' ,
		datefmt = '%d-%m-%Y %H:%M:%S', #시간을 표시하는 포맷.
		level=logging.DEBUG,#로깅 레벨 정하기
		filename='logs.txt' #저장할 파일
		)
```

​    

## 부모 logger 상속받기

마침표(.)를 구분 기호로 해서 자식 로거를 생성할 수 있음. 

```python
logger = logging.getLogger('test_logger.childlogger')
```

child logger는 parent logger의 configurations를 상속받게 됨.



## 변수 데이터 로깅하기 

시간이나 모듈 파일명 이외에도 이벤트 메시지에 다양한 변수를 저장해야 될 때가 있습니다.   

이 경우 메시지에 포맷 문자열(%s, %d, %f 등)을 사용하고 변수 데이터를 인자로 추가하면 됩니다.    

```python
logging.warning('%s before you %s', 'Look', 'leap!')
```

이렇게 로그 메시지를 찍으면  "WARNING:root:Look before you leap!" 을 출력합니다.    



## Handler 사용하기

Handler는 로그 메시지를 지정된 대상으로 전달하는 역할을 합니다.    

Handler를 이용하면 레벨에 따라 메시지를 stream이나 파일, http 서버, 메일 등 서로 다른 출력으로 보낼 수 있습니다.    

다음처럼 출력별로 Handler를 생성합니다.       

```python
#출력별 Handler 생성
handler_stream = logging.StreamHandler()
handler_file = logging.FileHandler()
```

핸들러를 이용해 조건에 따라 메시지를 서로 다른 출력으로 보낼 때는 로깅 레벨을 별도로 설정해줘야 합니다.   

```python
#핸들러별 로깅 레벨 지정하기
handler_stream.setLevel(logging.WARNING)
handler_file.setLevel(logging.DEBUG)
```

포맷도 다음처럼 별도로 설정해 줄 수 있습니다.

```python
#포맷 객체 생성
format_s = logging.Formatter('%(name)s - %(levelname)s - %(message)s')
format_f = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
#핸들러별 포맷 설정
handler_stream.setFormatter(format_s)
handler_file.setFormatter(format_f)
```

마지막으로 이렇게 설정된 핸들러를 로거에 추가해줍니다.

```python
#로거에 추가하기
logger.addHandler(handler_stream)
logger.addHandler(handler_file)
```



​     

## References

- https://docs.python.org/ko/3/howto/logging.html#logging-basic-tutorial

- https://docs.python.org/ko/3/howto/logging.html#logging-advanced-tutorial

- https://realpython.com/python-logging/



