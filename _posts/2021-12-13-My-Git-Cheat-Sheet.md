---
title: "My Git Cheat Sheet (1)"
excerpt: "개인적으로 자주 사용하는 깃 명령어를 정리해봤어요"
author_profile: true
search: true
categories: 
  - Version Control
tags: 
  - Git
toc : true
toc-sticky : true
layout: single
date: 2021-12-13
---



개인적으로 자주 사용하는 깃 명령어를 정리한 목록입니다.    

​    

### **COMMIT**      

add & commit   

```
git commit -am "커밋메시지"   
```

방금 작성한 커밋 메시지 수정하기   

``` 
git commit --amend
```

add 하기 전 수정사항 확인하기   

```
git diff  
```

commit 하기 전 수정사항 확인하기   

``` 
git diff head
```

​      

### **LOG**      

간략하게 한줄로 확인하기   

```
git log --pretty=oneline 
```

그래프로 확인하기   

```
git log --graph --all
```

diff 포함해서 보기   

``` 
git log -p
```

​      

### **CONFIG**   

글로벌 사용자 설정하기

```
git config --global user.name "사용자이름"
git config --global user.email 사용자이메일주소
```

시스템 설정 확인하기

```
git config --system --list
```

글로벌 설정 확인하기

``` 
git config --global --list
```

​         

### **BRANCH**     

새 브랜치 만들기    

```
git branch 브랜치이름
```

브랜치 생성 + 새로 만든 브랜치로 이동하기   

```
git checkout -b 브랜치이름
```

브랜치 리스트 확인하기      

```
git branch
```

모든 브랜치 리스트 확인하기 (로컬 + 원격)   

```
git branch -a
```

브랜치 세부사항 확인하기   

```
git branch -v
```

다른 브랜치로 이동하기   

``` 
git checkout 브랜치이름
```

이전 브랜치로 이동하기   

```
git checkout -
```

현재보다 한 단계 커밋 전 상태로 이동하기   

```
git checkout HEAD~1
```

브랜치 삭제하기   

```
git branch -d 브랜치이름
```

브랜치 삭제하기(추가 커밋 있는 경우 강제 삭제)   

```
git branch -D 브랜치이름
```

​        

### **REMOTE BRANCH**   

원격 저장소와 연결하기   

``` 
git remote add origin 원격저장소주소
```

원격 저장소 목록 확인하기   

```
git remote -v
```

원격 브랜치 정보 확인하기   

```
ls .git/refs/
```

원격 저장소 브랜치 목록 확인하기   

```
git branch -r
```

원격저장소로 push하기   

```
git push 원격저장소별칭 브랜치이름
```

로컬 브랜치와 원격 브랜치 이름이 다른 경우 push하기   

```
git push 원격저장소별칭 브랜치이름(로컬):브랜치이름(원격)
```

   

