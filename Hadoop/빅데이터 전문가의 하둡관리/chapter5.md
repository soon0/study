## 아파치 하이브
* Hbase와 달리 데이터 베이스는 아님
* HiveQL을 이용해 HDFS 내의 데이터를 SQL처리 할 수 있도록 하는 데이터웨어하우스 인프라스트럭처
* 동작 : 쿼리 실행 -> 맵리듀스 잡 생성
* 장점
	* 맵리듀스 코드를 짜지 않아도 됨, sql쿼리만 있으면 됨
	* Bi툴과도 잘 통합됨
* 단점
	* 업데이트/삭제 지원하지 않음
	* 맵리듀스 엔진을 사용하여 sql쿼리보다 오래 걸림
		
### 데이터 구성
* 데이터베이스
* 테이블
* 파티션
* 버킷
	
### 하이브로 테이블 작업
```
# 테이블 생성
CREATE TABLE page_view (viewTime INT, userid BIGINT, page_url STRING, refferrer_url STRING, 
	ip STRING COMMENT 'IP Address of the User'
COMMENT 'This is the page view table'
PARTITIONED BY (dt STRING, country STRING)
STORED AS SEQUENCEFILE;

# 하이브 테이블 목록 확인
SHOW TABLES;

# 파티션 확인
DESCRIBE page_view;

# 컬럼 & 컬럼 종류
DESCRIBE EXTENDED page_view;
```

### 데이터를 하이브로 로딩
```
CREATE TABLE page_view (viewTime INT, userid BIGINT, page_url STRING, refferrer_url STRING, 
	ip STRING COMMENT 'IP Address of the User'
COMMENT 'This is the page view table'
ROW FORMAT DELIMITED FIELDS TERMINATETD BY '44' LINES TERMINATED BY '12'
PARTITIONED BY (dt STRING, country STRING)
STORED AS TEXTFILE
LOCATION '/user/data/staging/page_view' ;

Hadoop dfs -put /tmp/pv_2016-03-09.txt /user/data/staging/page_view
```
```
FROM page_view_stg pvs
INSERT OVERWRITE TABLE page_view PARTITION(dt='2016-03-09', country='US')
SELECT pvs.viewtime, pvs.userid, pvs.page_url, pvs.referrer_url, null, null, pvs.ip
WHERE pvs.country = 'US';
```

### 하이브 쿼리
```
# 테이블에 데이터 추가
INSERT OVERWRITE TABLE xyz_com_page_views
SELECT page_views.*
FROM page_views
WHERE page_views.date >= '2016-03-01' AND page_views.date <= '2016-03-09' AND page_views.reffer_url like '%xyz.com';

# 테이블 결합
INSERT OVERWRITE TABLE pv_users
SELECT pv.*, u.gender, u.age
FROM user u JOIN page_view pv ON (pv.userid = u.id)
WHERE pv.date = '2016-03-09';
```
