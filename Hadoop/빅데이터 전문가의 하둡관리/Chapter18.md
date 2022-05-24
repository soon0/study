## 하이브 잡 최적화
### 1. 파티션 & 버켓 사용
* CREATE TABLE 문장에 PARTITION_BY 절 사용 : 파티션
* CREATE TABLE 문장에 CLUSTERED BY 절 사용 : 버켓
```
# 버켓 플래스 설정
SET hive.enforce.bucketing = true;
```
  
### 2. 병렬 실행
	```
	SET hive.exec.parallel = true;
	```
	
### 3. ORCFILE 포맷 사용
> 참고 : https://sparkdia.tistory.com/57
</br>

### 4. 하이브 비용 기반 최적화 선택
비용최적화가 자동으로 이뤄져 최적 쿼리 플랜을 선택하지만 아래 매개변수로 사용자가 지정할 수 있음
```
set hive.cbo.enable=ture;
set hive.compute.query.using.stats=true;
set hive.fetch.column.stats=true;
set hive.fetch.patition.stats=true;
```
	
### 5. 하이브 내장 기능 사용
Over (parttion by ) 등의 OLAP분석함수 사용

### 6. 벡터화 사용 
(로우 하나하나 작업하지 않고 여러 개의 로우를 한 번에 처리하는 것)
```
set hive.vectorized.excution.enabled = true;
set hive.vectorized.excution.reduce.enabled = true;
```
