---
title: Spring 게시판 CRUD 예제
date: 2022-01-16 15:06:05
category: 'Spring'
draft: true
---

게시판 DB table 만들기

```
CREATE TABLE board (
bno INT not null primary key,
title varchar(200) not null,
content varchar(4000),
writer varchar(50) not null,
regdate datetime default CURRENT_TIMESTAMP,
viewcnt INT default 0
);
```

# Reference

-
- https://lhoris.tistory.com/33
