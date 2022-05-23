> Data Analysis using Hadoop | Data Analytics in Big Data | Intellipaat </br>
> https://www.youtube.com/watch?v=WRP_CAmvBS0
</br>

## 1. what is data warehousing?
* 데이터에서 insight를 도출하기 위해 data warehouse를 구축하고 활용하는 것
* 데이터 웨어하우스는 다양한 heterogenous sources에서 데이터를 수집할 때 구축
* ex) customer datad & sales data는 분리되어 보관되어있어 etl툴을 이용하여 data warehouse에 저장 조직에서는 data warehouse를 보고 insight & reporting 

## 2. what is hive </br>
아파치 하둡 위에 구축 된 데이터 웨어하우징 도구
		
## 3. how does hive work?
* sql like interface
* 추상화된 query language(hiveQL : sql과 비슷한 datawarehousing에서 동작하는 tool) 
* hadoop과 통합된 데이터베이스 및 파일 시스템에 저장된 데이터를 쿼리
* HDFS(hadoop distributed file system)에 저장된 대규모 datasets 분석 제공
* 아마존 A3와 alluxio 같은 파일 시스템과도 호환됨
		
## 4. environment </br>
wmware, centos, hadoop, hive
		
## 5. hands-on-tutorial
#### 1. 데이터 준비 </br>
버그 때문에 csv파일은 header는 제외하고 생성해야함  **구버전의 경우인듯

#### 2. 하둡에 csv파일 올리기

```
$ hadoop fs -mkdir facebookdata
$ hadoop fs -put pseudo_facebook.csv facebookdata
$ hadoop fs -ls facebookdata
```

#### 3. hive database 실행
```
$ hive
> show databases;
> use [database name];
```

#### 4. 테이블 생성 및 조회 </br>
열 이름 및 타입 자유롭게 변경가능, 위 csv파일의 순서대로 입력하기만 하면 됨 --> 테이블 변경 자유로움
```
> create table fb ( id int, age int, day int, year int, month int, gender string, tenure int, friends int, 
                    ... , wlike int )  # create 쿼리
  row format delimited fields terminated by ','  # 구분자
  stored as textfile location '/user/training/facebookdata/'  # 원본 파일 위치 (파일명을 지정하지 않으면 폴더내 전체 테이블 선택 됨)
  ;
```

#### 5. 테이블 조회
```
# data warehouse 내 데이터 그냥 조회
> select * from fb limit 2; # rownum <=2 까지 조회

# MapReduce 연산 실행 
> select count(*) from fb
> select count(*) from fb where age > 25;
> select gender, avg(friends) from fb group by gender;
> select avg(likes_recd) from fb where age > 13 and age < 25;
```
