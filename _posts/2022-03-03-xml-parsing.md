---
title: "파이썬으로 xml 파싱하기"
excerpt: "파이썬 내장 라이브러리인 etree 라이브러리로 xml 데이터를 파싱하는 방법"
author_profile: true
search: true
categories:
  - python
tags: 
  - python
toc : true
toc-sticky : true
layout: single
date: 2022-03-03
---



안녕하세요, 킹래빗입니다.    

최근에 XML 파일을 파싱할 일이 생겨서 파이썬으로 파싱하는 방법을 글로 정리해봅니다.    

파이썬 표준 라이브러리인 xml.etree.ElementTree을 사용해서 파싱하는 방법이구요.

[공식 문서](https://docs.python.org/ko/3/library/xml.etree.elementtree.html)를 참고해 작성했습니다.    

​    

## XML이란?

xml은 트리 형태의 데이터인데요, html과 유사한 형태로 노드(또는 element)들이 층을 지어 나누어져 있습니다.    

각 노드는 태그로 이뤄져있고, 태그에 attribute가 있을 수 있어요. 또 태그가 텍스트를 감싸고 있을 수 있습니다.      

**xml.etree.ElementTree** 라이브러리에서는 다음과 같이 두 가지 클래스를 정의해 xml 데이터를 나타내고 있습니다.      

- *ElementTree* : xml 문서를 트리로 나타낸 클래스.      

- *Element* : 단일 노드를 나타낸 클래스.    

​    

## 데이터 불러오기

그럼 본격적으로 데이터를 불러오고 파싱 작업을 시작해 보겠습니다.    

샘플 파일(books.xml)은 [여기](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/ms762271(v=vs.85))에서 가져왔습니다.     

​    

먼저 라이브러리를 import해주고 데이터를 불러옵니다. xml 파일은 라이브러리에서 제공하는 parse 메서드를 이용해 파싱된 형태로 데이터를 불러올 수 있습니다.    

```python
import xml.etree.ElementTree as ET
tree = ET.parse('books.xml')
```

​    

다음으로 트리의 정점이 되는 root 데이터를 가져옵니다. 역시 라이브러리에서 이런 기능의 메서드 getroot()를 제공하고 있으니 이걸 사용합니다.    

```python
root = tree.getroot() # 해당 트리의 root를 반환
```

​    

여기서 tree는 ElementTree객체, root는 Element 객체입니다.

```python
print(type(tree))
print(type(root))
```

결과값이 아래처럼 출력됩니다.    

```
<class 'xml.etree.ElementTree.ElementTree'>
<class 'xml.etree.ElementTree.Element'>
```

​            

## Element 태그 / 속성 확인하기

Element는 **tag**와 **attribute**로 이루어져 있습니다.     

아래처럼 tag, attrib 변수로 조회해 봅니다. attribute는 딕셔너리 형태입니다.     

```python
print(root.tag)
print(root.attrib) 
```

아래처럼 결과값이 표시되는데요, root element의 태그는 catalog이고 attribute는 없네요.    

```
catalog
{}
```

​    

## 자식 노드 조회하기

element마다 **자식 노드**가 있을 수 있습니다.      

여기서는 root 노드의 자식 노드를 확인해봅니다. 자식 노드의 태그와 속성(attribute)를 확인합니다.     

```python
for child in root:
    print(child.tag, child.attrib)
```

​    

다음 결과값을 보시면 여기서 자식 노드들은 book 태그에 각 id값을 attribute로 갖고 있습니다.    

```
book {'id': 'bk101'}
book {'id': 'bk102'}
book {'id': 'bk103'}
book {'id': 'bk104'}
book {'id': 'bk105'}
book {'id': 'bk106'}
book {'id': 'bk107'}
book {'id': 'bk108'}
book {'id': 'bk109'}
book {'id': 'bk110'}
book {'id': 'bk111'}
book {'id': 'bk112'}
```



참고로 인덱스를 통해서도 자식 노드와 특정 태그/속성 값을 확인할 수 있습니다.

```python
root[0][1].text 
```

첫번째 book element의 두번째 태그(책 제목)의 텍스트를 출력해봅니다.    

```
"XML Developer's Guide"
```

​     

## attribute 값 가져오기

참고로 태그 안에 있는 attribute값은 key와 value로 구성되어 있습니다. 값을 각각 불러올 수도 있고 함께 불러올 수도 있습니다.    

​		**get()** : key값을 넣어서 해당하는 value 값을 반환

​		**keys()** :  attribute key값 리스트 가져오기

​		**items()** : attribute 딕셔너리 시퀀스를 반환.

```python
book1 = root[0]
print(book1.get('id')) 
print(book1.keys()) 
print(book1.items()) 
```

첫번째 book element의 attribute에 저장된 key와 value를 확인한 결과입니다.    

```
bk101
['id']
[('id', 'bk101')]
```

​        

## 태그 내 텍스트 출력하기

앞에서도 봤지만 태그로 감싸고 있는 텍스트는 text 변수로 확인할 수 있습니다.    

여기서는 첫번째 book element의 태그와 태그별 텍스트를 조회해 보았습니다.    

```python
for child in root[0]:
    print(child.tag, child.text)
```

아래처럼 각 book element에는 책의 저자와 제목, 장르, 가격, 출판일, 설명이 태그별로 저장되어 있고 해당 값이 무엇인지 알 수 있습니다.    

```
author Gambardella, Matthew
title XML Developer's Guide
genre Computer
price 44.95
publish_date 2000-10-01
description An in-depth look at creating applications with XML.
```

​    

## 전체 텍스트 출력하기

라이브러리에서 제공하는. **itertext()** 메소드를 이용해 특정 노드를 루트로 하는 트리 이터레이터를 만들고, 내부의 모든 텍스트를 반환할 수 있습니다.    

```python
for book_text in root[0].itertext():
		print(book_text)
```

아래처럼 내부 텍스트가 한번에 반환됩니다.    

```
Gambardella, Matthew

      
XML Developer's Guide

      
Computer

      
44.95

      
2000-10-01

      
An in-depth look at creating applications 
      with XML.


```

​    

## 특정 태그 값에 해당하는 element 찾기

특정 태그 값에 해당하는 element만 찾을 수도 있는데요, 이 때는 find() 또는 findall() 메소드를 사용합니다.    

find() 메소드로는 해당 태그가 있는 첫번째 element를, findall()로는 해당 태그가 있는 모든 element를 반환합니다.    

```python
for book in root.findall('book'):
    author = book.find('author').text
    title = book.find('title').text
    genre = book.find('genre').text
    print(author," | ", title, " | ",genre)
```

책의 저자와 제목, 장르를 파악하기 위해 author, title, genre 태그로 찾아 보았습니다.    

```
Gambardella, Matthew  |  XML Developer's Guide  |  Computer
Ralls, Kim  |  Midnight Rain  |  Fantasy
Corets, Eva  |  Maeve Ascendant  |  Fantasy
Corets, Eva  |  Oberon's Legacy  |  Fantasy
Corets, Eva  |  The Sundered Grail  |  Fantasy
Randall, Cynthia  |  Lover Birds  |  Romance
Thurman, Paula  |  Splish Splash  |  Romance
Knorr, Stefan  |  Creepy Crawlies  |  Horror
Kress, Peter  |  Paradox Lost  |  Science Fiction
O'Brien, Tim  |  Microsoft .NET: The Programming Bible  |  Computer
O'Brien, Tim  |  MSXML3: A Comprehensive Guide  |  Computer
Galos, Mike  |  Visual Studio 7: A Comprehensive Guide  |  Computer
```

​        

여기까지 간단하게 파이썬으로 xml 파일을 파싱하는 방법을 정리해봤습니다.    

기회가 되면 xml 파일을 만들거나 수정하는 방법도 써보려고 합니다.       

읽어주셔서 감사합니다!    
