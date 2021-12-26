---
title: "Gephi를 이용해 태블로 네트워크 그래프 그리기(1)"
author_profile: true
search: true
categories: 
  - Data Visualization
tags:
  - Tableau
  - Gephi
  - Network Graph
toc: true
toc_sticky: true
layout: single
date: 2021-12-25
---



안녕하세요, 킹래빗입니다.       

오늘은 Gephi와 태블로를 이용해 네트워크 그래프를 그리는 법을 이야기해보려고 합니다.   

약간 복잡할 수 있는데요, Gephi로 노드별 좌표값 데이터를 생성한 뒤 이를 태블로에 올린다고 생각하시면 됩니다.   

여기서는 데이터를 준비하고 Gephi로 그래프를 그리는 것까지 하고, 2편에서 태블로에 그래프 생성을 완료하려고 합니다.

예시 데이터 및 데이터 정제 과정은 [**깃허브 리포지토리**](https://github.com/king-rabbit/king-rabbit-data-analysis/tree/main/netflix_content)에 올려놓았으니 참고해주세요.   
그럼 시작해볼게요.   



## 1. Gephi용 데이터 준비 

데이터는 node와 link 테이블로 2가지를 준비합니다. 여기서는 예시 데이터로 miserables.json을 사용했습니다([데이터 출처](https://www-cs-faculty.stanford.edu/~knuth/sgb.html)). Gephi에 올리기 위해 json 파일을 pandas 데이터프레임으로 변환해서 csv 파일로 저장해줍니다.   



**nx_data_process.ipynb**

```
import pandas as pd
import json

with open('data/miserables.json', 'r') as f:
    data = json.load(f)

nodes = pd.DataFrame(data['nodes'])
links = pd.DataFrame(data['links'])

nodes.to_csv('data/nodes.csv', index=False)
links.to_csv('data/links.csv', index=False)
```



**데이터 설명** 

1) node.csv

   id 외 기타 node의 특성을 나타내는 속성으로 구성합니다. 여기서는 group 속성이 있습니다. 이 속성값을 이용해 그래프에서 노드의 크기 / 색상 / 노트의 모양을 변화시킬 수 있습니다.
   
   <p align="center"><img src="/images/nxgh-tb-img/node_data.png" alt="node.csv" style="zoom:70%;" /></p>



2) link.csv

   source, target, value(Weight) 컬럼으로 구성source와 target은 node.csv의 id값을 참조해야 합니다. 즉, link에서 1(source)->2(target)이라면 node에서도 이들 노드의 id는 1,2이어야 합니다. 
   
   <p align="center"><img src="/images/nxgh-tb-img/link_data.png" alt="link.csv" style="zoom:70%;" /></p>



## 2. Gephi에서 그래프 그리기

 



이번에는 Gephi에서 그래프를 그리고 노드별 좌표 데이터를 추출할 차례입니다.

1. Gephi를 설치하신 후 프로그램을 실행해주세요.

2. 파일 > Import Spreadsheet 를 클릭해 node와 link(또는 edge) 데이터를 각각 불러와 줍니다.	

   

   (1) node 데이터 불러오기

   node 데이터의 경우에는 Import as 항목에서 Nodes table 을 선택합니다.

   <p align="center"><img src="/images/nxgh-tb-img/nodes_1.png" alt="nodes_gephi" style="zoom:70%;" /></p>

   다음으로 데이터 형식 및 적용할 컬럼을 확인하신 후 데이터를 불러오시면 됩니다.      

   <p align="center"><img src="/images/nxgh-tb-img/nodes_2.png" alt="nodes_gephi" style="zoom:70%;" /></p>

   (2) link 데이터 불러오기

   link 테이블의 경우에는 Import as 항목에서 Edges table를 선택해줍니다.    

   <p align="center"><img src="/images/nxgh-tb-img/links_1.png" alt="nodes_gephi" style="zoom:70%;" /></p>

   node 데이터를 이미 추가한 상태이므로 여기서는 **기존 workspace에 더하는 방식**으로 import해야 합니다.   

   <p align="center"><img src="/images/nxgh-tb-img/links_2.png" alt="nodes_gephi" style="zoom:70%;" /></p>

   

3. 그래프 그리기

   Layout에서 Force Atlas, Fruchterman Reingold, Yitan Hu Proportional 등 원하는 레이아웃을 선택한 뒤 각종 property를 조정해서 그래프를 원하는 모양으로 조정해 그래프를 그립니다.

   <p align="center"><img src="/images/nxgh-tb-img/graph_gephi.png" alt="graph_gehi" style="zoom:70%;" /></p>

   

4. 파일 저장하기

   저장은 파일>export>Graph file 을 선택한 후 **gexf 파일**로 저장해주세요!



이번 포스팅은 여기까지입니다.   

2편에서 태블로에 그래프 파일을 올려서 그래프를 그리는 실습을 해보도록 하겠습니다.  

감사합니다.     
