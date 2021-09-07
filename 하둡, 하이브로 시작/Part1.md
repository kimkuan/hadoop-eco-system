## 🐘 하둡, 하이브로 시작 Part1️⃣
<br>

---

📌 **[ INDEX ]** 

1. [빅데이터란?](https://www.notion.so/Part-1-9cf76dbf00d94f8fab85963d373500ba)
2. [빅데이터 처리단계](https://www.notion.so/Part-1-9cf76dbf00d94f8fab85963d373500ba)
3. [빅데이터 에코시스템](https://www.notion.so/Part-1-9cf76dbf00d94f8fab85963d373500ba)
4. [빅데이터 시스템](https://www.notion.so/Part-1-9cf76dbf00d94f8fab85963d373500ba)

---
<br>

## # 1. 빅데이터란?


### 0. 빅데이터란?

- `더보기`
    - 큰 사이즈의 데이터에서 유의미한 지표를 분석해내는 것
    - 다양한 경로를 통해 수집한 여러 형태의 데이터를 이용하여 의사결정에 도움을 주는 지표를 분석하여 제공하는 것이다.

    ### 출현배경

    - SNS 등장, 스마트 기기 보급으로 인해 데이터 양이 증가하고 데이터 저장 기술의 발달
    - 저장장치의 가격 인하
    - 데이터 처리 기술의 발달
        - 분산 병렬처리 기술의 발달로 합리적인 시간안에 데이터 분석이 가능해졌다.
        - 또한, CPU의 발전, 클라우딩 컴퓨팅, Hadoop 등 오픈소스 활성화로 Scale Out이 편리해졌다.

    ### 빅데이터 특징 - 3V

    - Volume - 크기

        : SNS의 등장, 네트워크 속도의 향상으로 수 페타바이트의 데이터가 매일 생성됨

    - Variety - 다양성

        : 정형, 반정형, 비정형 형태의 다양한 데이터를 분석

    - Velocity - 속도

        : 정보 유통속도가 굉장히 빠르고, 데이터의 처리 속도가 빠름.

    + 아래 2개의 특징을 더해서 5V 라고 칭하기도 한다.

    - Value - 가치

        : 유의미한 가치를 가지는 지표

    - Veracity - 정확성

        : 빅데이터를 이용해서 뽑아낸 데이터의 신뢰성, 정확성이 높음 → 데이터가 많아질 수록 더 정확.

### 1. 데이터의 형태

- `더보기`

    ### 수집 형태

    - 데이터는 수집 형태에 따라 `정형, 반정형, 비정형`으로 구분된다.
    - 빅데이터는 정형 데이터보다는 비정형, 반정형 데이터가 더 많이 수집된다.

        → 이를 다양한 도구를 이용하여 정형 형태로 변형하여 분석에 이용하는 경우가 많다!

    ```markdown
    - **정형** : DB, CSV, 엑셀과 같이 칼럼 단위의 명확한 구분자와 형태가 존재하는 데이터
    - **반정형** : XML, HTML, JSON과 같이 메타데이터나 스키마가 존재하는 다양한 형태의 데이터
    - **비정형** : 동영상, SNS 메시지, 사진, 오디오, 음성처럼 형태가 존재하지 않는 데이터
    ```

    ### 수집 시간

    - 데이터는 수집과 처리하는 시간에 따라 `배치 데이터, 실시간 데이터`로 구분할 수 있다.

    ```markdown
    - **배치** : 시, 일, 주, 월 단위 등 일정한 주기로 수집, 처리되는 데이터
    - **실시간** : 실시간 검색어, 실시간 차트처럼 사용자의 입력과 동시에 처리되는 데이터
    ```

### 1-2. 분석 형태

- `더보기`
    - 빅데이터를 분석하는 방법은 다음과 같이 구분할 수 있다.

    ```markdown
    **- 대화형 분석
    	-** 사용자가 입력한 쿼리에 바로 반응하여 반환하는 분석 방법
    	- ex) 대화형 대시보드

    **- 배치 분석
    	-** 저장된 데이터를 일정한 주기로 분석하는 방법
    	- ex) 일/주/월간 보고서

    **- 실시간 분석
    	-** 사용자의 여러 입력이 실시간으로 저장되고 분석하는 방법
    	- ex) 1분마다 결제/사기 경고
     
    **- 기계 학습**
    	- 기계 학습 알고리즘을 이용해 예측 모델을 생성하는 방법
    	- ex) 심리 분석, 예측 모델
    ```

## # 2. 빅데이터 처리단계


### 1. 수집

- `더보기`
    - 빅데이터는 내부/외부의 여러 원천에서 데이터를 수집하게 된다.
    - 따라서 다양한 형식과 방법으로 데이터를 수집하게 된다.

    ### 내부/외부 데이터

    ```markdown
    - **내부 데이터**
        - 시스템 로그, DB 데이터
    - **외부 데이터**
        - 동영상, 오디오 정보
        - 웹 크롤링 데이터
        - SNS 데이터
    ```

    ### 수집 방식

    - 기존 데이터 수집 방식
        - HTTP 웹 서비스
        - RDB (= Relational Database, 관계형 데이터베이스)
        - FTP (= File Transfer Protocol, 파일 전송 프로토콜)
        - JMS (= Java Message Service , Java EE에 기반한 애플리케이션 구성요소에서 메시지를 작성, 전송, 수신하고 읽을 수 있도록 하는 API)
        - Text
    - 새로운 방식의 데이터 수집
        - SNS의 여러가지 데이터 ⇒ Text, 이미지, 동영상
        - 전화 음성, GPS
        - IoT 디바이스 센서
            - 공간 데이터 + 인구 데이터

    ### 데이터 수집 트랜잭션

    - 데이터를 수집할 때 연동하는 데이터가 적다면 개별적으로 관리가 가능하다.
    - 하지만 수백, 수천개가 되면 트랜잭션 관리가 어려울 수 있다.
    - 따라서, 데이터의 유실, 데이터의 전송 여부 확인을 위한 **트랜잭션 처리가 중요**하다.

    ### 데이터 수집 기술

    - Flume

        : 많은 양의 로그 데이터를 효율적으로 수집, 취합, 이동하기 위한 분산형 소프트웨어

    - Kafka

        : Pub-Sub 모델의 오픈 소스 메시지 브로커. 분산환경에 특화되어 있다.

    - Sqoop

        : 관계형 데이터베이스와 아파치 하둡간의 대용량 데이터들을 효율적으로 변환해주는 툴

    - Nifi

        : 소프트웨어 시스템 간 데이터 전송을 자동화하도록 설계된 소프트웨어 

    - Flink

        : Streaming 데이터를 처리하기 위한 오픈소스 프레임워크

        배치 방식이 아닌 native 방식으로 스트림 처리를 하기 때문에 low latency(낮은 대기시간) 특성을 갖는다. 또한, Exactly-Once, 즉 한번의 메세지 전송을 보장한다.

        ↔ Apache Storm ⇒ low-level API를 제공하는 native streaming 시스템

        ↔ Spark ⇒ batch processing 플랫폼

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/381fd327-94ca-4a76-936d-fda62a5c36dd/Untitled.png)

    - Splunk

        : 기계가 생성한 빅데이터를, 웹 스타일 인터페이스를 통해 검색, 모니터링, 분석하는 소프트웨어

    - Logstash

        : 실시간 파이프라인 기능을 가진 오픈소스 데이터 수집 엔진. 서로 다른 소스의 데이터를 동적으로 통합하고 원하는 대상으로 데이터를 정규화하는 기능을 갖는다.

    - Fluentd

        : 보통 로그를 수집하는데 사용하지만 다양한 데이터 소스(HTTP, TCP 등)으로 부터 데이터를 받아올 수 있다. Fluentd로 전달된 데이터는 tag, time, recode(JSON)로 구성된 이벤트로 처리되며, 원하는 형태로 가공되어 다양한 목적지(Elasticsearch, S3, HDFS)로 전달될 수 있다. + Logstash 보다 가볍다는 장점이 있다.

### 2. 정제

- `더보기`
    - 정제 단계는 **데이터를 분석 가능한 형태로 정리하는 것**이다.
    - 여러 경로에서 수집된 데이터의 형식이 다르기 때문에 분석 단계에서 사용할 도구에 맞게 형태를 변환한다.
    - 이때, 오류 데이터와 불필요한 데이터를 제거한다.
    - 정제한 데이터는 압축하여 데이터 사이즈를 줄여준다.

    ### 정제 단계

    1. Identification

        : 알려진 다양한 데이터 포맷이나 비정형 데이터에 할당된 기본 포맷을 식별한다.

    2. Filteration

        : 수집된 정보에서 정확하지 않은 데이터를 제외시킨다.

    3. Validation

        : 데이터의 유효성을 검증한다.

    4. Noise Reduction

        : 오류 데이터를 제거하고 분석 불가능한 데이터를 제외시킨다.

    5. Transformation

        : 데이터를 분석 가능한 형태로 변환시킨다.

    6. Compression

        : 저장장치 효율성을 위해 변환한 데이터를 압축시킨다.

    7. Integration

        : 처리 완료한 데이터를 분산 저장소에 적재한다.

### 3. 적재

- `더보기`
    - 적재 단계는 대량의 데이터를 안전하게 보관하고 분석할 수 있는 환경으로 옮기는 것이다.
    - 분석에 사용할 도구에 따라 NoSQL, RDB, 클라우드 스토리지, HDFS 등 다양한 환경에 적재한다.
    - RDB에서 추출한 데이터나, CSV 형태로 제공되는 데이터는 별도의 정제 단계없이 바로 적재할 수 있다.

    ```markdown
    - 대용량 파일 전체를 영구적으로 저장하기 위한 **HDFS**
    - 대규모 메시징 데이터 전체를 영구 저장하기 위한 **NoSQL**(HBase, MongoDB, Casandra 등)
    - 대규모 메시징 데이터의 일부만 임시 저장하기 위한 **인메모리 캐시**(Redis, Memcached, Infinispan 등)
    - 대규모 메시징 데이터 전체를 버퍼링 처리하기 위한 **Message Oriented Middleware**(Kafka, RabbitMQ, AcriveMQ 등)

    출처) https://coding-sojin2.tistory.com/entry/%EB%B9%85%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%88%98%EC%A7%91-%EC%A0%81%EC%9E%AC-%EC%B2%98%EB%A6%AC%ED%83%90%EC%83%89-%EB%B6%84%EC%84%9D%EC%9D%91%EC%9A%A9-%EA%B8%B0%EC%88%A0
    ```

### 4. 분석

- `더보기`
    - 분석 단계는 적재된 데이터를 이용하여 의사결정을 위한 데이터를 제공하기 위한 리포트를 생성하는 단계이다.
    - 대용량의 데이터를 빠르게 분석하기 위한 **처리 엔진**이 필요하다.
    - 또한, 효율적으로 분석하기 위해서 **파티셔닝, 인덱싱** 등의 기술이 필요하다.
    - **실시간 분석, 배치 분석**을 이용해 리포트를 생성한다.

### 5. 시각화

- `더보기`
    - 사용자가 빠르게 인식할 수 있는 형태로 분석결과를 시각화 한다.

## # 3. 빅데이터 에코시스템


### 0. 빅데이터 에코시스템

- `더보기`
    - 빅데이터는 `수집 → 정제 → 적재 → 분석 → 시각화`의 여러 단계를 거친다.
    - 이 단계를 거치는 동안 여러 기술을 이용하게 되는 데, 이 기술들을 통틀어 **빅데이터 에코 시스템(Bigdata Eco System)**이라고 한다.

    ```markdown
    1. 수집 기술
    	- 플룸(Flume)
    	- 카프카(Kafka)
    	- NiFi
    	- Sqoop
    	- scribe
    	- Fluentd
    2. 작업 관리 기술
    	- Airflow
    	- Azkaban
    	- Oozie
    3. 데이터 직렬화
    	- Avro
    	- Thrift
    	- Protocol Buffers
    4. 저장
    	- HDFS
    	- S3
    5. NoSQL
    	- HBase
    6. 데이터 처리
    	- MapReduce
    	- Spark
    	- Impala
    	- Presto
    	- Hive
    	- Hcatalog
    	- Pig
    7. 클러스터 관리
    	- YARN
    	- Mesos
    8. 분산 서버 관리
    	- Zookeeper
    9. 시각화
    	- Zeppelin
    	- Hue
    10. 보안
    	- Ranger
    11. 데이터 거버넌스
    	- Atlas
    	- Amundsen
    ```

    ### 1. 수집 기술

    - `더보기`
        - 수집 기술은 빅데이터 분석을 위한 원천 데이터를 수집하는 기술이다.
        - 원천 데이터는 **실시간** 데이터 수집 기술, **배치** 데이터 수집기술이 있다.
        - 원천 데이터의 종류에는 로그 데이터, DB 데이터, API 호출 데이터 등 여러가지 종류가 있다.

        **1) 플룸 (Flume)**

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/715a4aab-624e-4e9f-b059-e3cc1f82f379/Untitled.png)

        - 클라우데라에서 개발한 서버 로그 수집 도구
        - 각 서버에 에이전트가 설치되고, 에이전트로부터 데이터를 전달받는 콜렉터(Collector)로 구성된다.

        **2) 카프카 (kafka)**

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a35a61ba-c637-46e6-af53-ae41c048781c/Untitled.png)

        - 카프카는 Linked-In에서 개발한 분산 메세징 시스템이다.
        - Pub-Sub 모델로 구성되어 있으며, 대용량 실시간 로그 처리에 특화되어 있다.

        **3) NiFi**

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/26b26265-1ec4-48b3-9a98-ea690eff04d3/Untitled.png)

        - 미국 국가안보국(NSA)에서 개발한 시스템 간 **데이터 전달**을 효율적으로 처리, 관리, 모니터링 하기 위한 최적의 시스템이다.

        [Big Data Ingestion: Flume, Kafka and NiFi](https://tsicilian.wordpress.com/2017/07/06/big-data-ingestion-flume-kafka-and-nifi/)

        **4) Sqoop** 

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6870e179-cf55-4b67-bf65-8aa88bf1f8bd/Untitled.png)

        - RDBMS와 HDFS간 대용량 데이터 전송을 위한 솔루션이다.
        - RDBMS에 있는 데이터를 Hadoop 파일 시스템(HDFS, HDase, Hive)에 신속하게 import 할 수 있도록 한다. (그 반대로 export도 지원)

        **5) scribe**

        - 페이스북에서 제작한 로그 수집 시스템
        - 메시지 큐에 쌓인 로그를 DB나 Message Queue에 전달한다.
        - C++로 제작되어 속도가 빠르고, 페이스북의 대용량 데이터 처리에 사용되었을 정도로 안전성을 가지고 있다.
        - 다만, 현재는 페이스북이 Calligraphus를 사용하여서 2014년 이후 개선사항이 없다.

        **6) fluentd**

        - 트레저 데이터에서 개발한 로그 수집 시스템
        - 주로 Ruby와 C로 작성되었다.
        - 여러 형태의 로그를 전달받아서 원하는 저장소에 쌓을 수 있다.
        - 비정형, 반정형 데이터를 필터링, 버퍼링하여 사용자가 원하는 데이터베이스나 클라우드 저장소에 효율적으로 저장할 수 있다.
        - 기본적인 구조는 Flume NG와 비슷하나, Flume의 Source, Channel, Sink가 Input, Buffer, Output으로 대체되었다.
        - 장점으로 각 파트별로 플러그인을 만들기 쉽다는 점을 갖고 있다.

    ### 2. 작업 관리 기술

    - `더보기`
        - 빅데이터를 분석하는 여러가지 단계를 효율적으로 생성, 관리하고 모니터링 할 수 있게 도와주는 기술이다.

        **1) Airflow**

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7c965682-3c24-4c29-ad16-6778a1b861e7/Untitled.png)

        - 에어비앤비에서 개발한 데이터 흐름의 시작화, 스케쥴링, 모니터링이 가능한 워크플로우 플랫폼.
        - Hive, 프레스토, DBMS 엔진과 결합하여 사용할 수 있다.

        **2) Azkaban**

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/83d4d0ad-434a-4884-874a-217d5502d5e5/Untitled.png)

        - Linked-In에서 개발한 워크플로우 스케쥴러, 시각화된 절차, 인증 및 권한 관리, 작업 모니터링 및 알림 등 다양한 기능을 갖는 워크플로우 관리 도구.

        **3) Oozie**

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a750c3d9-f123-409f-8972-70ab61bf2f35/Untitled.png)

        - Hadoop 작업을 관리하는 워크플로우 및 코디네이터 시스템
        - 자**바 웹 어플리케이션 서버로 UI를 제공**하고, 맵리듀스, Hive, Pig 작업 같은 특화된 액션으로 구성된 **XML 포맷**의 워크플로우로 작업을 제어한다.

    ### 3. 데이터 직렬화

    - `더보기`
        - 빅데이터 에코 시스템이 다양한 기술과 언어로 구현되기 때문에 **각 언어간에 내부 객체를 공유**해야하는 경우가 있다.
        - 이를 효율적으로 처리하기 위해서 데이터 직렬화 기술을 이용한다

        ```markdown
        📌 **RPC(Remote Procedure Call)이란?**
        : 별도의 원격 제어를 위한 코딩 없이 다른 주소 공간에서 함수나 프로시저를 실행할 수 있게하는
        프로세스 간 통신 기술이다.

        -> 즉, RPC를 이용하면 프로그래머는 함수가 로컬위치에 있든 원격위치에 있든 동일한 코드를 이용
        할 수 있다.
        ```

        **1) Avro**

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f1412f88-7c61-446f-98ad-6a467e41c33f/Untitled.png)

        - 에이브로는 Apache의 Hadoop 프로젝트에서 개발된 원격 프로시져 호출(RPC) 및 데이터 직렬화 프레임워크다.
        - 자료형과 프로토콜 정의를 위해 **JSON**을 사용하며 콤팩트 바이너리 포맷으로 데이터를 직렬화한다.

        **2) Thrift**

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/df5c515c-3b2e-480a-96b7-39af33ddd88c/Untitled.png)

        - 스리프트는 페이스북에서 개발한 RPC 프레임워크다.
        - 서로 다른 언어로 개발된 모듈의 통합을 지원한다.
        - 데이터 타입과 서비스 인터페이스를 선언하면, RPC 형태의 클라이언트와 서버 코드를 자동으로 생성해준다.
        - C++언어로 작성되었으나, JAVA, C++, Python, Go, Node.js와 같이 다양한 언어를 지원한다.

        **3) Protocol Buffers**

        - 구글에서 개발한 RPC 프레임워크이다.
        - 구조화된 데이터를 직렬화하는 방식을 제공한다.
        - C++,C#, Go, Java, Python, Object C, Javascript, Ruby 등 다양한 언어를 지원한다.
        - 특히 **직렬화 속도가 빠르고 직렬화된 파일의 크기도 작아서** Apache Avro 파일 포맷과 함께 많이 사용된다.

    ### 4. 저장

    - `더보기`
        - 빅데이터는 대용량의 데이터를 저장하기 때문에 데이터 저장의 안정성과 속도가 중요하다.
        - 스토리지의 종류에는 HDFS외에도 AWS의 S3, MS Azure의 Data Lake, Blob Storage, Google의 Cloud Storage가 있다.

        **1) HDFS (Hadoop Distributed File System)**

        - 하둡 분산 파일 시스템은 하둡 프레임워크를 위해 **Java 언어**로 작성된 **분산 확장 파일 시스템**이다.
        - 범용 컴퓨터를 클러스터로 구성하여 대용량의 **파일을 블록단위로 분할**하여 여러 서버에 복제해 저장한다.

         **2) S3 (Simple Storage Service)**

        - AWS에서 제공하는 인터넷용 저장소

    ### 5. NoSQL

    - `더보기`
        - 관계형 테이블과 다른 방식으로 데이터를 저장하는 데이터베이스
        - 대량의 데이터와 높은 사용자 부하에서도 손쉽게 확장할 수 있다는 장점이 있다.

        ```markdown
        **NoSQL의 종류

        - Key-Value
        	-** key-value가 하나의 묶음으로 저장되는 단순한 구조
        ****	- 고속 읽기, 고속 쓰기, 분산 저장 시 용이하다.
        	- ex) redis, AWS DynamoDB

        **- Document
        	-** XML, JSON과 같은 Document 형식으로 데이터를 저장하는 방식. 
        	- 트리형 구조(B-Tree)로 레코드를 저장하거나 검색할 때 좋은 데이터베이스다.
        	- 단, 데이터가 커질수록 입력, 삭제에 대한 효율이 저하된다.
        	- ex) mongoDB, Firebase

        **- Column**
        	- 쓰기에 특화된 데이터베이스
        	- 채팅내용 저장, 실시간 분석에 주로 사용된다.
        	- ex) cassandra

        **- Graph
        	-** 각 노드 사이의 관계를 알아야 할 때 많이 사용된다 -> 소셜 네트워크
        	- ex) neo4j
        ```

        **1) HBase**

        - HDFS 기반의 **column 기반 NoSQL** 데이터베이스이다.
        - 구글의 빅테이블(BigTable) 논문을 기반으로 개발되었다.
        - 실시간 랜덤 조회 및 업데이트가 가능하며, 각 프로세스는 개인의 데이터를 비동기적으로 업데이트 할 수 있다.

    ### 6. 데이터 처리

    - `더보기`

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/60df377a-9bfe-4273-bf33-4e246dceb4e8/Untitled.png)

        - 데이터 처리는 빅데이터를 분석하는 기술이다.
        - Hadoop의 맵리듀스를 기반으로, Spark, Hive, 임팔라 등 여러가지 기술이 있다.

        **1) MapReduce** 

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bb4f7005-b4e7-446b-b575-72e664aca8f9/Untitled.png)

        - HDFS 상에서 동작하는 가장 기본적인 분석 기술이다.
        - **간단한 단위작업을 반복**할 때 효율적인 맵리듀스 모델을 이용해서 데이터를 분석한다.

        **2) Spark**

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3098a9bd-7288-4b18-90cf-412896bd988b/Untitled.png)

        - Spark는 인메모리 기반의 범용 데이터 처리 플랫폼이다.
        - 배치 처리, 머신러닝, SQL 질의 처리, 스트리밍 데이터 처리, 그래프 라이브러리 처리와 같은 다양한 작업을 수용할 수 있도록 설계되어 있다.
        - 현재 가장 빠르게 성장하고 있는 오픈소스 프로젝트 중의 하나이다.

        **3) Impala**

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/52473500-e875-4429-8f1c-c83edcb30ea5/Untitled.png)

        - 임팔라는 클라우데라에서 개발한 **Hadoop 기반의 분산 쿼리 엔진**이다.
        - 맵리듀스를 사용하지 않고, C++로 개발한 **인메모리 엔진을 사용해 빠른 성능**을 보여준다.
        - 데이터 조회를 위한 인터페이스로 HiveQL을 사용하며 수초내에 SQL 질의 결과를 확인할 수 있다.
        - 임팔라는 Hadoop에 스케일링 가능한 병렬 데이터베이스 기술을 도입함으로써 **데이터 이동이나 전송 과정 없이** 사용자들이 Low Latency의 SQL 쿼리를 HDFS과 아파치 HBase에 저장된 데이터에 발행할 수 있게 한다.

        **4) Presto**

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/41441d34-1121-4590-85ea-78749f64133e/Untitled.png)

        - 프레스토는 페이스북이 개발한 **대화형 질의**를 처리하기 위한 분산 쿼리 엔진이다.
        - 메모리 기반으로 데이터를 처리하며, 다양한 데이터 저장소에 저장된 데이터를 SQL로 처리할 수 있다.
        - 특정 질의의 경우 Hive 대비 10배 정도 빠른 성능을 보여준다.
        - 현재 오픈소스로 개발이 진행되고 있다.

        **5) Hive**

        - 하이브는 Hadoop 기반의 데이터웨어 하우징용 솔루션이다.
        - 페이스북에서 개발했으며, 오픈소스로 공개된 기술이다.
        - SQL과 매우 유사한 **HiveQL이라는 쿼리 언어를 제공**한다. 따라서 Java를 모르는 데이터 분석가도 쉽게 Hadoop 데이터를 분석할 수 있게 한다.
        - **HiveQL은 내부적으로 맵리듀스 job으로 변환되어 실행**된다.

        **6) Hcatalog**

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b988047a-1e1b-42cd-bc80-61bc40e0b64b/Untitled.png)

        - HCatalog는 Pig, MapReduce, Spark에서 Hive 메타스토어 테이블에 액세스할 수 있는 도구이다.
        - 테이블을 생성하거나 작업을 수행할 수 있는 REST 인터페이스와 명령줄 클라이언트를 제공한다.

        **7) Pig** 

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/595d50e8-a609-49ac-a718-06ea3510e7c0/Untitled.png)

        - 야후에서 개발되었으나, 현재는 아파치 프로젝트에 속한 프로젝이다.
        - 복잡할 맵리듀스 프로그래밍을 대체할 Pig Latin이라는 자체 언어를 제공한다.
        - **맵리듀스를 API를 매우 단순화한 형태이고 SQL과 유사한 형태로 설계되었다.**
        - 하지만, SQL과 유사하기만 할 뿐, 기존 SQL 지식을 활용하기가 어려운 편이다.

    ### 7. 클러스터 관리

    - `더보기`
        - 빅데이터는 단일 시스템보다는 보통 클러스터로 처리되기 때문에 자원의 효율적인 사용이 필요하다.
        - 클러스터를 관리하기 위한 기술에는 여러가지가 있다.

        **1) YARN**

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e71126a7-a1f3-4e7e-b56b-bd4b1cea7927/Untitled.png)

        - 얀은 데이터 처리 작업을 실행하기 위한 클러스터 자원(CPU, 메모리, 디스크 등)과 스케쥴링을 위한 프레임워크이다.
        - 기존 Hadoop의 데이터 처리 프레임워크인 **맵리듀스의 단점을 극복**하기 위해서 시작된 프로젝트이며, Hadoop 2.0부터 이용이 가능하다.
        - 맵리듀스, 하이브, 임팔라, 타조, 스파크 등 다양한 애플리케이션들은 **얀에서 리소스를 할당**받아서, 작업을 실행하게 된다.

        **2) Mesos**

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/112e14ad-3c5e-4c68-a413-22feb0b3932d/Untitled.png)

        - 메소스는 클라우드 Infra Structure 및 컴퓨팅 엔진의 다양한 자원(CPU, 메모리, 디스크)을 통합적으로 관리할 수 있도록 만든 자원 관리 프로젝트이다.
        - 현재 아파치 탑레벨 프로젝트로 진행중이며, 페이스북, 에어비엔비, 트위터, 이베이 등 다양한 글로벌 기업들이 메소스로 클러스터 자원을 관리하고 있다.
        - 메소스는 **클러스터링 환경에서 동적으로 자원을 할당**하고 **격리해주는 매커니즘을 제공**하며, 이를 통해 분산 환경에서 작업 실행을 최적화시킬 수있다.

    ### 8. 분산 서버 관리

    - `더보기`
        - 클러스터에서 여러가지 기술이 이용될 때 하나의 서버에서 모든 작업이 진행되면 이 서버가 단일실패지점(SPOF)가 된다.
        - 이로 인한 리스크를 줄이기 위해 분산 서버 관리 기술을 이용한다.

        ```markdown
        📌 단일 실패 지점(SPOF, Single Point of Failure)
        : 시스템 구성 요소 중에서, 동작하지 않으면 전체 시스템이 중단되는 요소를 말한다.
        ```

        **1) Zookeeper**

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a3dbf10f-d48b-486f-806c-7a524eb497c4/Untitled.png)

        - 분산 환경에서 서버 간의 상호 조정이 필요한 다양한 서비스를 제공하는 시스템이다.
        - 크게 다음과 같은 4가지 역할을 수행한다.
            1. 하나의 서버에만 서비스가 집중되지 않게 알맞게 분산해 동시에 처리하게 해준다.
            2. 하나의 서버에서 처리한 결과를 다른 서버와도 동기화해서 데이터의 안전성을 보장한다.
            3. 운영서버에서 문제가 발생해서 서비스를 제공할 수 없을 경우, 다른 대기 중인 서버를 운영서버로 바꾸어 서비스가 중지 없이 제공되게 한다.
            4. 분산 환경을 구성하는 서버의 환경설정을 통합적으로 관리한다.

    ### 9. 시각화

    - `더보기`

        **1) Zeppelin**

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/279e075d-15a0-43c7-8416-f59fd25adb16/Untitled.png)

        - 한국의 NFLab이라는 회사에서 개발하여 아파치 탑 레벨 프로젝트로 최근 승인받은 오픈소스 솔루션.
        - Notebook이라고 하는 웹 기반 Workspace에 Spark, Tajo, Hive, ElasticSearch 등 다양한 솔루션의 API, Query 등을 실행하고 결과를 웹에 나타내는 솔루션이다.

        **2) Hue**

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/97997175-bb7c-45d3-87ac-c4deafcfaeb4/Untitled.png)

        - Hue는 Hadoop User Experience의 약자로, 하둡과 하둡 에코시스템의 지원을 위한 웹 인터페이스를 제공하는 오픈 소스이다.
        - Hive 쿼리를 실행하는 인터페이스를 제공하고, 시각화를 위한 도구를 제공한다.
        - Job의 스케줄링을 위한 인터페이스와 Job, HDFS, 등 Hadoop을 모니터링하기 위한 인터페이스도 제공한다.

    ### 10. 보안

    - `더보기`

        **1) Ranger**

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d132c1dd-9316-4ddb-8657-b73ad427b46e/Untitled.png)

        - 레인저는 하둡 클러스터의 각 모듈에 대한 보안 정책을 관리할 수 있다.
        - HDFS의 ACL, Hive 데이터베이스의 접근권한 등의 보안 정책과 각 모듈에 대한 접근 기록을 보관한다.

    ### 11. 데이터 거버넌스

    - `더보기`
        - 데이터 거버넌스는 기업의 여기저기에 산재한 데이터를 같은 저장소에 관리하고 비정형 데이터를 규칙에 맞게 표준화하는 전사 차원의 빅데이터 관리 체계이다.

        **1) Atlas**

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/70f08cf2-4425-46fa-b4de-139d36a22deb/Untitled.png)

        - 아틀라스는 데이터 거버넌스로 조직이 보안/컴플라이언스 요구사항을 준수할 수 있도록 지원한다.
        - 데이터 자원에 대한 태깅, 다운스트림 데이터셋에 대한 태그 전파, 메타 데이터 접근에 대한 보안등 다양한 기능을 가지고 있다.

        **2) Amundsen**

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/97e3f460-6e98-47fa-81b5-7c1ac5a3b9c4/Untitled.png)

        - 아문센은 데이터 디스커버리 플랫폼이다.
        - 기업에 존재하는 데이터를 검색하고 추천하는 기능을 가지고 있다.

### 1. Flume

- `더보기`
    - 클라우데라에서 대량의 로그 데이터를 여러 소스에서 수집하여 저장하기 위해 만든 데이터 **수집** 프레임워크.

    ### 특징

    - `더보기`
        - 여러 개의 플룸 에이전트를 연결하여 확장 가능
        - 다양한 연결 모드를 지원하여 최종 목적지에 데이터를 전달할 때 까지 유연한 구성이 가능하다.
        - 전달 받은 데이터를 메모리, File, DB에 임시 저장하여 오류 발생시 복구 가능하게 설정할 수 있다 →  신뢰성 있음

    ### Flume의 구조: OG vs NG

    - `더보기`
        - **OG(Old Generation)**

            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fada3a77-f2eb-4b9a-926a-a6126e4a355c/Untitled.png)

            - master가 agent를 관리하는 방식
            - Agent가 모은 로그 데이터를 Collector로 보내고, Collector가 어떤 저장소로 데이터를 전송할지 정해준다. 그리고 이런 데이터 플로우를 컨트롤 해주는 게 Master의 역할이었다.
            - 하지만, Agent와 Controller의 설정값을 변경해줄 때, Master도 함께 변경해줘야 하는 불편함이 있었다.
            - 또한, 데이터가 집중되면 master의 병목현상이 발생한다.
        - **NG(Next Generation)**

            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/086f990c-1486-4898-9e06-4eb15da5b59e/Untitled.png)

            - Master와 Collector의 개념이 사라진 방식.
            - 1.x 버전 부터를 NG라고 부른다.

    ### Event

    - `더보기`
        - Flume에서 전달하는 데이터의 단위
        - Header와 Body로 구성되어 있다.
            - Header : 설정 값, 딕셔너리 형태
            - Body : 전달할 데이터

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/847f9d99-f437-4ae9-b06f-e52e76b29641/Untitled.png)

    ### Agent

    - `더보기`
        - Source, Channel, Sink로 구성된 데이터 수집을 위한 JVM 프로세스
        - 소스로 입력된 메시지를 채널에 저장하고, 저장된 메시지 묶음을 싱크로 전달
        - **소스(Source)**
            - 웹 서버 같은 외부 소스에 의해 전달되는 **이벤트를 수집**하는 역할
            - 외부 소스는 Flume이 인식하는 형태로 이벤트를 전달한다.
            - ex) Avro, Thrift, File Http 소스
        - **채널(Channel)**
            - 소스가 이벤트를 수신하면 채널에 **임시 저장**된다.
            - 채널은 싱크가 이벤트를 다른 목적지로 전달할 때까지 파일이나 메모리 등에 이벤트를 보관
            - ex) 메모리, File, Kafka, DB 채널
        - **싱크(Sink)**
            - 채널에 저장된 이벤트를 외부 저장소, 다른 플룸 에이전트로 **전달**하는 역할
            - 소스와 싱크는 **비동기적**으로 실행된다.
            - ex) HDFS, Hive, Thrift, Avro 싱크

    ### 채널 셀렉터 (Channel Selector)

    - `더보기`
        - 하나의 소스에 다수의 채널이 연결되었을 때 이벤트를 전달하는 기준이다.
        - 종류에는 복제(replicating), 멀티플렉싱(multiplexing)이 있다.
            - 복제(replication) :  모든 채널에 동일한 이벤트를 전달한다. (기본 설정)
            - 멀티플렉싱(multiplexing) : 헤더 정보를 이용해 분기한다.

    ### 싱크 프로세서 (Sink Processors)

    - `더보기`
        - 채널에 연결된 **싱크를 그룹으로 묶어서** 사용한다.
        - 기본 설정은 한 개의 싱크를 사용한다.
        - Failover 모드 시 우선순위가 높은 싱크부터 사용하다가 오류가 발생하면 다음 순위의 싱크를 사용한다.

    ### 인터셉터 (Interceptor)

    - `더보기`
        - 소스로 들어온 이벤트의 헤더를 수정하거나, 내용을 추가할 때 사용한다.

            → 시간 추가(TimeStamp Interceptor)나, 수집서버(Host Interceptor) 정보를 추가할 때 사용

    ### 연결 모드

    - `더보기`
        - multi-agent flow
            - 에이전트의 싱크와 소스를 연결하여, 다수의 에이전트를 Linked-List처럼 연결
        - Consolidation
            - 하나의 에이전트가 여러 에이전트에서 수집한 데이터를 받아서 처리
        - Multiplexing the flow
            - 하나의 에이전트에서 여러 개의 채널로 이벤트를 전달

### 2. Kafka

- `더보기`
    - kafka는 Linked-In에서 개발한 분산 스트리밍 플랫폼이다.
    - 메시징, 메트릭 수집, 로그 수집, 스트림 처리 등 다양한 용도로 사용할 수 있다.

    ```markdown
    📌 **Merix이란?**
    : 특정 시간에 시스템의 일부 측면을 설명하는 수치 값이다.
    ```

    ### 특징

    - `더보기`
        - 빠르다

            → 수천개의 데이터 소스로부터 초당 수백 메가바이트의 데이터를 입력 받아도 안정적으로 처리 가능.

        - 확장가능하다.

            → 메시지를 파티션으로 분리하여, 분산 저장, 처리할 수 있어 클러스터로 구성하여 확장 가능.

        - 안정적이다.

            → 클러스터에 파티션 복제하여 장애 내구성을 가짐

    ### 발행-구독(Pub-Sub) 모델

    - `더보기`
        - kafka는 Pub-Sub 모델을 기반으로 동작한다.
        - 이는 발행자(Producer)가 메시지를 특정 수신자에게 직접 보내는 방식이 아니라 주제(Topic)에 맞게 브로커에게 전달하면 구독자(Consumer)가 브로커에게 요청해서 가져가는 방식이다.

    ### 카프카의 주요 구성

    - `더보기`
        - Topic, Partition
        - Producer, Consumer
        - Broker, Zookeeper
        - Consumer Group
        - Replication

        ### Topic-Partition

        - Topic

            ```markdown
            - 메시지는 Topic으로 분류된다.
            - Topic은 발행자가 스트림을 발행하는 단위를 의미한다.
            - 스트림의 발행과 구독은 **Topic 단위**로 처리된다.
            ```

        - Partition

            ```markdown
            - 하나의 topic은 여러 개의 파티션으로 저장될 수 있다.
            - 여러 개의 파티션으로 나누면 메시지 분산처리로 처리량을 높일 수있고, 분산 저장을 통해 
            	오류가 발생할 때 데이터를 복구할 수 있다.
            - 파티션의 데이터 저장은 기본적으로 RR(Round-Robin) 방식으로 동작한다.
            	ex) P0 -> P1 -> P2
            ```

        ### Producer, Consumer

        - 발행자 (Producer)

            ```markdown
            - 메세지를 생산하는 주체
            - 메시지를 만들어고 브로커(Broker)에게 Topic으로 분류된 메시지를 전달 -> **배치 형태로!
            - 발행자는 구독자의 존재를 알지 못함**
            ```

        - 구독자 (Consumer)

            ```markdown
            - 소비자로 메시지를 소비하는 주체
            - 원하는 topic의 각 파티션에 존재하는 오프셋의 위치를 기억하고 관리하여 데이터의 중복관리
            **- 구독자는 발행자의 존재를 알지 못함**
            ```

        - 구독자 그룹 (Consumer Group)

            ```markdown
            - 구독자들의 묶음으로 **하나의 파티션에 하나의 구독자 그룹**이 존재한다.
            - 파티션을 늘릴 때는 구독자의 수도 고려해야 한다.
            	- 기본 설정은 파티션과 구독자의 수를 동일하게 설정한다.
            	- 메시지가 파티션에 쌓이는 속도와 구독자가 처리하는 속도를 비교하여 적절한 개수 설정!
            - 구독자 그룹에서 하나의 구독자에 장애가 발생하면, 다른 구독자가 장애가 발생한 구독자의
            	파티션 데이터도 함께 처리하여 장애를 극복한다.
            ```

        ### 브로커 (Kafka Cluster)

        - 클러스터로 구성된 **메시지 큐**라고 할 수 있다.
        - 메시지는 클러스터에 파티션 단위로 나누어 관리/복제된다.
        - 구독자가 메시지를 가져가도 바로 삭제하지 않음 (기본설정은 7일동안 저장)

### 3. Sqoop

- `더보기`
    - Sqoop은 RDB(관계형 데이터베이스)와 Hadoop HDFS간에 데이터를 **전송**할 수 있도록 설계된 오픈소스 소프트웨어이다.
    - Sqoop 1, 2  두 가지 버전이 존재한다.
        - Sqoop 1은 **클라이언트 방식**으로 CLI 명령어로 작업을 실행한다.
        - Sqoop 2는 **클라이언트 방식에 서버사이드 방식이 추가** 되었다. Sqoop 서버가 존재하고, 사용자가 서버에 요청해서 작업을 실행하는 방식이다.
    - `import`(DB ⇒ HDFS), `export`(HDFS ⇒ DB) 모두 가능하다.

## # 4. 빅데이터 시스템


### 1. 빅데이터 시스템: 멜론

- `더보기`

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9bf3dfa3-0b46-4fd0-9eee-7a858199d01d/Untitled.png)

    - **수집**
        - 데이터베이스 데이터 → Sqoop
        - 로그 데이터
            - Flume을 이용하여 1시간마다 쉘 스크립트(scp)로 수집
            - Hudson(허드슨)을 이용하여 배치 데이터를 수집

                 **→ 허드슨은 더이상 유지 보수 되고 있지 않으며, 젠킨스가 이를 대체한다**

    - **분석**
        - 실시간 분석과 배치 분석을 제공
        - Hive를 이용하여 분석 결과 제공
        - SQL 기반의 분석 플랫폼을 유지하기 위해 노력함.
    - **서비스**
        - 온라인 서비스와 통계 시스템 제공
        - MySQL과 HBase, ElasticSearch를 이용하여 제공

        ```markdown
        **📌 ElasticSearch이란?**
        : Java 오픈소스 분산 검색 엔진.
        	실시간 분석, 저장, 고속 검색이 가능하다. (Document 기반의 데이터 저장)
        ```

### 2. 빅데이터 시스템: 네이버 - DataProc

- `더보기`

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fa1a1de1-7b02-4c13-ac51-7842b26c72c0/Untitled.png)

    - **데이터 로그(DataLog)**
        - ElasticSearch 기반의 로그 통합 관리 플랫폼
        - 검색 서비스의 모든 로그를 한곳에 모아 효율적인 분석을 위한 환경을 제공
        - 초당 22만건 실시간 색인이 가능하다.
    - **데이터 스토어(DataStore)**
        - HBase 기반의 데이터 저장소
        - **데이터 카탈로그**를 통해 보관된 데이터의 목록, 상세정보, 생산자와 소비자를 한눈에 알 수 있도록 제공한다.
        - 저장된 데이터의 효율적인 활용을 위해 SQL 기반의 처리 시스템을 구축
        - 비슷한 형태의 요청이 많으므로 SQL 템플릿을 제공하여 처리할 수 있도록, [Hue](https://www.notion.so/Part-1-9cf76dbf00d94f8fab85963d373500ba)를 이용하여 SQL 템플릿을 지원한다.
    - **데이터프록(DataProc)**
        - 보관된 데이터를 개발자가 마음껏 분석할 수 있는 환경을 제공한다.
        - 개발자가 자유롭게 컴퓨팅 자원을 이용할 수 있는 환경을 제공한다.

### 3. 빅데이터 시스템: 카카오 광고 시스템

- `더보기`
    - 카카오는 데이터를 다각도로 분석하기 위해서 키린(Kylin)을 이용한다.

    ```markdown
    📌 **키린(Kyline)**
    - eBay에서 개발된 Hadoop 에코 시스템 기반의 OLAP 시스템.
    	(OLAP : 다차원적 정보를 관련자들이 공유해 빠르게 분석하는 과정)

    - 원본 데이터는 Hive에 스키마 형태로 저장되며, 큐브 형태로 가공된 데이터는 HBase에 저장된다.
    - 데이터를 큐브 형태로 가공하여 분석하고 있기 때문에 사용자의 쿼리에 빠른 속도로 결과를 제공
    ```

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e4aa6b69-6d12-4c0c-bcee-e0f1def8c7dd/Untitled.png)

    - **수집**
        - Kafka, LogStash
        - Spark Streaming, Flink → 스트리밍 데이터 수집/처리
    - **처리**
        - Hive, Impala
    - **저장**
        - HDFS, 카산드라, HBase, ElasticSearch, Redis

### 4. 빅데이터 시스템: 쿠팡

- `더보기`
    - 쿠팡은 RDB를 이용한 초기부터 2019년까지 4단계를 거쳐서 빅데이터 처리 시스템을 완성하였다.
    - **Phase 1**
        - 관계형 데이터베이스를 이용한 처리
    - **Phase 2**
        - on Premise Hadoop, Hive, MPP 시스템
        - 데이터 생산과 수요의 증가
        - 고객의 행동을 기반으로 하는 데이터 분석
        - 데이터의 증가로 인한 병목 현상 발생하였으며 onPremise 환경에서는 Scale Out이 쉽지 않았다.
    - **Phase 3**
        - 클라우드 인프라로 이동
        - 고객의 행동 기반 로그도 수집
        - 데이터 수집은 그대로 시행하나 저장은 클라우드 저장 공간으로 변경되었다.
        - AirFlow Scheduler를 도입
        - 사용자를 위한 휴, 태블루, 제플린, CLI, 내부 도구 등을 제공
    - **Phase 4**

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d239350e-f314-4633-a20c-fffa6d66e61b/Untitled.png)

### 5. 빅데이터 시스템: LINE 광고 데이터 파이프라인 - BigDB

- `더보기`

    ### BigDB란?

    - `더보기`
        - LINE 광고의 빅데이터 처리 파이프라인
        - 데이터 수집, 가공, 재가공, 조회 등의 기능을 제공한다.
        - 실시간 분석

            → LINE의 광고의 분석 형태는 사용자가 광고에 노출이 되었을 경우 해당 이벤트를 받아서 즉시 처리한다.

        - 배치 분석

            → 이벤트를 받아서 1시간이나 1일, 등록한 시간마다 처리한다.

        - 유연한 데이터 제공 기능

            → BigDB는 분석을 위해 데이터를 제공

            → 시계열 데이터와 정적인 데이터를 Join하여 제공

            ```markdown
            **📌 시계열 데이터?**
            : 일정 시간동안 수집된 일련의 순차적으로 정해진 데이터 셋의 집합.
            	시간에 관해 순서가 매겨져 있다는 점이 특징이다.
            ```

    ### 특징

    - `더보기`
        - REST CLI 기능을 제공
        - 사용자 요청의 데이터 형식을 지정하여 데이터를 조회하는 기능
        - 저장소를 크게 2개로 구분 → `SSD 기반 HDFS`,  `SATA 기반 HDFS`

    ### 솔루션 구조

    - `더보기`
        - 다양한 오픈소스를 활용함.

            → 단, 다양한 오픈소스를 활용했을 때, 통합된 툴은 사용하지 않는 별도의 솔루션을 포함하고, 이 솔루션이 노드의 자원을 사용하는 문제가 있음

            → 또한, 다양한 오픈소스를 활용하게 되면 의존성 문제가 발생할 수 있음

            ⇒ 최대한 변경은 없게하고, 솔루션 간의 흐름에 대한 컨트롤 역할을 BigDB Core와 API로 개발

            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/09d51de6-bff6-44e0-a94b-b865f4a364c9/Untitled.png)

            - Message Proxy: akka.http를 사용하여 JSON 데이터를 수집하고, Kafka의 정해진 Topic에 produce 한다.
            - Kafka: 수집된 JSON 데이터를 7일간 보관하며, Partition은 스트리밍에서 사용하는 코어의 개수에 따라 변경된다.
            - Streaming: Spark을 사용하며, 5초 주기로 Task를 수행하고 간단한 데이터의 변환 작업 후 스키마에서 지정한 Table에 Data Frame을 Insert Into로 추가하게 됩니다. Streaming이 두 개로 나누어져 있고, **왼쪽은 집계 데이터를 위한 Table**, **오른쪽은 원본 데이터와 원본과의 Join 데이터를 위한 Table**을 다루게 됩니다.
            - HDFS: 수집된 JSON 데이터를 압축한 후 Parquet 형식으로 저장하게 됩니다. **왼쪽은 SSD 디스크를, 오른쪽은 SATA 디스크를 사용한다**. Federation 설정으로 각각의 HDFS는 namespace만으로 편리하게 접근할 수 있습니다.
            - Spark: Zeppelin, Streaming, BigDB 각각 별도의 세션으로 동작을 하며, Hive Meta-Store를 사용하여 Table은 세션 간에 공유된다. Locality를 위해서 Table(실제 데이터 파일)이 존재하는 파트의 자원을 사용하게 된다.
            - End Point: Web/Zeppelin/BigDB API 등에서 Spark의 자원과 Table을 사용하게 된다. 오**른쪽에서는 주로 집계와 조회**를 하고, **왼쪽에서는 조회와 스케줄링된 작업을 수행하는 용도**로 사용된다.
            - BigDB Core/API: akka.http를 사용하고 있다. 스키마의 생성과 관리, 쿼리 기반의 집계나 스케줄링된 작업의 수행을 관리한다. **terminus.js를 사용하여 REST CLI를 제공**한다. End Point에 결과를 전달하기 위해서 JSON 데이터를 CSV/TSV/JSON 형태로 변환한다.

    ### 시스템 구조

    - `더보기`
        - 디스크의 종류에 따라 두 개의 클러스터로 구분하여 사용
            1. SSD
                - 읽기/쓰기가 빠르고 용량이 적은 경우에 사용
                - 일반적으로 집계 후의 작은 사이즈를 장기간 보관하는 용도로 사용
            2. SATA
                - 읽기/쓰기가 SSD에 비해서 느리지만 용량이 큰 노드일 때 사용
                - 원본데이터와 원본데이터 수준으로 가공되는 큰 사이즈를 장기간 보관하는 용도로 사용
                - SATA를 사용하는 데이터 노드의 경우는 물리적으로 12개의 디스크를 분산해서 사용함으로써 효율성을 확보
        - CPU의 경우는 동일하게 구성
        - Memory의 경우는 디스크의 공간을 고려한 Rack 구성으로 차이 나게 구성
        - 두 개의 클러스터가 모두 수평 확장을 고려
        - 메모리
            - Memory의 경우는 Parquet를 사용하게 되면서 사용성이 적어지고 있고, 남는 Memory를 Elasticsearch에 일부 할당함으로써 검색이 필요한 데이터 처리에 활용
        - 네트워크
            - 조회 시 대상이 되는 데이터의 사이즈가 크고, 그에 따라서 셔플의 성능을 확보하기 위해 네트워크는 10GB를 사용하고 있습니다. 사용되는 솔루션 모두 가능한 로컬통신을 할 수 있도록 설정

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/18b59d2d-d8a3-48a6-a09b-9fd0884da940/Untitled.png)
