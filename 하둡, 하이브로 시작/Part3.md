## 🐘 하둡, 하이브로 시작 Part3️⃣


---

📌 **[ INDEX ]** 

1. 하이브란?
2. 데이터베이스
3. 테이블
4. 함수
5. 관리
6. 트랜잭션
7. 성능 최적화

---

 

## # 1. 하이브이란?

---

### 1. 하이브란?

- `더보기`
    - 하이브는 하둡 에코 시스템 중에서 데이터를 모델링하고 프로세싱하는 경우 가장 많이 사용하는 **데이터 웨어하우징용 솔루션**이다.
    - **RDB의 데이터베이스, 테이블과 같은 형태**로 HDFS에 저장된 데이터의 구조를 정의하는 방법을 제공한다.
    - 이 데이터를 대상으로 SQL과 유사한 **HiveQL 쿼리**를 이용해 데이터를 조회하는 방법을 제공한다.

    ### Hive 구성요소

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/566891cf-b0be-4f8f-b727-8dae5f921613/Untitled.png)

    - UI
        - 사용자가 쿼리 및 기타 작업을 시스템에 제출하는 사용자 인터페이스
        - CLI, Beeline, JDBC 등
    - Driver
        - 쿼리를 입력받고 작업을 처리
        - 사용자 세션을 구현하고, JDBC/ODBC 인터페이스 API 제공
    - Compiler
        - 메타 스토어를 참고하여 쿼리 구문을 분석하고 실행 계획을 생성
    - MetaStore
        - DB, 테이블, Partition의 정보를 저장
    - Execution Engine
        - 컴파일러에 의해 생성된 실행 계획을 실행

    ### 1) 하이브 버전별 특징

    - `더보기`
        - 하이브는 **SQL을 하둡에서 사용하기 위해** 시작되었다.

        ### Hive 1.0

        - 2015년 2월 → 1.0 버전 발표
        - SQL을 이용한 맵리듀스 처리
        - 파일 데이터의 논리적 표현
        - 빅데이터의 배치 처리를 목표

        ### Hive 2.0

        - 2016년 2월 → 2.0 버전 발표
        - LLAP(Live Long and Process) 구조 추가
        - 기본 실행 엔진이 TEZ로 변경
        - Spark 지원 강화
        - CBO 강화
        - HPLSQL 추가

        ### Hive 3.0

        - 2018년 5월 → 3.0 버전 발표
        - Hive CLI를 제거 & TEZ 엔진과 비라인을 이용해 작업을 처리하도록 수정
        - 롤을 이용한 작업 상태 관리(workload management)
        - 트랜잭션 처리 강화
        - 구체화 뷰(Materialized View) 추가
        - 쿼리 결과를 캐슁하여 더 빠른속도로 작업 가능
        - 테이블 정보 관리 데이터베이스 추가

    ### 2) 하이브 서비스

    - `더보기`

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b3b9cf78-a4b2-4684-986e-5b5d284eeed5/Untitled.png)

        ### 메타스토어

        - 메타스토어 서비스는 HDFS 데이터 구조를 저장하는 실제 데이터베이스를 갖는다.
        - 메타스토어에는 3가지 실행모드가 존재한다.

        ```java
        1. 임베이디드(Embeded)
        	- 별도의 데이터 베이스를 구성하지 않고 더비 DB를 이용한 모드
        	- 한번에 하나의 유저만 접근 가능

        2. 로컬(Local)
        	- 별도의 데이터베이스를 가지고 있지만, 하이브 드라이버와 같은 JVM에서 동작

        3. 리모트(Remode)
        	- 별도의 데이터베이스를 가지고, 별도의 JVM에서 단독으로 동작하는 모드
        	- 리모트로 동작하는 하이브 메타스토어를 HCat 서버1라고도 함
        ```

        - **테스트**로 동작할 때는 **임베디드 모드**를 사용하고, **실제 운영**에서는 **리모트 모드**를 많이 사용한다.

        ### 하이브서버2(hiveserver2)

        - 하이브서버2는 다른 언어로 개발된 클라이언트와 연동 서비스를 제공한다.
        - 또한, 기존 하이브서버1을 개선하여 인증과 다중 사용자 동시성을 지원한다.
        - 쓰리프트, JDBC, ODBC 연결을 사용하는 애플리케이션과 통신하여 하이브 연산을 수행하고 결과를 반환한다.

        ### 비라인(beeline)

        - 하이브서버2 프로세스에 접근할 수 있는 하이브의 **명령행 인터페이스**이다.
        - CLI는 로컬 하이브 서비스에만 접근할 수 있지만 비라인은 **원격 하이브 서비스에 접속**할 수 있다.

        ### HCatalog

        - HCatalog는 Pig, MapReduce, Spark에서 하이브의 데이터 파일에 접근할 수 있도록 도와주는 추상 계층을 제공한다.

        ### WebHCat

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c054cc4a-f37a-48d4-9235-14994a5839fe/Untitled.png)

        - HCatalog의 기능을 **REST API**로 제공한다.
        - 기본적으로 50111 포트를 이용

    ### 3) 하이브 CLI

    - `더보기`
        - 하이브 CLI(Command Line Interface)는 **하이브 쿼리를 실행하는 가장 기본적인 도구**다.
        - 인터랙티브 쉘을 이용하여 사용자의 명령을 입력할 수 있다.

        ### CLI 옵션

        - `hiveconf`는 옵션값을 설정할 때 이용한다.
        - `hivevar`는 쿼리에 변수를 지정할 때 이용한다.

        ### 쿼리 실행

        - 하이브 쿼리 실행은 인터랙티브 쉘에서 입력하는 방법, 커맨드 라인 입력, 파일 입력방법 3가지가 있다.

        **1) 인터랙티브 쉘 입력**

        - 하이브 CLI를 실행하고, 쉘을 이용하여 입력하는 방법

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/878ab2a4-4ef7-4219-98c2-3b296ae35531/Untitled.png)

        **2) 커맨드 라인 입력**

        - 커맨드 라인 입력은 `-e` 옵션을 이용한다.

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e73312f0-6f75-4550-b801-d457997209ff/Untitled.png)

        **3) 파일 입력**

        - 파일입력은 쿼리를 파일로 저장해놓고 해당 파일을 지정하는 방식이다.
        - `-f`옵션을 이용한다.

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fb884fdc-1227-4fe5-8e38-ef86c80c7bfd/Untitled.png)

        ### CLI 내부 명령어

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8c3453a9-33cd-4df9-bde2-9eba719bb0b8/Untitled.png)

    ### 4) 비라인(beeline)

    - `더보기`

    ### 5) 메타스토어

    - `더보기`
