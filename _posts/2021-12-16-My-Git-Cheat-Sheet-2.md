---
title: "My Git Cheat Sheet (2)"
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
date: 2021-12-16
---



개인적으로 자주 사용하는 깃 명령어를 정리한 목록 2탄입니다 :blush:       



### **MERGE**      

현재 브랜치에 특정 브랜치를 병합하기

```
git merge 브랜치이름
```

병합 커밋 메시지와 함께 병합하기 

``` 
git merge 브랜치이름 --edit
```

충돌이 발생했을 때 충돌 파일 확인하기   

```
git ls-files -u  
```

병합 브랜치 확인하기(병합한 브랜치에 * 표시)   

``` 
git branch --merged
```

​    

### **REBASE**      

특정 브랜치를 리베이스하기   

```
git rebase 브랜치이름
```

충돌 발생 시 이를 해결하고 다시 리베이스하기   

```
git rebase --continue
```

리베이스 취소하기      

``` 
git rebase --abort
```



### **RESET**   

####  최신 커밋을 리셋하기

##### 1. RESET --SOFT : 스테이지 영역 포함한 상태로 복원하기

``` 
git reset --soft HEAD~
```

- 최신 커밋에서 수정된 내용을 스테이지 영역으로 보냄.
- add (O) commit (X)

##### 2. RESET --MIXED: 기본 옵션

``` 
git reset --mixed HEAD~
```

- 최신 커밋에서 수정된 내용은 unstaged상태가 됨.
- add (X) commit (X)

##### 3. RESET --HARD: 파일 삭제된 상태로 복원하기

``` 
git reset --hard HEAD~
```

- 해당 커밋 이후 내역 모두 삭제

   

   

### REVERT

현재 커밋 리버트하기    

```
git revert HEAD
```

특정 커밋을 리버트하기    

```
git revert 커밋ID
```

병합 취소하기(병합 취소 후 checkout할 커밋번호 + 취소할 병합커밋ID)    

```
git revert --mainline 커밋번호 병합커밋ID
```

