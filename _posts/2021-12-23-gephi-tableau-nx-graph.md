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
date: 2021-12-23
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



