---
title: "netflix_content_analysis"
search: true
categories:
 - Notebook
tags:
 - Need_modify
last_modified_at: 2999-12-31 23:59
layout: single
classes: wide
---
# pyecharts로 넷플릭스 데이터 시각화하기
=================

안녕하세요, 킹래빗입니다🐰    
데이터 분석으로 처음 글을 적어보는데요,     
    
다들 많이 보시는 넷플릭스 영화 및 TV 콘텐츠를 분석해 본 내용입니다🎬🍿    
데이터는 kaggle의 데이터셋을 이용해서 **미국 넷플릭스 데이터**라고 보시면 될 것 같아요.   
pyecharts를 이용해 간단하게 시각화하는 내용이어서 쉽게 따라하실 수 있어요.    

> 데이터와 분석 노트북은 깃허브 리포지토리에서 확인하실 수 있습니다.     
> + [데이터셋 바로가기][link]
> + [리포지토리 바로가기][link_repository]

[link]: https://www.kaggle.com/shivamb/netflix-shows "netflix-shows-dataset"
[link_repository]: https://github.com/king-rabbit

## 1. 데이터 불러오기
---------------------
필요한 라이브러리와 데이터를 불러오고 데이터 형식 등을 확인해봅니다.    
**참고로 pyecharts는 0.5.11버전입니다.**

<div class="prompt input_prompt">
In&nbsp;[1]:
</div>

<div class="input_area" markdown="1">

```python
import warnings
import pandas as pd
import numpy as np
from pyecharts import Bar, Line, Pie, Map
warnings.filterwarnings(action='ignore')
```

</div>

<div class="prompt input_prompt">
In&nbsp;[2]:
</div>

<div class="input_area" markdown="1">

```python
#데이터 불러오기
data = pd.read_csv('netflix_titles.csv')
```

</div>

* 드라마/영화 콘텐츠에는 고유번호가 지정되어 있습니다(show_id 컬럼)
* type에서는 영화인지 TV쇼인지를 구분합니다
* 출연진(cast)과 국가(country, 두개 이상인 경우)는 comma(,)로 구분되어 있습니다
* 콘텐츠 길이(duration)의 경우 영화면 분 단위로, TV쇼이면 시즌 단위로 표기됩니다
* 장르는 listed_in 컬럼이며, 두개 이상의 장르에 속한 경우 comma(,)로 구분됩니다.

<div class="prompt input_prompt">
In&nbsp;[3]:
</div>

<div class="input_area" markdown="1">

```python
data.head(5)
```

</div>




<div markdown="0">
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>show_id</th>
      <th>type</th>
      <th>title</th>
      <th>director</th>
      <th>cast</th>
      <th>country</th>
      <th>date_added</th>
      <th>release_year</th>
      <th>rating</th>
      <th>duration</th>
      <th>listed_in</th>
      <th>description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>s1</td>
      <td>Movie</td>
      <td>Dick Johnson Is Dead</td>
      <td>Kirsten Johnson</td>
      <td>NaN</td>
      <td>United States</td>
      <td>September 25, 2021</td>
      <td>2020</td>
      <td>PG-13</td>
      <td>90 min</td>
      <td>Documentaries</td>
      <td>As her father nears the end of his life, filmm...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>s2</td>
      <td>TV Show</td>
      <td>Blood &amp; Water</td>
      <td>NaN</td>
      <td>Ama Qamata, Khosi Ngema, Gail Mabalane, Thaban...</td>
      <td>South Africa</td>
      <td>September 24, 2021</td>
      <td>2021</td>
      <td>TV-MA</td>
      <td>2 Seasons</td>
      <td>International TV Shows, TV Dramas, TV Mysteries</td>
      <td>After crossing paths at a party, a Cape Town t...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>s3</td>
      <td>TV Show</td>
      <td>Ganglands</td>
      <td>Julien Leclercq</td>
      <td>Sami Bouajila, Tracy Gotoas, Samuel Jouy, Nabi...</td>
      <td>NaN</td>
      <td>September 24, 2021</td>
      <td>2021</td>
      <td>TV-MA</td>
      <td>1 Season</td>
      <td>Crime TV Shows, International TV Shows, TV Act...</td>
      <td>To protect his family from a powerful drug lor...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>s4</td>
      <td>TV Show</td>
      <td>Jailbirds New Orleans</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>September 24, 2021</td>
      <td>2021</td>
      <td>TV-MA</td>
      <td>1 Season</td>
      <td>Docuseries, Reality TV</td>
      <td>Feuds, flirtations and toilet talk go down amo...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>s5</td>
      <td>TV Show</td>
      <td>Kota Factory</td>
      <td>NaN</td>
      <td>Mayur More, Jitendra Kumar, Ranjan Raj, Alam K...</td>
      <td>India</td>
      <td>September 24, 2021</td>
      <td>2021</td>
      <td>TV-MA</td>
      <td>2 Seasons</td>
      <td>International TV Shows, Romantic TV Shows, TV ...</td>
      <td>In a city of coaching centers known to train I...</td>
    </tr>
  </tbody>
</table>
</div>
</div>



## 2. 어느 나라 콘텐츠가 많을까
------------------
아무래도 나라별로 콘텐츠 개수에 차이가 나겠죠. 국가별 콘텐츠 개수가 어떻게 되는지 확인하기 위해 먼저 country 컬럼을 전처리해 줍니다.    
comma를 기준으로 country 컬럼을 구분해 준 뒤 국가별로 영화/TV콘텐츠 개수를 집계한 데이터프레임을 만드는 방식입니다.

<div class="prompt input_prompt">
In&nbsp;[4]:
</div>

<div class="input_area" markdown="1">

```python
countries = []
for i in range(len(data)):
    if type(data.loc[i, 'country']) == str:
        data.loc[i, 'country'] = data.loc[i, 'country'].replace("\"", "")
        countries_content = data.loc[i, 'country'].split(', ')
        for country in countries_content:
            countries.append(country)
    else:
        pass

countries = list(set(countries))
```

</div>

<div class="prompt input_prompt">
In&nbsp;[5]:
</div>

<div class="input_area" markdown="1">

```python
countries.remove('') #아무것도 없는 문자열 제거
countires_re = [ country.replace(',', '') for country in countries ]
countires_re = list(set(countires_re)) # 국가명 리스트 생성
```

</div>

<div class="prompt input_prompt">
In&nbsp;[6]:
</div>

<div class="input_area" markdown="1">

```python
countries_cnt = []
for country in countires_re:
    cnt = 0
    # 국가별로 영화/TV콘텐츠 개수를 셉니다. 영화의 country컬럼에 해당 국가명이 포함되면 1을 추가하는 방식입니다.
    for i in range(len(data)):  
        if type(data.loc[i, 'country']) == str and data.loc[i, 'country'].__contains__(country):
            cnt +=1
    countries_cnt.append(cnt) 

# count를 기준으로 정렬합니다.
countries_movie_number = pd.DataFrame({'country':countires_re, 'count':countries_cnt}).sort_values(by='count', ascending=False)
```

</div>

> 데이터 프레임을 만든 후에는 이를 pyecharts를 이용해 바 그래프로 시각화해 봅니다.  
* 콘텐츠 수 상위 10개 국가만 시각화했습니다.
* 먼저 Bar 그래프 객체를 생성한 뒤 테마를 설정하고 그래프 이름, x축 및 y축 데이터 등을 설정하는 방식입니다.
* 그래프 객체 설정 시 전체 그래프의 크기(여기서는 width만 설정함)를 정할 수 있습니다.
* x축의 폰트크기는 9입니다.
* label_text_size에서는 그래프 라벨의 폰트크기를 정합니다.
* is_datazoom_show를 False로 설정해 줌이 불가능하게 했습니다.
* 따로 설정하지는 않았는데 bar.use_theme()으로 테마를 설정할 수 있습니다.

<div class="prompt input_prompt">
In&nbsp;[7]:
</div>

<div class="input_area" markdown="1">

```python
bar = Bar("국가별 넷플릭스 콘텐츠 개수",
         width=900)
bar.add("", 
        countries_movie_number['country'][:10], 
        countries_movie_number['count'][:10],
        xaxis_label_textsize=9,
        xaxis_interval=0,
        label_text_size=10,
        is_label_show=True, 
        is_datazoom_show=False)
bar
```

</div>




<div markdown="0">
<script>
    require.config({
        paths: {
            'echarts': '/nbextensions/echarts/echarts.min'
        }
    });
</script>
    <div id="6874e37f8c4543b3a4796fc3ffd907e2" style="width:900px;height:400px;"></div>


<script>
    require(['echarts'], function(echarts) {

var myChart_6874e37f8c4543b3a4796fc3ffd907e2 = echarts.init(document.getElementById('6874e37f8c4543b3a4796fc3ffd907e2'), 'light', {renderer: 'canvas'});

var option_6874e37f8c4543b3a4796fc3ffd907e2 = {
    "title": [
        {
            "text": "\uad6d\uac00\ubcc4 \ub137\ud50c\ub9ad\uc2a4 \ucf58\ud150\uce20 \uac1c\uc218",
            "left": "auto",
            "top": "auto",
            "textStyle": {
                "fontSize": 18
            },
            "subtextStyle": {
                "fontSize": 12
            }
        }
    ],
    "toolbox": {
        "show": true,
        "orient": "vertical",
        "left": "95%",
        "top": "center",
        "feature": {
            "saveAsImage": {
                "show": true,
                "title": "save as image"
            },
            "restore": {
                "show": true,
                "title": "restore"
            },
            "dataView": {
                "show": true,
                "title": "data view"
            }
        }
    },
    "series_id": 467944,
    "tooltip": {
        "trigger": "item",
        "triggerOn": "mousemove|click",
        "axisPointer": {
            "type": "line"
        },
        "textStyle": {
            "fontSize": 14
        },
        "backgroundColor": "rgba(50,50,50,0.7)",
        "borderColor": "#333",
        "borderWidth": 0
    },
    "series": [
        {
            "type": "bar",
            "data": [
                3690.0,
                1046.0,
                806.0,
                445.0,
                393.0,
                318.0,
                232.0,
                231.0,
                231.0,
                169.0
            ],
            "barCategoryGap": "20%",
            "label": {
                "normal": {
                    "show": true,
                    "position": "top",
                    "textStyle": {
                        "fontSize": 10
                    }
                },
                "emphasis": {
                    "show": true,
                    "textStyle": {
                        "fontSize": 12
                    }
                }
            },
            "markPoint": {
                "data": []
            },
            "markLine": {
                "data": []
            },
            "seriesId": 467944
        }
    ],
    "legend": [
        {
            "data": [
                ""
            ],
            "selectedMode": "multiple",
            "show": true,
            "left": "center",
            "top": "top",
            "orient": "horizontal",
            "textStyle": {
                "fontSize": 12
            }
        }
    ],
    "animation": true,
    "xAxis": [
        {
            "show": true,
            "nameLocation": "middle",
            "nameGap": 25,
            "nameTextStyle": {
                "fontSize": 14
            },
            "axisTick": {
                "alignWithLabel": false
            },
            "inverse": false,
            "boundaryGap": true,
            "type": "category",
            "splitLine": {
                "show": false
            },
            "axisLine": {
                "lineStyle": {
                    "width": 1
                }
            },
            "axisLabel": {
                "interval": 0,
                "rotate": 0,
                "margin": 8,
                "textStyle": {
                    "fontSize": 9
                }
            },
            "data": [
                "United States",
                "India",
                "United Kingdom",
                "Canada",
                "France",
                "Japan",
                "Spain",
                "South Korea",
                "Germany",
                "Mexico"
            ]
        }
    ],
    "yAxis": [
        {
            "show": true,
            "nameLocation": "middle",
            "nameGap": 25,
            "nameTextStyle": {
                "fontSize": 14
            },
            "axisTick": {
                "alignWithLabel": false
            },
            "inverse": false,
            "boundaryGap": true,
            "type": "value",
            "splitLine": {
                "show": true
            },
            "axisLine": {
                "lineStyle": {
                    "width": 1
                }
            },
            "axisLabel": {
                "interval": "auto",
                "formatter": "{value} ",
                "rotate": 0,
                "margin": 8,
                "textStyle": {
                    "fontSize": 12
                }
            }
        }
    ],
    "color": [
        "#c23531",
        "#2f4554",
        "#61a0a8",
        "#d48265",
        "#749f83",
        "#ca8622",
        "#bda29a",
        "#6e7074",
        "#546570",
        "#c4ccd3",
        "#f05b72",
        "#ef5b9c",
        "#f47920",
        "#905a3d",
        "#fab27b",
        "#2a5caa",
        "#444693",
        "#726930",
        "#b2d235",
        "#6d8346",
        "#ac6767",
        "#1d953f",
        "#6950a1",
        "#918597",
        "#f6f5ec"
    ]
};
myChart_6874e37f8c4543b3a4796fc3ffd907e2.setOption(option_6874e37f8c4543b3a4796fc3ffd907e2);

    });
</script>

</div>



역시 미국🗽의 콘텐츠 양이 압도적입니다. 다음은 발리우드로 유명한 인도인데 2위임에도 미국과 3.5배 정도 차이가 납니다.    
한국은 231개로 독일과 공동 8위입니다. 멕시코가 169개로 10위를 차지했네요.

### 3. 연도별 콘텐츠 개수 분석
----------------------------
몇 년 전만 해도 넷플릭스에서 볼 게 많지 않다는 평가가 많았는데요.     
최근에는 콘텐츠 양도 절대적으로 많아졌고 오리지널 시리즈도 많이 제작되고 있어서 더이상 이런 평가는 들리지 않는 것 같습니다.    
넷플릭스가 매년 콘텐츠를 얼마나 늘려왔는지 한번 확인해볼까요.      
콘텐츠별로 넷플릭스에 추가된 날짜는 date_added 컬럼에 저장되어 있습니다. 여기서 연도만 date_added_year 컬럼으로 저장해 준 뒤 시각화해보았습니다.

<div class="prompt input_prompt">
In&nbsp;[8]:
</div>

<div class="input_area" markdown="1">

```python
for i in range(len(data)):
    if type(data.loc[i, 'date_added']) == str:
        data.loc[i, 'date_added_year'] = data.loc[i, 'date_added'][-4:]
```

</div>

<div class="prompt input_prompt">
In&nbsp;[9]:
</div>

<div class="input_area" markdown="1">

```python
contents_added_year = pd.DataFrame(data['date_added_year'].value_counts()).reset_index().rename(columns={'index':'year', 'date_added_year':'count'}).sort_values(by='year')
```

</div>

<div class="prompt input_prompt">
In&nbsp;[10]:
</div>

<div class="input_area" markdown="1">

```python
bar = Bar("연도별 추가된 콘텐츠 수",
         width=900)
bar.add("", 
        contents_added_year['year'], 
        contents_added_year['count'],
        xaxis_label_textsize=9,
        xaxis_interval=0,
        label_text_size=10,
        is_label_show=True, 
        is_stack=True,
        is_datazoom_show=False)
bar
```

</div>




<div markdown="0">
<script>
    require.config({
        paths: {
            'echarts': '/nbextensions/echarts/echarts.min'
        }
    });
</script>
    <div id="d3586439516549b1b965868f444b9ec4" style="width:900px;height:400px;"></div>


<script>
    require(['echarts'], function(echarts) {

var myChart_d3586439516549b1b965868f444b9ec4 = echarts.init(document.getElementById('d3586439516549b1b965868f444b9ec4'), 'light', {renderer: 'canvas'});

var option_d3586439516549b1b965868f444b9ec4 = {
    "title": [
        {
            "text": "\uc5f0\ub3c4\ubcc4 \ucd94\uac00\ub41c \ucf58\ud150\uce20 \uc218",
            "left": "auto",
            "top": "auto",
            "textStyle": {
                "fontSize": 18
            },
            "subtextStyle": {
                "fontSize": 12
            }
        }
    ],
    "toolbox": {
        "show": true,
        "orient": "vertical",
        "left": "95%",
        "top": "center",
        "feature": {
            "saveAsImage": {
                "show": true,
                "title": "save as image"
            },
            "restore": {
                "show": true,
                "title": "restore"
            },
            "dataView": {
                "show": true,
                "title": "data view"
            }
        }
    },
    "series_id": 6087743,
    "tooltip": {
        "trigger": "item",
        "triggerOn": "mousemove|click",
        "axisPointer": {
            "type": "line"
        },
        "textStyle": {
            "fontSize": 14
        },
        "backgroundColor": "rgba(50,50,50,0.7)",
        "borderColor": "#333",
        "borderWidth": 0
    },
    "series": [
        {
            "type": "bar",
            "data": [
                2.0,
                2.0,
                1.0,
                13.0,
                3.0,
                11.0,
                24.0,
                82.0,
                429.0,
                1188.0,
                1649.0,
                2016.0,
                1879.0,
                1498.0
            ],
            "stack": "stack_6087743",
            "barCategoryGap": "20%",
            "label": {
                "normal": {
                    "show": true,
                    "position": "top",
                    "textStyle": {
                        "fontSize": 10
                    }
                },
                "emphasis": {
                    "show": true,
                    "textStyle": {
                        "fontSize": 12
                    }
                }
            },
            "markPoint": {
                "data": []
            },
            "markLine": {
                "data": []
            },
            "seriesId": 6087743
        }
    ],
    "legend": [
        {
            "data": [
                ""
            ],
            "selectedMode": "multiple",
            "show": true,
            "left": "center",
            "top": "top",
            "orient": "horizontal",
            "textStyle": {
                "fontSize": 12
            }
        }
    ],
    "animation": true,
    "xAxis": [
        {
            "show": true,
            "nameLocation": "middle",
            "nameGap": 25,
            "nameTextStyle": {
                "fontSize": 14
            },
            "axisTick": {
                "alignWithLabel": false
            },
            "inverse": false,
            "boundaryGap": true,
            "type": "category",
            "splitLine": {
                "show": false
            },
            "axisLine": {
                "lineStyle": {
                    "width": 1
                }
            },
            "axisLabel": {
                "interval": 0,
                "rotate": 0,
                "margin": 8,
                "textStyle": {
                    "fontSize": 9
                }
            },
            "data": [
                2008.0,
                2009.0,
                2010.0,
                2011.0,
                2012.0,
                2013.0,
                2014.0,
                2015.0,
                2016.0,
                2017.0,
                2018.0,
                2019.0,
                2020.0,
                2021.0
            ]
        }
    ],
    "yAxis": [
        {
            "show": true,
            "nameLocation": "middle",
            "nameGap": 25,
            "nameTextStyle": {
                "fontSize": 14
            },
            "axisTick": {
                "alignWithLabel": false
            },
            "inverse": false,
            "boundaryGap": true,
            "type": "value",
            "splitLine": {
                "show": true
            },
            "axisLine": {
                "lineStyle": {
                    "width": 1
                }
            },
            "axisLabel": {
                "interval": "auto",
                "formatter": "{value} ",
                "rotate": 0,
                "margin": 8,
                "textStyle": {
                    "fontSize": 12
                }
            }
        }
    ],
    "color": [
        "#c23531",
        "#2f4554",
        "#61a0a8",
        "#d48265",
        "#749f83",
        "#ca8622",
        "#bda29a",
        "#6e7074",
        "#546570",
        "#c4ccd3",
        "#f05b72",
        "#ef5b9c",
        "#f47920",
        "#905a3d",
        "#fab27b",
        "#2a5caa",
        "#444693",
        "#726930",
        "#b2d235",
        "#6d8346",
        "#ac6767",
        "#1d953f",
        "#6950a1",
        "#918597",
        "#f6f5ec"
    ]
};
myChart_d3586439516549b1b965868f444b9ec4.setOption(option_d3586439516549b1b965868f444b9ec4);

    });
</script>

</div>



### 4. 유형/장르 분석
-----------------------
어떤 유형/장르의 콘텐츠가 넷플릭스에 많은지도 개인적으로 궁금했던 부분입니다.    
유형은 TV쇼인지 영화인지 구분하는 걸 의미해서 분석이 용이합니다.     
반면에 영화와 TV쇼의 장르 구분이 별개로 되어 있고 한 콘텐츠가 여러 장르로 구분되어 있어서 구분이 명확하지 않긴 하지만, 일단 주어진 데이터로 돌려봤습니다.
    
#### 4-1. 유형 분석
영화와 TV쇼 중 어느 쪽 비중이 큰지를 파이차트로 시각화했습니다.    
영화가 거의 70%에 육박하네요. TV쇼도 많다고 생각했는데, 아직은 영화의 비중이 높군요.

<div class="prompt input_prompt">
In&nbsp;[11]:
</div>

<div class="input_area" markdown="1">

```python
pie = Pie("유형별 넷플릭스 콘텐츠 비중")
pie.add("", data['type'].value_counts().index, data['type'].value_counts().values, is_label_show=True)
pie
```

</div>




<div markdown="0">
<script>
    require.config({
        paths: {
            'echarts': '/nbextensions/echarts/echarts.min'
        }
    });
</script>
    <div id="082850cdd599474997d743be3c54a611" style="width:800px;height:400px;"></div>


<script>
    require(['echarts'], function(echarts) {

var myChart_082850cdd599474997d743be3c54a611 = echarts.init(document.getElementById('082850cdd599474997d743be3c54a611'), 'light', {renderer: 'canvas'});

var option_082850cdd599474997d743be3c54a611 = {
    "title": [
        {
            "text": "\uc720\ud615\ubcc4 \ub137\ud50c\ub9ad\uc2a4 \ucf58\ud150\uce20 \ube44\uc911",
            "left": "auto",
            "top": "auto",
            "textStyle": {
                "fontSize": 18
            },
            "subtextStyle": {
                "fontSize": 12
            }
        }
    ],
    "toolbox": {
        "show": true,
        "orient": "vertical",
        "left": "95%",
        "top": "center",
        "feature": {
            "saveAsImage": {
                "show": true,
                "title": "save as image"
            },
            "restore": {
                "show": true,
                "title": "restore"
            },
            "dataView": {
                "show": true,
                "title": "data view"
            }
        }
    },
    "series_id": 2993534,
    "tooltip": {
        "trigger": "item",
        "triggerOn": "mousemove|click",
        "axisPointer": {
            "type": "line"
        },
        "textStyle": {
            "fontSize": 14
        },
        "backgroundColor": "rgba(50,50,50,0.7)",
        "borderColor": "#333",
        "borderWidth": 0
    },
    "series": [
        {
            "type": "pie",
            "data": [
                {
                    "name": "Movie",
                    "value": 6131.0
                },
                {
                    "name": "TV Show",
                    "value": 2676.0
                }
            ],
            "radius": [
                "0%",
                "75%"
            ],
            "center": [
                "50%",
                "50%"
            ],
            "label": {
                "normal": {
                    "show": true,
                    "position": "outside",
                    "textStyle": {
                        "fontSize": 12
                    },
                    "formatter": "{b}: {d}%"
                },
                "emphasis": {
                    "show": true,
                    "textStyle": {
                        "fontSize": 12
                    },
                    "formatter": "{b}: {d}%"
                }
            },
            "seriesId": 2993534
        }
    ],
    "legend": [
        {
            "data": [
                "Movie",
                "TV Show"
            ],
            "selectedMode": "multiple",
            "show": true,
            "left": "center",
            "top": "top",
            "orient": "horizontal",
            "textStyle": {
                "fontSize": 12
            }
        }
    ],
    "animation": true,
    "color": [
        "#c23531",
        "#2f4554",
        "#61a0a8",
        "#d48265",
        "#749f83",
        "#ca8622",
        "#bda29a",
        "#6e7074",
        "#546570",
        "#c4ccd3",
        "#f05b72",
        "#ef5b9c",
        "#f47920",
        "#905a3d",
        "#fab27b",
        "#2a5caa",
        "#444693",
        "#726930",
        "#b2d235",
        "#6d8346",
        "#ac6767",
        "#1d953f",
        "#6950a1",
        "#918597",
        "#f6f5ec"
    ]
};
myChart_082850cdd599474997d743be3c54a611.setOption(option_082850cdd599474997d743be3c54a611);

    });
</script>

</div>



#### 4-2. 장르 분석
다음은 장르분석입니다. 국가별 콘텐츠 개수 분석과 마찬가지로 장르별로 콘텐츠 개수를 집계해줍니다.

<div class="prompt input_prompt">
In&nbsp;[12]:
</div>

<div class="input_area" markdown="1">

```python
categories = []

for i in range(len(data)):
    cats = data.loc[i, 'listed_in'].split(', ')
    for cat in cats:
        categories.append(cat)

categories = set(categories)
```

</div>

<div class="prompt input_prompt">
In&nbsp;[13]:
</div>

<div class="input_area" markdown="1">

```python
show_ids = []
categories_list = []

for i in range(len(data)):
    cats = data.loc[i, 'listed_in'].split(', ')
    for cat in cats:
        show_ids.append(data.loc[i, 'show_id'])
        categories_list.append(cat)
```

</div>

<div class="prompt input_prompt">
In&nbsp;[14]:
</div>

<div class="input_area" markdown="1">

```python
show_category = pd.DataFrame({'show_id':show_ids, 'category':categories_list})
```

</div>

<div class="prompt input_prompt">
In&nbsp;[15]:
</div>

<div class="input_area" markdown="1">

```python
bar = Bar("장르별 콘텐츠 개수",
         width=900)
bar.add("", 
        show_category.category.value_counts()[:10].keys(), 
        show_category.category.value_counts()[:10].values,
        bar_category_gap='10%',
        xaxis_type='category',
        xaxis_label_textsize=9,
        xaxis_interval=0,
        label_text_size=10,
        is_label_show=True, 
        is_datazoom_show=False)
bar
```

</div>




<div markdown="0">
<script>
    require.config({
        paths: {
            'echarts': '/nbextensions/echarts/echarts.min'
        }
    });
</script>
    <div id="fd31c07c7ce14fbaa5a7c595122e1d2e" style="width:900px;height:400px;"></div>


<script>
    require(['echarts'], function(echarts) {

var myChart_fd31c07c7ce14fbaa5a7c595122e1d2e = echarts.init(document.getElementById('fd31c07c7ce14fbaa5a7c595122e1d2e'), 'light', {renderer: 'canvas'});

var option_fd31c07c7ce14fbaa5a7c595122e1d2e = {
    "title": [
        {
            "text": "\uc7a5\ub974\ubcc4 \ucf58\ud150\uce20 \uac1c\uc218",
            "left": "auto",
            "top": "auto",
            "textStyle": {
                "fontSize": 18
            },
            "subtextStyle": {
                "fontSize": 12
            }
        }
    ],
    "toolbox": {
        "show": true,
        "orient": "vertical",
        "left": "95%",
        "top": "center",
        "feature": {
            "saveAsImage": {
                "show": true,
                "title": "save as image"
            },
            "restore": {
                "show": true,
                "title": "restore"
            },
            "dataView": {
                "show": true,
                "title": "data view"
            }
        }
    },
    "series_id": 3226739,
    "tooltip": {
        "trigger": "item",
        "triggerOn": "mousemove|click",
        "axisPointer": {
            "type": "line"
        },
        "textStyle": {
            "fontSize": 14
        },
        "backgroundColor": "rgba(50,50,50,0.7)",
        "borderColor": "#333",
        "borderWidth": 0
    },
    "series": [
        {
            "type": "bar",
            "data": [
                2752.0,
                2427.0,
                1674.0,
                1351.0,
                869.0,
                859.0,
                763.0,
                756.0,
                641.0,
                616.0
            ],
            "barCategoryGap": "10%",
            "label": {
                "normal": {
                    "show": true,
                    "position": "top",
                    "textStyle": {
                        "fontSize": 10
                    }
                },
                "emphasis": {
                    "show": true,
                    "textStyle": {
                        "fontSize": 12
                    }
                }
            },
            "markPoint": {
                "data": []
            },
            "markLine": {
                "data": []
            },
            "seriesId": 3226739
        }
    ],
    "legend": [
        {
            "data": [
                ""
            ],
            "selectedMode": "multiple",
            "show": true,
            "left": "center",
            "top": "top",
            "orient": "horizontal",
            "textStyle": {
                "fontSize": 12
            }
        }
    ],
    "animation": true,
    "xAxis": [
        {
            "show": true,
            "nameLocation": "middle",
            "nameGap": 25,
            "nameTextStyle": {
                "fontSize": 14
            },
            "axisTick": {
                "alignWithLabel": false
            },
            "inverse": false,
            "boundaryGap": true,
            "type": "category",
            "splitLine": {
                "show": false
            },
            "axisLine": {
                "lineStyle": {
                    "width": 1
                }
            },
            "axisLabel": {
                "interval": 0,
                "rotate": 0,
                "margin": 8,
                "textStyle": {
                    "fontSize": 9
                }
            },
            "data": [
                "International Movies",
                "Dramas",
                "Comedies",
                "International TV Shows",
                "Documentaries",
                "Action & Adventure",
                "TV Dramas",
                "Independent Movies",
                "Children & Family Movies",
                "Romantic Movies"
            ]
        }
    ],
    "yAxis": [
        {
            "show": true,
            "nameLocation": "middle",
            "nameGap": 25,
            "nameTextStyle": {
                "fontSize": 14
            },
            "axisTick": {
                "alignWithLabel": false
            },
            "inverse": false,
            "boundaryGap": true,
            "type": "value",
            "splitLine": {
                "show": true
            },
            "axisLine": {
                "lineStyle": {
                    "width": 1
                }
            },
            "axisLabel": {
                "interval": "auto",
                "formatter": "{value} ",
                "rotate": 0,
                "margin": 8,
                "textStyle": {
                    "fontSize": 12
                }
            }
        }
    ],
    "color": [
        "#c23531",
        "#2f4554",
        "#61a0a8",
        "#d48265",
        "#749f83",
        "#ca8622",
        "#bda29a",
        "#6e7074",
        "#546570",
        "#c4ccd3",
        "#f05b72",
        "#ef5b9c",
        "#f47920",
        "#905a3d",
        "#fab27b",
        "#2a5caa",
        "#444693",
        "#726930",
        "#b2d235",
        "#6d8346",
        "#ac6767",
        "#1d953f",
        "#6950a1",
        "#918597",
        "#f6f5ec"
    ]
};
myChart_fd31c07c7ce14fbaa5a7c595122e1d2e.setOption(option_fd31c07c7ce14fbaa5a7c595122e1d2e);

    });
</script>

</div>



미국 외 다른 나라의 영화/TV쇼를 의미하는 International Movies 장르의 콘텐츠가 가장 많네요. International TV Show도 1351개로 4위이구요. 넷플릭스가 글로벌한 서비스이다보니 자연스레 나타난 결과인 것 같습니다.    
드라마가 2위인데 1위와 별로 차이가 안나는 게 오히려 의외입니다. 3위인 코미디와도 개수 차이가 크고요.    
다큐멘터리가 액션 영화보다 약간 더 많은 점, TV 드라마가 이 두 장르보다 적은 것도 개인적으로는 의외입니다🤔   
넷플릭스에서 다큐멘터리가 생각보다 인기라는 얘기를 들었는데 이런 점이 반영된 것일 수도 있겠네요.
    
-----------------------------

여기까지 넷플릭스 콘텐츠 데이터를 분석해보았는데요, 생각보다 글이 길어져서 2편에서 마저 적으려고 합니다😅      
읽어주셔서 감사하고 궁금하신 부분 있으면 댓글로 달아주세요!    

