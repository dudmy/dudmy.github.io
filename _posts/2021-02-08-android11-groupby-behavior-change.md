---
layout: post
title: "Android 11 GROUP BY 동작 변경"
categories: android
comments: true
---

일반적으로 GROUP BY 절은 반드시 그룹 함수(COUNT, MIN, SUM 등...)와 함께 쓰여야 한다. 다른 데이터베이스는 ```SELECT * FROM {table} GROUP BY {column}``` 같은 쿼리를 허용하지 않는다. SQLite는 이를 허용하고 있지만 이러한 쿼리의 결과는 예측할 수 없다. 이와 별개로 SQLite 버전에 따른 동작 변경 사항이 있어서 기록해둔다.

### TABLE dog

```
SELECT * FROM dog;
```

| rowid       | address     | name        |
| ----------- | ----------- | ----------- |
| 1           | seoul       | coco        |
| 2           | seoul       | ari         |
| 3           | busan       | kiara       |
| 4           | seoul       | casper      |
| 5           | incheon     | marie       |
| 6           | seoul       | fleur       |
| 7           | busan       | rai         |
| 8           | jeju        | rai         |
| 9           | incheon     | bella       |
| 10          | busan       | lucy        |
| 11          | seoul       | molly       |
| 12          | jeju        | daisy       |

### Android 10 SQLite v3.22.0

```
SELECT * FROM dog GROUP BY address;
```

| rowid       | address     | name        |
| ----------- | ----------- | ----------- |
| 10          | busan       | lucy        |
| 9           | incheon     | bella       |
| 12          | jeju        | daisy       |
| 11          | seoul       | molly       |

### Android 11 SQLite v3.28.0

```
SELECT * FROM dog GROUP BY address;
```

| rowid       | address     | name        |
| ----------- | ----------- | ----------- |
| 3           | busan       | kiara       |
| 5           | incheon     | marie       |
| 8           | jeju        | rai         |
| 1           | seoul       | coco        |

### 쿼리 결과

v3.22.0은 rowid 컬럼이 최대값인 데이터를, v3.28.0은 rowid 컬럼이 최소값인 데이터를 가져오는 차이가 있다.
