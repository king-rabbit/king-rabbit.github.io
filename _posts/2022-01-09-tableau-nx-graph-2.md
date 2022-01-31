---
title: "Gephi를 이용해 태블로 네트워크 그래프 그리기(2)"
excerpt: "데이터 전처리 및 Gephi 그래프 그리기"
author_profile: true
search: true
categories: 
  - data visualization
tags: 
  - Tableau
toc: true
toc_sticky: true
layout: single
date: 2022-01-09
---



안녕하세요, 킹래빗입니다.       

저번 포스팅에 이어서 네트워크 그래프를 그리는 법을 설명해 보려고 합니다.

지난 포스틩에서 Gephi에 그래프를 그리는 것까지 완성했는데요, 이번 편에서는 태블로로 그래프를 예쁘게 그려볼게요!

예시 데이터 및 데이터 정제 과정은 [**깃허브 리포지토리**](https://github.com/king-rabbit/king-rabbit-data-analysis/tree/main/netflix_content)에 올려놓았으니 참고해주세요.   
그럼 시작해볼게요.   



## 1. 그래프 파일 변환하기 

Gephi에서 저장한 그래프 파일을 엑셀로 열어줍니다. 엑셀로 그래프 파일을 열면 파일을 여는 방법을 선택하라고 나올텐데요, 여기서 'XML 표로'를 선택해주시면 됩니다.    

그래프 파일을 열면 다음 이미지처럼 뜨는데 여기서 노드값과 x,y 좌표값 데이터만 긁어서(붉은색 박스 부분) 새 엑셀 파일에 붙여넣기 한 뒤 coordinates.xlsx 파일로 저장해주세요! (행이 중복되는 경우가 있으면 삭제하시면 됩니다.) 



<p align="center"><img src="/assets/images/nxgh-tb-img/gephi_excel.png" alt="node.csv" style="zoom:70%;" /></p>



 coordinates.xlsx 파일은 아래와 같은 모양이 됩니다.



<p align="center"><img src="/assets/images/nxgh-tb-img/coordinates_data.png" alt="node.csv" style="zoom:70%;" /></p>





이제 다시 jupyter notebook으로 돌아와서 링크 데이터를 정제해야 합니다왜냐면 태블로에서 네트워크 그래프를 그리기 위해서는 아래와 같은 형태의 링크 데이터가 필요하기 때문인데요(노드 데이터는 추가 전처리가 필요없습니다),

​    

**링크 파일 형태**:	

	- name / relationship / x / y 컬럼으로 구성
	
	- name에는 노드 이름
	- relationship에는 관계값 (이름->이름)	
	- x,y 좌표는 각 노드의 좌표값 넣어줌.	
	
	- 따라서 하나의 관계당 2개의 행이 있어야 합니다.		
	
	ex)		a / a->b / 5 / 1		b / a->b / 10 / 3

​    

​    

jupyter notebook에서는 다음처럼 링크 데이터를 정제한 뒤 저장해주세요.

```
#좌표 데이터 불러오기
coordinates = pd.read_excel('data/coordinates.xlsx')

#링크 데이터에 relationship 컬럼 생성
links['relationship'] = [pair[0] + '->' + pair[1] for pair in zip(links['source'], links['target'])]

# 태블로형 데이터로 변환
links_re = links[['target', 'value', 'relationship']] #target ~ relationship 컬럼을 복사한 데이터프레임
links_re = links_re.rename(columns={'target': 'id'}) #두 데이터프레임을 합치기 위해 컬럼 이름 변경
links = links.rename(columns={'source': 'id'}) #두 데이터프레임을 합치기 위해 컬럼 이름 변경
del links['target'] #필요없는 target 컬럼 제거
links_concated = pd.concat([links, links_re]) #데이터 합치기

#좌표값을 merge해줍니다.
links_concated = pd.merge(links_concated, coordinates, on='id', how='left')

links_concated.to_csv('data/links_concated.csv') #데이터 저장

```



이렇게 저장한 links_concated.csv파일은 다음과 같은 형태가 됩니다.



<p align="center"><img src="/assets/images/nxgh-tb-img/link_concated_data.png" alt="node.csv" style="zoom:70%;" /></p>



여기까지 오느라 수고하셨습니다. 이렇게 링크 데이터 전처리가 끝나면 마지막으로 태블로로 가서 그래프를 그려줍니다!    







## 2. 태블로에서 그래프 그리기

 

태블로에서는 준비한 node.csv와 link_concated.csv를 불러옵니다. 'id'칼럼으로 node 데이터와 link 데이터를 결합해주세요.



<p align="center"><img src="/assets/images/nxgh-tb-img/tableau_1.png" alt="node.csv" style="zoom:70%;" /></p>



열에는 link 데이터의 x 컬럼을, 행에는 node 데이터의 y컬럼을 설정합니다(차원값으로 넣어주세요).     

다음으로 node 데이터의 y컬럼을 행에 한번 더 넣고 이중축으로 설정해주세요. 노드의 원과 선을 각각 설정해주기 위해서 이렇게 node 데이터의 y컬럼을 두번 넣게 됩니다. 



<p align="center"><img src="/assets/images/nxgh-tb-img/tableau_2.png" alt="node.csv" style="zoom:70%;" /></p>



'마크'탭에서 첫번째 y컬럼 카드는 '라인'으로, 두번째 y컬럼 카드는 원으로 설정해줍니다.    

그리고 '라인'으로 설정한 카드에서 세부정보로 링크 데이터의 'Relationship' 값을 넣어주세요. 그러면 다음과 같이 기본적인 네트워크 그래프가 나옵니다.



<p align="center"><img src="/assets/images/nxgh-tb-img/tableau_3.png" alt="node.csv" style="zoom:70%;" /></p>





마지막으로 노드 데이터의 Group 컬럼 값을 이용해 원의 색(또는 크기)을, 링크 데이터의 Value 값을 이용해 선의 색 (또는 굵기)를 설정해주세요.    
이제 완성입니다!    

<p align="center"><img src="/assets/images/nxgh-tb-img/tableau_4.png" alt="node.csv" style="zoom:70%;" /></p>



