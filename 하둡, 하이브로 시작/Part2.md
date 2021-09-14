## 🐘 하둡, 하이브로 시작 Part2️⃣

---

📌 **[ INDEX ]** 

1. 하둡이란?
2. HDFS
3. 맵리듀스
4. YARN
5. 작원 지원 도구
6. 오브젝트 저장소
7. 설정

---

 

## # 1. 하둡이란?

---

### 1. 하둡 버전별 특징

- `더보기`

    **1) 하둡 v1**

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/32775f9b-2116-45fb-84ad-0e66a9556072/Untitled.png)

    - 하둡의 기본 아키텍쳐를 정립함
    - 병렬처리(MapReduce)는 Job Tracker와 Task Tracker가 담당하고, 분산저장(HDFS)는 NameNode와 DataNode가 담당하도록 구조를 설정하였다.

        ➡ 하지만, 병렬처리의 클러스터의 자원관리와 애플리케이션의 라이프사이클 관리를  Job Tracker가 모두 담당하여 **병목현상이 발생**하였다.

    - 클러스터당 최대 4000개의 노드를 등록

    **2) 하둡 v2**

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/42f7f6b0-b3c6-47f1-98c4-6a71c2f4437b/Untitled.png)

    - Job Tracker의 병목현상을 개선하기 위해 **YARN 아키텍쳐를 도입**하였다.
    - YARN 아키텍쳐는 Job Tracker의 기능을 분리하여 클러스터의 자원관리는 리소스 매니저와 노드매니저에게 담당하도록 하였다.
    - 또한, 애플리케이션의 라이프 사이클 관리는 애플리케이션 마스터와 컨테이너에게 담당하도록 하였다.
    - YARN 아키텍처를 도입하면서 작업 처리를 컨테이너(Container)단위로 처리
    - 클러스터당 1만개 이상의 노드 등록 가능

    **3) 하둡 v3**

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/36cb28e5-a7be-46ac-b835-b3c61e3412c4/Untitled.png)

    - 이레이져 코딩의 도입

        ➡ HDFS의 데이터 저장 효율성을 증가시켰다.

    - YARN 타임라인 서비스를 개선
    - Java 8 지원
    - Ozone 추가 → 오브젝트 저장소 추가
    - 하둡 v1부터 사용된 쉘 스크립트를 재작성하여 안정성을 높였다.
    - MapReduce 처리에 네이티브 프로그램을 도입하여 성능을 개선하였다.
    - 고가용성을 위해 2개 이상의 NameNode를 지원하도록 개선되었다.

## # 2. HDFS

---

### 0. HDFS

- `더보기`
    - HDFS(Hadoop Distributed File System)는 실시간 처리보다 **배치처리를 위해 설계**되었다.
    - 따라서 빠른 데이터 응답시간이 필요한 작업에는 적합하지 않다!
    - 또한 네임노드가 단일 실패 지점(SPOF)이 되기 때문에 네임노드 관리가 중요하다.

    ### 특징

    ### 블록 단위 저장

    - HDFS는 데이터를 블록 단위로 나누어서 저장한다.
    - 블록사이즈보다 큰 크기의 데이터 파일은 블록단위로 나누어 저장하기 때문에 단일 디스크의 데이터보다 큰 파일도 저장할 수 있다.

    ### 블록 복제를 이용한 장애 복구

    - HDFS는 장애 복구를 위해서 각 블록을 복제하여 저장한다.
    - 하나의 블록은 **3개의 블록**으로 복제되고, 같은 랙(Rack) 서버와 다른 랙(Rack) 서버로 복제되어 저장된다.

        ➡ 예를 들어, 1G 데이터를 저장할 때, 3개의 데이터가 복제되어야 하므로 3G의 저장공간 필요!

        ➡ 너무 많은 저장공간이 필요하다는 문제점이 발생🤔

    - 블록에 문제가 생기면 복제한 다른 블록을 이용해서 데이터를 복구한다.

    ### 읽기 중심

    - HDFS는 데이터를 한 번 쓰면 여러 번 읽는 것을 목적으로 한다.
    - **따라서 파일의 수정을 지원하지 않는다!**
    - 이를 통해 동작을 단순화하고 읽기 속도를 높일 수 있다.

    ### 데이터 지역성

    - 맵리듀스는 HDFS의 데이터 지역성을 이용해서 처리 속도를 증가시킨다.
    - 데이터가 있는 곳에서 알고리즘을 처리하기 때문에 네트워크를 통해 대용량 데이터를 이동시키는 비용을 줄일 수 있다.

### 1. 구조(Architecture)

- `더보기`

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b944a476-769f-496e-a305-d56a168f7432/Untitled.png)

    - HDFS는 마스터 슬레이브 구조로 **하나의 네임노드와 여러 개의 데이터노드**로 구성된다.
    - 네임노드는 메타데이터를 가지고 있고, 데이터는 **블록 단위로 나누어 데이터노드에 저장**된다.
    - 사용자는 네임노드를 이용해 데이터를 쓰고, 읽을 수 있다. (수정은 불가능!)

    ```markdown
    1. 네임노드
    	- 메타데이터 관리
    		- 메타데이터 파일 종류
    		- 메타데이터 파일 저장 형태
    	- 데이터 노드 관리
    2. 데이타노드
    	- 블록 파일 저장형태
    	- 데이타노드 상태
    		- 활성상태
    		- 운영 상태
    3. 네임노드 구동 과정
    4. 파일 읽기/쓰기
    	- 파일 읽기
    	- 파일 쓰기
    5. 참고
    ```

    ### 네임 노드

    - 네임 노드의 주요 역할을 **메타데이터 관리**와 **데이터 노드의 관리**이다 (= Master)

    ### 메타데이터 관리

    ```markdown
    **📌 메타메이터(meta-data)란?
    :** 데이터에 대한 데이터.
    	파일이름, 파일크기, 파일생성시간, 파일접근권한, 파일 소유자 및 그룹 소유자, 
    	파일이 위치한 블록의 정보 등으로 구성된다
    ```

    - 네임노드에서는 각 데이터노드에서 전달하는 메타데이터를 받아서 전체 노드의 메타데이터 정보와 파일 정보를 묶어서 관리한다.
    - 메타데이터는 사용자가 설정한 위치(dfs.name.dir)에 보관된다.
    - 네임노드가 실행될 때 파일을 읽어서 메모리에 보관한다.
    - 운영중에 발생한 수정사항은 네임노드의 메모리에는 바로 적용되고, 데이터의 수정사항을 다음 구동시 적용을 위해서 주기적으로 [Edist 파일](https://www.notion.so/Part-2-99f9730fc5b54eddb340711449f12b94)로 저장한다.

    ### 메타데이터 파일 종류

    - Fsimage 파일
        - 네임스페이스와 블록 정보
    - Edits 파일
        - 파일의 생성, 삭제에 대한 트랜잭션 로그
        - 메모리에 저장하다가 주기적으로 생성

    ### 메타데이터 파일 저장 형태

    - 사용자가 설정한 위치(dfs.name.dir)에 다음과 같은 파일의 형태로 저장된다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/915b00f9-dc93-4245-8095-d33dce2da70b/Untitled.png)

    ### 데이터 노드 관리

    - 네임노드는 데이터노드가 주기적으로 전달하는 하트비트와 블록리포트를 이용하여 데이터노드의 동작상태, 블록 상태를 관리한다.

    ```markdown
    📌 **하트비트(heart-beat)
    	-** 하트비트를 이용하여 데이터가 동작중이라는 것을 네임노드가 알 수 있다.
    	- 하트비트가 도착하지 않으면 네임노드는 데이터노드가 동작하지 않는 것으로 간주하고, 
    		더 이상 IO가 발생하지 않도록 조치한다.
    	- 3초, dfs.heartbeat.interval

    📌 **블록리포트(block-report)**
    	- 블록리포트를 이용하여 HDFS에 저장된 파일에 대한 최신 정보를 유지한다.
    	- 블록의 변경사항을 체크하고, 네임노드의 메타데이터를 갱신한다.
    	- 데이터노드에 저장된 블록목록, 각 볼록이 로컬 디스크의 어디에 저장되어 있는지 알 수 있다
    	- 6초, dfs.blockreport.intervalMsec
    ```

    ### 데이터 노드

    - 데이타노드는 파일을 저장하는 역할을 한다. → 블록단위
    - 데이타노드는 주기적으로 네임노드에 하트비트와 블록리포트를 전달한다.

    ### 데이터 노드 상태

    - 데이터 노드의 상태를 나타내는 정보는 두 가지로 구분할 수 있다.

        ➡️ `활성 상태`, `운영 상태`

    1) **활성상태**

    - 활성 상태는 데이터노드가 Live 상태인지 Dead 상태인지를 나타낸다.
    - **Live 상태** → 데이터노드가 하트비트를 주기적으로 전달하여 살아 있는지 확인되는 상태
    - **Statle 상태** → 이터노드에 문제가 발생하여 지정한 시간동안 하트비트를 받지 못한 상태
    - **Dead 상태** → 이후 지정한 시간동안 응답이 없는 상태

    **2) 운영 상태**

    - 운영 상태는 데이터노드의 업그레이드, 패치 같은 작업을 하기 위해 **서비스를 잠시 멈추어야 할 경우** 블록을 안전하게 보관하기 위해 설정한다.
    - 다음의 상태가 있다.

        ```markdown
        1) NORMAL: 서비스 상태
        2) DECOMMISSIONED: 서비스 중단 상태
        3) DECOMMISSION_INPROGRESS: 서비스 중단 상태로 진행 중
        4) IN_MAINTENANCE: 정비 상태
        5) ENTERING_MAINTENANCE: 정비 상태로 진행 중
        ```

    - [dfs.host](http://dfs.host)s 파일에 노드의 상태를 JSON 형태로 기술하여 운영중에 서비스를 잠시 멈추고 정비를 한 후 다시 진행할 수 있다!

    ### 네임노드 구동 과정

    - 네임노드는 다음의 순서로 구동된다.

    ```markdown
    1. **Fsimage**를 읽어 메모리에 적재합니다.
    2. **Edits 파일**을 읽어와서 변경내역을 반영
    3. 현재의 메모리 상태를 스냅샷으로 생성하여 Fsimage 파일 생성
    4. 데이터 노드로부터 블록리포트를 수신하여 매핑정보 생성
    5. 서비스 시작
    ```

    ➡️ Fsimage와 Edits를 읽어서 작업을 처리하기 때문에 두 파일의 크기가 크면 구동 시작시간이 오래 걸릴 수 있다!

    ### 파일 읽기/쓰기

    - HDFS 명령행 인터페이스를 이용하여 HDFS 파일에 쉽게 접근할 수 있다.
    - 또한, Java, C API를 제공하여 이를 구현해서 접근하면 된다.

    ### 파일 읽기

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/731f1c4a-67e0-4606-b2b2-9ecbb5a9260a/Untitled.png)

    1. 네임노드에 파일이 보관된 블록 위치를 요청한다람쥐.
    2. 네임노드가 해당 파일의 블록 위치를 반환한다람쥐.
    3. 각 데이터 노드에 파일 블록을 요청한다람쥐.

        ➡️ 만약 노드의 블록이 깨져있으면 네임노드에 이를 통지하고 다른 블록을 확인한다!

    ### 파일 쓰기

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/98be49c5-ee01-4d5f-b940-899d199bde06/Untitled.png)

    1. 네임노드에 파일 정보를 전송하고, 파일의 블록을 써야할 노드 목록을 요청한다.
    2. 네임노드가 파일을 저장할 목록 반환한 후,
    3. 데이터 노드에 파일 쓰기를 요청한다.

        ➡️ 이 때, 데이터 노드간 복제도 함께 진행된다.

    ### 1.1 블록

    - HDFS 파일은 지정한 크기의 블록으로 나누어지고, **각 블록은 독립적으로 저장**된다.
    - HDFS의 블록은 128MB와 같이 매우 큰 단위이다.

        ➡️ 블록이 크면 하드디스크에서 블록의 시작점을 탐색하는 데 걸리는 시간을 줄일 수 있다.

        ➡️ 이를 통해, 네트워크를 통해 데이터를 전송하는데 더 많은 시간을 할당할 수 있다.

    - 따라서 여러 개의 블록으로 구성된 대용량 파일을 전송하는 시간은 디스크 IO 속도에 크게 영향을 받게 된다.

    ### 블록 크기 파일 분할

    - 기본 블록 크기를 넘어서는 파일은 블록 크기로 분할하여 여러 개의 블록에 저장된다.

    ### 블록 추상화의 이점

    1. 파일 하나의 크기가 단일 디스크의 용량보다 더 커질 수 있다.

        ➡️ 크기가 큰 파일을 블록단위로 나누어서 단일 디스크의 용량보다 큰 파일을 저장할 수 있다.

    2. 스토리지의 서브 시스템을 단순하게 만들 수 있다.

        ➡️ 파일 탐색 지점이나 메타정보를 저장할 때 사이즈가 고정되어 있으므로 구현이 좀 더 쉽다.

    3. 내고장성을 제공하는데 필요한 복제(replication)을 구현할 때 매우 적합하다.

        ➡️ 블록 단위로 복제를 구현하기 때문에 데이터 복제도 어렵지 않게 처리할 수 있다.

    ### 블록 지역성

    ```markdown
    **📌 블록 지역성(Block Locality)이란?**
    : 같은 파일에서 분리된 블록끼리 가까운 위치에 존재하는 것을 의미한다.
    	HDFS에서는 맵리듀스를 처리할 때 현재 노드에 저장되어 있는 블록을 이용하는 것을 의미한다.
    ```

    - 맵리듀스를 이용한 분산 컴퓨팅은 블록의 지역성을 이용해 성능을 높인다.
        - 네트워크를 이용한 데이터 전송 시간 감소
        - 대용량 데이터 확인을 위한 디스크 탐색 시간 감소
        - 적절한 단위의 블록 크기를 이용한 CPU 처리시간 증가
    - 클라우드 저장공간을 이용하는 경우, HDFS보다 속도가 느려진다.

        ➡️ 하지만, 영구적인 데이터 보관 및 HDFS 관리비 절감에 따른 장점이 있다!

    ### 블록 작업 순서

    - 블록 지역성을 위한 작업 우선 순위는 다음과 같다.
        1. 같은 노드에 있는 데이터
        2. 같은 랙(Rack)의 노드에 있는 데이터
        3. 다른 랙의 노드에 있는 데이터

    ### 블록 스캐너

    - 데이터 노드는 주기적으로 블록 스캐너를 실행하여 블록의 체크섬을 확인하고 오류가 발생하면 수정한다.

    ### 블록 캐싱

    - 데이터 노드에 저장된 데이터 중 자주 읽는 블록은 **블록 캐시(block cache)라는 데이터 노드의 메모리**에 명시적으로 캐싱할 수 있다.
    - 파일 단위로 캐싱 할 수도 있어서 조인에 사용되는 데이터들을 등록하여 읽기 성능을 높일 수 있다.

    ```bash
    $ hdfs cacheadmin
    Usage: bin/hdfs cacheadmin [COMMAND]
              [-addDirective -path <path> -pool <pool-name> [-force] [-replication <replication>] [-ttl <time-to-live>]]
              [-modifyDirective -id <id> [-path <path>] [-force] [-replication <replication>] [-pool <pool-name>] [-ttl <time-to-live>]]
              [-listDirectives [-stats] [-path <path>] [-pool <pool>] [-id <id>]
              [-removeDirective <id>]
              [-removeDirectives -path <path>]
              [-addPool <name> [-owner <owner>] [-group <group>] [-mode <mode>] [-limit <limit>] [-maxTtl <maxTtl>]
              [-modifyPool <name> [-owner <owner>] [-group <group>] [-mode <mode>] [-limit <limit>] [-maxTtl <maxTtl>]]
              [-removePool <name>]
              [-listPools [-stats] [<name>]]
              [-help <command-name>]

    # pool 등록 
    $ hdfs cacheadmin -addPool pool1
    Successfully added cache pool pool1.

    # path 등록
    $ hdfs cacheadmin -addDirective -path /user/hadoop/shs -pool pool1
    Added cache directive 1

    # 캐쉬 확인 
     hdfs cacheadmin -listDirectives
    Found 1 entry
     ID POOL    REPL EXPIRY  PATH             
      1 pool1      1 never   /user/hadoop/shs
    ```

    ### 1-2. 세컨더리 네임노드

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/71ac3676-a856-47f7-81fd-7f622396ca62/Untitled.png)

    - 네임노드가 구동되고 나면 Edits 파일이 주기적으로 생성된다.
    - 네임노드의 트랜잭션이 빈번하면 빠른 속도로 Edits 파일이 생성되는데 이는 네임노드의 데이터 부족 문제를 생성할 수 있고, 네임노드가 재구동 되는 시간을 느려지게 할 수 있다.
    - **세컨더리 네임노드**는 Fsimage와 Edits 파일을 주기적으로 머지하여 최신 블록의 상태로 파일을 생성한다.
    - 또한, 파일을 머지하면서 Edits 파일을 삭제하기 때문에 디스크 부족 문제도 해결 할 수 있다!

### 2. HDFS Federation

- `더보기`

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6b338829-5125-4c47-8b28-8e83bb54210d/Untitled.png)

    - 네임노드는 파일 정보 메타데이터를 메모리에서 관리한다.
    - 파일이 많아지면 메모리 사용량이 늘어나게 되고, 메모리 관리가 문제가 된다.

        ➡ 이를 해결하기 위해, 하둡 v2 부터 HDFS 페더레이션을 지원한다.

    - HDFS 페더레이션은 디렉토리(네임스페이스) 단위로 네임노드를 등록하여 사용하는 것이다.
    - 예를들어, user, hadoop, tmp 세개의 디렉토리가 존재할 때 각각의 디렉토리 단위로 총 3개의 네임노드를 실행하여 파일을 관리하게 하는 것이다.
    - HDFS 페더레이션을 사용하면 파일, 디렉토리의 정보를 가지는 네임스페이스와 블록의 정보를 가지는 블록 풀을 각 네임노드가 독립적으로 관리한다.
    - 네임스페이스와 블록풀을 네임스페이스 볼륨이라고 한다.
    - 네임스페이스 볼륨은 독립적으로 관리되기 때문에 하나의 네임노드에 문제가 생겨도 다른 네임노드에 영향을 주지 않는다.

### 3. HDFS 고가용성

- `더보기`
    - HDFS는 네임노드가 단일 실패 지점이다.
    - 네임노드에 문제가 발생하면 모든 작업이 중지되고, 파일을 읽거나 쓸수 없게 된다.

        ➡ 이 문제를 해결하기 위해서 하둡 v2에서는 HDFS 고가용성(High Availability)을 제공한다!

    - HDFS 고가용성은 이중화된 두대의 서버인 `액티브(active) 네임노드`와 `스탠바이(standby) 네임노드`를 이용하여 지원한다.
    - 두 서버는 데이터 노드로부터 블록 리포트와 하트비트를 모두 받아서 동일한 메타데이터를 유지하고, 공유 스토리지를 이용하여 edits파일을 공유한다.

    ```bash
    **📌 액티브 네임노드(Active Namenode)**
    - 네임노드의 역할을 수행한다.

    **📌 스탠바이 네임노드(Standby Namenode)**
    - 스탠바이 네임노드는 액티브 네임노드와 동일한 메타데이터 정보를 유지한다.
    - 액티브 네임노드에 문제가 발생하면 스탠바이 네임노드가 액티브 네임노드로 동작하게 된다.
    - 또한, 스탠바이 네임노드는 세컨더리 네임노드의 역할을 동일하게 수행한다.
    - 따라서 HDFS를 고가용성 모드로 설정하였을 때는 세컨더리 네임노드를 실행하지 않아도 된다.
    	➡ 오히려 고가용성 모드에서 세컨더리 네임노드를 실행하면 오류가 발생한다!
    ```

    ### QJM(Quorum Journal Manager)

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/897d9bac-470e-4c1e-8429-7ada89bb426a/Untitled.png)

    - QJM은 HDFS 전용 구현체이다.
    - 고가용성 에디트 로그를 지원하기 위한 목적으로 설계되었고 HDFS의 권장 옵션이다.
    - QJM은 저널 노드 그룹에서 동작하며, 각 **에디트 로그는 전체 저널 노드에 동시에** 쓰여집니다. (일반적으로 저널 노드는 3개로 동작)

    ### NFS(Network File System)

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cb70ac53-d213-4d53-94c1-1f2520936853/Untitled.png)

    - NFS를 이용하는 방법은 **에디트 파일을 공유 스토리지를 이용하여 공유**하는 방법이다.
    - 공유스토리지에 에디트 로그를 공유하고 펜싱을 이용하여 **하나의 네임노드만 에디트 로그를 기록**하도록 합니다.

### 4. HDFS 세이프모드

- `더보기`
    - HDFS의 세이프모드(safemode)는 데이터 노드를 **수정할 수 없는 상태**이다.
    - 이 때는, 읽기 전용 상태가 되고, 데이터 추가와 수정이 불가능 하며 데이터 복제도 일어나지 않는다.
    - 관리자가 서버 운영 정비를 위해 세이프 모드를 설정 할 수도 있고, 네임노드에 문제가 생겨서 정상적인 동작을 할 수 없을 때 자동으로 세이프 모드로 전환된다.
    - 만약, 세이프모드에서 파일을 수정한다면 다음과 같은 오류를 출력한다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f2c172da-e7e3-4046-9f29-be288008e453/Untitled.png)

    ### 세이프 모드 커맨드

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/754aa14e-653e-4cd0-835b-2e8857eeafbc/Untitled.png)

    ### 세이프 모드의 복구

    - HDFS 운영 중 네임노드 서버에 문제가 생겨서 세이프 모드에 진입하는 경우는 네임노드 자체의 문제와 데이터 노드의 문제일 경우가 많다.
    - `fsck` 명령으로 HDFS의 무결성을 체크하고, `hdfs dfsadmin -report` 명령으로 각 데이터 노드의 상태를 확인하여 문제를 확인 및 해결한 후, 세이프 모드를 해제하면 된다.

### 5. HDFS 데이터 블록 관리

- `더보기`
    - HDFS 운영중 데이터 노드에 문제가 생기면, 데이터 블록에 문제가 발생 할 수 있다.
    - `커럽트(CORRUPT) 상태`와 `복제 개수 부족(Under replicated) 상태` 가 존재한다.

    ### 커럽트 블록

    - HDFS는 하트비트를 통해 데이터 블록에 문제가 생기는 것을 감지하고 자동으로 복구를 진행한다.
    - 다른 데이터 노드에 있는 데이터를 가져와서 복구한다.
    - 하지만 **모든 블록 노드에 문제가 생겨서 복구를 하지 못하게 되면 커럽트 상태**가 된다.

        ➡️ 커럽트 상태의 파일들은 삭제하고, 원본 파일을 다시 HDFS에 올려주어야 한다!

    ### 커럽트 상태 확인

    - HDFS 커맨드의 `fsck`를 이용하여 상태를 체크할 수 있다.
    - 이 명령어를 통해, 커럽트 상태의 블록과 복제 개수가 부족한 블록의 정보를 확인할 수 있다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9284a97b-6baf-4015-a15d-429bb661ffd5/Untitled.png)

    - 상태를 확인하고, 커럽트 상태이면 `hdfs fsck -delete`명령을 이용해 커럽트 상태의 블록을 **삭제**한다.
    - 커럽트 상태의 방지를 위해서 `hadoop fs -setrep` 명령을 이용하여 중요한 파일은 복제 개수를 늘려주면 좋다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/276b38e3-4d6e-4c3a-a7e5-7150075fa389/Untitled.png)

    ### 복제 개수 부족 상태

    - 복제개수 부족 상태는 파일에 지정된 복제 개수 만큼 데이터 블록에 복제되지 않았을 때 발생한다.
    - 이 상태는 데이터 노드의 개수보다 복제 개수를 많이 지정했을 때 발생할 수 있다.
    - `fsck` 명령으로 복제 개수가 부족한 파일을 확인할 수 있으며 복제개수가 부족한 파일은 `hadoop fs -setrep` 명령을 이용하여 복제 개수를 늘려주면 된다.

### 6. HDFS 휴지통

- `더보기`
    - HDFS는 사용자의 실수에 의한 파일 삭제를 방지하기 위해서 휴지통 기능을 제공한다.
    - 휴지통 기능이 설정되면 HDFS에서 삭제한 파일은 바로 삭제되지 않고, 각 사용자의 홈디렉토리 아래 휴지통 디렉토리 `(/user/유저명/.Trash)`로 이동된다.
    - **휴지통 아래의 파일은 복구할 수 있다.**
    - 휴지통 디렉토리는 지정한 간격으로 체크포인트가 생성되고, 유효 기간이 만료되면 체크포인트를 삭제한다.
    - 삭제되면 해당 블록을 해제하고, 사용자에게 반환한다.

    ### 휴지통 설정

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f99af327-6d3d-4523-855b-64e983215674/Untitled.png)

    ### 휴지통 설정값

    - core-site.xml에 아래와 같이 설정한다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/49b45514-4758-4fb6-adc6-4f0a82a7cb3c/Untitled.png)

    ### 휴지통 명령

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/da497f50-4758-4924-af47-04e40f7c9b07/Untitled.png)

### 7. HDFS 명령어

- `더보기`
    - HDFS 커맨드는 `사용자 커맨드`, `운영자 커맨드`, `디버그 커맨드`로 구분된다.

    ```bash
    1. 사용자 커맨드
    	1. 사용자 커맨드 목록
    	2. dfs 커맨드
    		- dfs 커맨드 명령어
    		- cat
    		- text
    		- ls
    		- mkdir
    		- cp
    		- mv
    		- get
    		- put
    		- rm
    		- setrep
    		- test
    		- touchz
    		- stat
    		- setfacl
    		- getfacl
    		- count
    	3. fsck 커맨드
    		- fsck 커맨드 명령어
    		- 사용법
    2. 운영자 커맨드
    	1. 운영자 커맨드 목록
    	2. fsck 커맨드 명령어
    	3. dfsadmin 커맨드
    		- 사용법
    		- 전체 명령어
    		- -report
    		- -safemode
    		- -triggerBlockReport
    	4. fetchdt
    ```

    ### 사용자 커맨드

    - 사용자 커맨드는 `hdfs`, `hadoop` 쉘을 이용할 수 있다.
    - 일부 커맨트는 `hdfs` 쉘을 이용해야한다.
    - 둘 다 이용할 수 있는 경우에는 각 쉘의 결과가 동일하다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/415e6410-2089-4368-81e0-bcb459984482/Untitled.png)

    ### 사용자 커맨드 목록

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/23c57c16-2b34-4b4e-a1a2-11d49605182c/Untitled.png)

    ### dfs 커맨드

    - dfs 커맨드는 파일시스템 쉘을 실행하는 명령어다.
    - `hdfs dfs`, `hadoop fs`, `hadoop dfs` 세가지 형태로 실행이 가능

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/225f9cda-3fe3-440e-a7d0-c11dda1f4615/Untitled.png)

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7ded6633-1733-4f3c-9538-b17beebe46dc/Untitled.png)

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a14bd862-ad91-42c6-9e89-e251b9f05738/Untitled.png)

    **1) cat**

    - 저장한 파일을 기본 입력으로 읽어서 출력한다.

    ```java
    $ hadoop fs -cat /user/file.txt
    ```

    **2) text**

    - 지정한 파일을 텍스트 형식으로 읽는다.
    - gzip, snappy 등의 형식으로 압축된 파일을 자동으로 텍스트 형식으로 출력해준다.

    ```java
    $ hadoop fs -text /user/file.txt
    ```

    **3) ls**

    - 주어진 경로의 파일 목록을 조회한다.
    - 주요 옵션 : `-h`, `-R`, `-u`

    ```java
    $ hadoop fs -ls /user/

    # 사람이 읽을 수 있는 형태로 파일사이즈를 메가, 기가로 변경하여 출력 
    $ hadoop fs -ls -h /user/

    # 하위 폴더까지 조회 
    $ hadoop fs -ls -R /user/

    # 액세스 시간을 조회. 기본설정은 생성 시간. 마지막 접근 시간을 확인하여 파일 정리 가능 
    $ hadoop fs -ls -u /user/
    ```

    **4) mkdir**

    - 디렉토리를 생성한다.
    - 주요 옵션 : `-p`

    ```java
    $ hadoop fs -mkdir /user/folder
    #
     /user/folder1/folder2를 생성, 상위 폴더가 없으면 자동으로 생성 
    $ hadoop fs -mkdir -p /user/folder1/folder2
    ```

    **5) cp**

    - HDFS 상의 파일을 복사한다. **(HDFS → HDFS)**

    ```java
    $ hadoop fs -cp /user/data1.txt /user/data2.txt
    ```

    **6) mv**

    - HDFS 상의 파일을 이동한다.
    - **파일 이름을 변경할 때도 사용한다.**

    ```java
    $ hadoop fs -mv /user/data1.txt /user/data2.txt
    ```

    **7) get**

    - HDFS의 파일을 **로컬에 복사할 때** 사용한다. **(HDFS → 로컬)**
    - 주요 옵션 : `-f`

    ```java
    $ hadoop fs -get /user/data1.txt ./

    # 동일한 이름의 파일이 존재하면 덮어씀 
    $ hadoop fs -get -f /user/data1.txt ./
    ```

    **8) put**

    - 로컬의 파일을 HDFS에 복사할 때 사용한다. (로컬 → HDFS)
    - 주요 옵션 : `-f`

    ```java
    $ hadoop fs -put ./data1.txt /user/

    # 동일한 이름의 파일이 존재하면 덮어씀 
    $ hadoop fs -put -f ./data1.txt /user/
    ```

    **9) rm**

    - HDFS의 파일을 삭제할 때 사용한다.
    - 주요 옵션 : `-r`, `-skipTrash`

    ```java
    $ hadoop fs -rm /user/data1.txt 
    # 디렉토리를 포함하여 하위의 모든 파일을 삭제, 디렉토리 삭제시 필요함 
    $ hadoop fs -rm -r /user/
    # 쓰레기통을 사용하지 않고 파일 삭제  
    $ hadoop fs -skipTrash /user/
    ```

    **10) setrep**

    - 디렉토리, 파일의 레플리케이션 팩터(= 복제 개수)를 수정한다.
    - 주요 옵션: `-R`

    ```java
    # 디렉토리의 복제개수를 5로 설정  
    $ hadoop fs -setrep 5 /user/

    # 하위의 모든 디렉토리, 파일의 복제개수를 5로 설정 
    $ hadoop fs -setrep 5 -R /user/
    ```

    **11) test**

    - 파일, 디렉토리의 존재 여부를 확인한다.
    - 주요 옵션: `-d`, `-e`, `-f`, `-s`, `-w`, `-r`, `-z`

    ```java
    # -d: 경로가 디렉토리이면 0을 반환합니다.
    # -e: 경로가 있으면 0을 반환합니다.
    # -f: 경로가 파일이면 0을 반환합니다.
    # -s: 경로가 비어 있지 않으면 0을 반환합니다.
    # -w: 경로가 존재하고 쓰기 권한이 부여 된 경우 0을 반환합니다.
    # -r: 경로가 존재하고 읽기 권한이 부여 된 경우 0을 반환합니다.
    # -z: 파일 길이가 0이면 0을 반환합니다.

    # /user/test.txt 파일이 존재하면 0을 반환 
    $ hadoop fs -test -e /user/test.txt
    ```

    **12) touchz**

    - 0 byte 파일을 생성한다.

    ```java
    $ hadoop fs -touchz /user/test.txt
    ```

    **13) state**

    - 주어진 포맷에 따른 파일의 정보를 확인한다.

    ```java
    # 주요 포맷
    # %y : 마지막 수정 시간 
    # %x : 마지막 접근 시간 
    # %n : 파일 이름 
    # %b : 파일 사이즈 (byte)
    $ hadoop fs -stat "%y %n" hdfs://127.0.0.1:8020/*
    ```

    **14) setfacl**

    - 파일의 ACL(Access Control List, 접근 제어 목록)을 설정한다.
    - ls명령으로 확인할 수 있는 파일의 권한과 **별도로 권한을 설정**할 수 있다.

    ```java
    # -m: 권한을 수정
    # -b: 설정한 권한을 삭제 
    # user a에게 /user/file의 읽기(r), 쓰기(w) 권한을 줌
    $ hadoop fs -setfacl -m user:a:rw- /user/file

    # /user/file에서 신규로 생성되는 파일, 디렉토리에 대하여 user a에게 의 읽기(r), 쓰기(w) 권한을 줌
    $ hadoop fs -setfacl -m default:user:a:rw- /user/file
    ```

    **15) getfacl**

    - 파일의 ACL을 확인한다.

    ```java
    $ hadoop fs -getfacl /user/file
    # file: hdfs:///user/file
    # owner: c
    # group: g
    user::rw-
    user:a:rw-
    group::r--
    mask::rw-
    other::---
    ```

    **16) count** 

    - 지정한 경로의 디렉토리 개수, 파일개수, 파일 사이즈를 확인한다.

    ```java
    # 디렉토리 개수 150
    # 파일 개수 2000
    # 디렉토리 용량 123456789
    $ hadoop fs -count /user
          150      2000     123456789 hdfs:///user

    # -h 사용자가 읽기 편하게 변환
    # -q 디렉토리의 쿼터 정보 확인: QUOTA, REMAINING_QUOTA, SPACE_QUOTA, REMAINING_SPACE_QUOTA, DIR_COUNT, FILE_COUNT, CONTENT_SIZE, PATHNAME
    # -u 디렉토리의 쿼터 정보 확인: QUOTA, REMAINING_QUOTA, SPACE_QUOTA, REMAINING_SPACE_QUOTA, PATHNAME
    # -v는 -q, -u와 함께 사용하면 헤더를 함께 출력합니다.
    ```

### 8. WebHDFS REST API 사용법

- `더보기`
    - HDFS는 **REST API**를 이용하여 파일을 `조회`하고, `생성`, `수정`, `삭제`하는 기능을 제공한다.
    - 이 기능을 이용하여 원격지에서 HDFS의 내용에 접근하는 것이 가능하다.

    ### REST API 설정

    - REST API를 사용하기 위해서는 `hdfs-site.xml`에 다음의 설정이 되어 있어야 한다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/10ccb59d-3a8c-4226-9eb3-d65f11f1e54b/Untitled.png)

    ### 파일 리스트 확인

    - 위에서 설정한 http 포트로 요청을 날리면 JSON 형식으로 결과를 반환한다.
    - curl명령을 이용하여 요청을 처리하는 방법은 다음과 같다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/94d390fe-cb41-4d83-a86e-ab0c20584f3b/Untitled.png)

### 9. HDFS 암호화

- `더보기`
    - HDFS는 민감정보의 보안을 위해 암호화 기능을 제공한다.
    - 암호화를 적용하면 디스크에 저장되는 파일을 암호화하여 저장할 수 있다.
    - 또한, HDFS의 디렉토리에 접근할 때 하둡 KMS를 이용하여 key 기반으로 전송 데이터의 암/복호화를 지원한다.

    ### 사용예제

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f20b5624-f087-4c02-8d41-d675a6fc3928/Untitled.png)

    ### 하둡 KMS REST API

    - 하둡 KMS는 REST API 서버도 제공한다. (기본포트: 9700)

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/42f4b8e8-54c7-4937-9849-16b12f18a642/Untitled.png)

### 10. HDFS 사용량 제한 설정

- `더보기`
    - HDFS 관리자는 **디렉토리 별로 파일 개수와 파일 용량을 제한**할 수 있다.
    - 각 설정은 개별 적으로 동작한다.

    ### 파일 개수 제한

    - 디렉토리별로 생성할 수 있는 `파일 개수`를 제한 할 수 있으며, 할당량을 초과하면 파일, 디렉토리를 생성할 수 없다!

    ### 파일 용량 제한

    - 디렉토리별로 `용량`을 제한할 수 있습니다. ****
    - **파일 용량만 포함되고, 디렉토리는 용량에 포함되지 않는다.**

    ### 제한 설정 명령

    - 파일 개수 제한, 파일 용량 제한은 `hdfs dfsadmin` 명령을 이용하여 설정할 수 있다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/88d8a699-e4f1-43d3-bd27-c79f4ec402e3/Untitled.png)

    ### 제한 명령 확인

    - 디렉토리 별로 설정된 제한은 `hadoop fs -count` 명령을 이용하여 확인할 수 있다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/767c3e6d-7550-4ded-8569-d75133d75a3c/Untitled.png)

### 11. 데이터 압축

- `더보기`
    - 데이터 압축 여부와 사용할 압축 형식은 성능에 큰 영향을 줄 수 있다.
    - 데이터 압축을 고려해야 할 가장 중요한 두 가지 요소는 `MapReduce 작업` 및 `HBase에 저장된 데이터 측면` 이다.

    ### 압축 유형

    ```bash
    1. GZIP
    	- Snappy 또는 LZO보다 많은 CPU 리소스를 사용하지만 더 높은 압축률을 제공한다.
    	- GZip은 콜드 데이터에 적합하다. (즉, 액세스가 드문 데이터!)
    	- Snappy 또는 LZO는 자주 액세스하는 핫 데이터에 더 적합하다.

    2. BZip2
    	- 압축 및 압축 해제시 빠른 속도로 일부 유형의 파일에 대해 GZip보다 더 많은 압축을 생성할 수 있다.
    	- HBase는 BZip2 압축을 지원하지 않는다.
    ```

    - **BZip2, LZO 및 Snappy 형식은 분할 가능하지만 GZip은 분할 할 수 없다.**
    - 따라서 MapReduce는 분할할 수 있어야 하는 경우에 따라 압축 방법을 선택해야 한다.
    - MapReduce의 경우 중간 데이터, 출력 또는 둘 다 압축 할 수 있다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d9fc2a36-da97-48f9-b7d6-8ba49932e993/Untitled.png)

### 12. RPC

- `더보기`
    - HDFS는 서버와 클라이언트 통신에 RPC(Remote Procedure Call)을 이용한다.
    - RPC는 원격지에 있는 노드의 함수를 실행하여 결과를 반환 받을 수 있다.

### 13. 이레이져 코딩 (EC)

- `더보기`

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/afb6396f-9596-4ba4-acb3-41bed9dd6d38/Untitled.png)

    ### 목적

    - HDFS는 3개의 복제 데이터 파일을 이용해서 내결함성을 제공한다.
    - 이러한 복제는 저장 공간, 네트워크 대역폭 등에서 오버헤드를 가지는 것으로 계산된다.

        ➡️ 이는 EC(Erasure Coding)을 이용해서 이런 문제를 해결할 수 있다!

        ➡️ EC는 훨씬 적은 저장 공간을 사용하여 동일한 수준의 내결함성을 제공하며, 파일의 복제 개수는 항상 1이다.

    ### 배경

    - 무슴소리지

### 14. 밸런서 (balance)

- `더보기`
    - HDFS를 운영하면서 데이터 불균형이 발생하여 밸런싱을 실행해야 하는 경우가 발생한다.

    ### 데이터 불균형이 발생하는 경우

    - **데이터 노드를 추가하는 경우**
        - 하둡의 데이터 저장 공간이 부족하여 데이터노드를 추가하는 경우
        - 다른 노드의 사용공간은 70~80% 정도인데 신규 데이터 노드는 사용공간이 0%
    - **대량의 데이터를 삭제하는 경우**
        - `특정` 데이터 노드에 블록이 많이 저장되어 데이터노드간 저장공간 차이가 20~30% 정도 발생하는 경우
    - **대량의 데이터를 추가하는 경우**
        - 특정 데이터 노드에 데이터가 적은 경우 네임노드는 데이터 저장공간이 작은 노드를 우선적으로 사용한다.
        - 이 때, 특정 노드로 I/O가 집중 되게 됨

    ### HDFS Balancer

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5e120730-c1df-4b77-904b-072c771022cd/Untitled.png)

    ### 대역폭

    - 밸런서는 작업 간 많은 데이터 이동이 발생하기 때문에 `대역폭을 지정`하여 다른 작업에 영햐이 가지 않도록 하는 것이 좋다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9225d3c7-1f0c-420b-a810-b1d49c326d1e/Untitled.png)

    ### threshold

    - 각 노드간 데이터 사용비율 차이를 입계값을 이용해서 지정할 수 있다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d3fa41f0-060c-49bb-a2ba-6a8c7d97a29a/Untitled.png)

    ### 추가 설정

    - `hdfs-site.xml`에 데이터 밸런싱을 위한 추가 설정을 해줄 수 있다.

    ```bash
    dfs.datanode.balance.max.concurrent.moves: 50
    -> 데이터노드가 데이터 이동에 사용할 스레드 개수

    dfs.datanode.balance.bandwidthPerSec: 10MB
    -> 데이터 이동에 사용하는 대역폭
    ```

## # 3. 맵리듀스

---

### 0. 맵리듀스

- `더보기`
    - 맵리듀스는 **간단한 단위작업**을 반복하여 처리할 때 사용하는 프로그래밍 모델이다.

    ```bash
    - Map : 간단한 단위작업을 처리
    - Reduce : 맵 작업의 결과물을 모아서 집계
    ```

    - 하둡에서 분산처리를 담당하는 맵리듀스 작업은 맵과 리듀스로 나누어져 처리된다.
    - 맵, 리듀스작업은 병렬로 처리가 가능한 작업으로, 여러 컴퓨터에서 동시에 작업을 처리하여 속도를 높일 수 있다.

    ### 맵리듀스 작업 단위

    - 하둡 v1의 작업 단위는 `job`이고, 하둡 v2의 작업 단위는 `애플리케이션(application)` 이다.

        ➡️ YARN 아키텍처가 도입되면서 이름은 변경되었지만 동일하게 관리된다!

    - `job`은 `Map 태스크`와 `Reduce 태스크`로 나누어진다.
    - 태스크는 `attempt`단위로 실행된다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5371e775-7725-4082-be6f-ee58cebba0ea/Untitled.png)

    - job에서 생성되는 Map 태스크와 Reduce 태스크는 아이디가 `attempt_xxx_xxx_m_000000_0`으로 생성된다.
    - Map 태스크는 중간아이디가 **m**으로 생성되고, Reduce 태스크는 **r**로 생성된다

    ### 맵리듀스 장애극복 (Failover)

    - 맵리듀스는 실행 중 오류가 발생하면 설정된 횟수만큼 자동으로 반복된다.
    - 오류가 발생하면 job ID의 마지막 숫자가 증가하면서 반복 횟수를 알려준다.

        ➡️ `attempt_xxx_xxx_m_000000_0`  → `attempt_xxx_xxx_m_000000_1`

    ### 맵 입력 분할

    - 맵의 입력은 스플릿(InputSplit) 단위로 분할된다.
    - 맵작업은 큰 데이터를 하나의 노드에서 처리하지 않고, 분할하여 동시에 병렬 처리하여 작업 시간을 단축한다.
    - 따라서, **스플릿이 작으면 작업 분하가 분산되어 성능을 높일 수 있다.**
    - 하지만 스플릿의 크기가 너무 작으면 맵 작업의 개수가 증가하고 맥 작업 생성을 위한 오버헤드가 증가하여 작업이 느려질 수 있다.

        ➡️ 따라서 적절한 개수의 Map 작업을 생성해야 한다! (일반적으로는 HDFS 블록 기본 크기만큼)

    ### 맵 작업 데이터 지역성

    - Map 작업은 HDFS에 입력 데이터가 있는 노드에서 실행할 때 가장 빠르게 동작한다.
    - 데이터가 있는 노드에서 작업을 처리할 수 없다면? 🤔

        1. 동일한 랙의 노드

        2. 다른 랙의 노드 

        순서로 맵 작업이 실행가능한 노드를 찾는다.

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2845d66e-792a-458c-a090-fc32128d67ef/Untitled.png)

    - Map 작업의 적절한 스플릿 크기가 HDFS 블록의 기본크기인 이유는 단일 노드에 해당 블록이 모두 저장된다고 확신할 수 있는 입력 크기이기 때문이다.
    - Map 작업의 결과는 로컬 디스크에 임시 저장된다.
    - 리듀스 작업은 Map 작업의 결과를 입력받기 때문에 지역성의 장점이 없다.
    - 리듀스 작업의 개수는 입력 크기와 상관없이 지정할 수 있다.

        ➡️ 리듀스의 개수 만큼 파티션을 생성하고 Map 함수의 결과를 각 파티션에 분배한다!

        ➡️ 이 때, 파티션별로 key가 존재하고 **key는 같은 파티션에** 전달된다.

    ### 맵리듀스 작업의 종류

    - 맵리듀스는 Reduce 작업이 있는 경우와 없는 경우가 있다.
    - 파일을 읽어서 바로 쓰는 작업의 경우는 Reducer가 필요없어서 Map만 있는 작업(Mapper Only)이 됩니다.
    - 집계를 진행해야 하는 경우, 정렬이 필요한 경우는 Reducer가 하나만 생성된다.
    - 나머지 경우에는 Reducer가 여러 개로 생성된다.

    **1) 리듀서가 하나인 경우**

    - 모든 데이터의 **정렬작업** 같은 경우이다.
    - 리듀서 하나로 모든 작업을 처리하기 때문에 시간이 오래걸린다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/effc2ed4-9547-4add-9578-6328903250ec/Untitled.png)

    **2) 리듀서가 여러 개인 경우**

    - 일반적인 집계 작업의 경우 리듀서가 여러개 생성된다.
    - 리듀서의 수만큼 파일이 생성된다.
    - HDFS의 부하를 방지하기 위해서 **추가적인 파일 머지 작업**이 필요할 수 있다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dc01cb70-5992-4695-89d8-beecd245a4cb/Untitled.png)

    **3) 리듀서가 없는 경우(Mapper Only 작업)**

    - 원천 데이터를 읽어서 가공을 하고 바로 쓰는 경우다.
    - 리듀서 작업이 없기 때문에 속도가 빠르다.
    - 매퍼의 수만큼 파일이 생성되기 때문에 **추가적인 파일 머지 작업**이 필요할 수 있다.

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/41164b93-1ca3-48f8-94eb-b20225485c19/Untitled.png)

### 1. 처리단계

- `더보기`

    ### 맵리듀스 처리 단계

    - 처리단계는 8단계로 크게 구분할 수 있다.

    ```bash
    1. **입력**
    	- 데이터를 입력하는 단계
    	- 텍스트, csv, gzip 형태의 데이터를 읽어서 Map으로 전달
    **2. 맵(Map)**
    	- 입력을 분할하여 key별로 데이터를 처리
    **3. 컴바이너(Combiner)**
    	- 네트워크를 타고 넘어가는 데이터를 줄이기 위해 Map의 결과를 정리
    	- 로컬 Reducer라고도 함
    	- 컴바이너는 작업의 설정에 따라 없을 수도 있다
    **4. 파티셔너(Partitioner)**
    	- Map의 출력 결과 key값을 해쉬처리하여 어떤 Reducer로 넘길지를 결정
    **5. 셔플(Shuffle)**
    	- 각 Reducer로 데이터 이동
    **6. 정렬(Sort)**
    	- Reducer로 전달된 데이터를 key값 기준으로 정렬
    **7. 리듀서(Reducer)**
    	- Reducer로 데이터를 처리하고 결과를 저장
    **8. 출력**
    	- Reducer의 결과를 정의된 형태로 저장
    ```

    ### 입력

    - 입력 단계는 InputFormat 클래스를 이용한다.

    **1) InputFormat**

    - 입력파일이 **분할되는 방식(InputSplit)**이나 **읽어들이는 방식(RecordReader)**을 정의하는 클래스이다.
    - InputFormat 추상 클래스를 상속하여 구현한다.
    - Hadoop은 파일을 읽기 위한 FileInputFormat이나, 여러 개의 파일을 한번에 읽을 수 있는 CombineFileInputFormat 등을 제공한다.

    ```java
    package org.apache.hadoop.mapreduce;

    @InterfaceAudience.Public
    @InterfaceStability.Stable
    public abstract class InputFormat<K, V> {

      /** 
       * Logically split the set of input files for the job.  
       */
      public abstract 
        List<InputSplit> getSplits(JobContext context
                                   ) throws IOException, InterruptedException;

      /**
       * Create a record reader for a given split. The framework will call
       */
      public abstract 
        RecordReader<K,V> createRecordReader(InputSplit split,
                                             TaskAttemptContext context
                                            ) throws IOException, 
                                                     InterruptedException;
    }
    ```

    **2) InputSplit**

    - InputSplist은 맵의 입력으로 들어오는 데이터를 분할 하는 방식을 제공한다.
    - 데이터의 위치와 읽어 들이는 길이를 정의한다.

    ```java
    package org.apache.hadoop.mapreduce;

    @InterfaceAudience.Public
    @InterfaceStability.Stable
    public abstract class InputSplit {
      /**
       * Get the size of the split, so that the input splits can be sorted by size.
       */
      public abstract long getLength() throws IOException, InterruptedException;

      /**
       * Get the list of nodes by name where the data for the split would be local.
       * The locations do not need to be serialized.
       */
      public abstract 
        String[] getLocations() throws IOException, InterruptedException;

      /**
       * Gets info about which nodes the input split is stored on and how it is
       * stored at each location.
       */
      @Evolving
      public SplitLocationInfo[] getLocationInfo() throws IOException {
        return null;
      }
    }
    ```

    **3) RecordReader**

    - RecordReader는 실제 파일에 접근하여 데이터를 읽어 들이는 역할을 한다.
    - 데이터를 읽어서 <key, value> 형태로 반환한다.

    ```java
    package org.apache.hadoop.mapreduce;

    import java.io.Closeable;
    import java.io.IOException;

    import org.apache.hadoop.classification.InterfaceAudience;
    import org.apache.hadoop.classification.InterfaceStability;

    /**
     * The record reader breaks the data into key/value pairs for input to the
     */
    @InterfaceAudience.Public
    @InterfaceStability.Stable
    public abstract class RecordReader<KEYIN, VALUEIN> implements Closeable {

      /**
       * Called once at initialization.
       */
      public abstract void initialize(InputSplit split,
                                      TaskAttemptContext context
                                      ) throws IOException, InterruptedException;

      /**
       * Read the next key, value pair.
       */
      public abstract 
      boolean nextKeyValue() throws IOException, InterruptedException;

      /**
       * Get the current key
       */
      public abstract
      KEYIN getCurrentKey() throws IOException, InterruptedException;

      /**
       * Get the current value.
       */
      public abstract 
      VALUEIN getCurrentValue() throws IOException, InterruptedException;

      /**
       * The current progress of the record reader through its data.
       */
      public abstract float getProgress() throws IOException, InterruptedException;

      /**
       * Close the record reader.
       */
      public abstract void close() throws IOException;
    }
    ```

    ### Mapper

    - Mapper는 사용자가 정의한 단순 반복작업을 수행한다.
    - 사용자는 Mapper 클래스를 상속하고 map() 메소드를 구현하면 된다.

    ```java
    1. run() 메소드를 보면 실제 매퍼 작업이 동작하는 방식을 알 수 있습니다. 
    2. setup() 메소드로 매퍼를 초기화하고
    3. RecordReader를 이용하여 데이터를 읽어서 map(키, 밸류) 함수를 호출합니다. 
    4. 데이터를 모두 처리할 때까지 반복합니다. 
    5. 마지막으로 cleanup() 메소드로 사용한 리소스의 반환 등을 처리합니다.
    ```

    ```java
    package org.apache.hadoop.mapreduce;

    @InterfaceAudience.Public
    @InterfaceStability.Stable
    public class Mapper<KEYIN, VALUEIN, KEYOUT, VALUEOUT> {

        /**
         * The <code>Context</code> passed on to the {@link Mapper} implementations.
         */
        public abstract class Context implements MapContext<KEYIN, VALUEIN, KEYOUT, VALUEOUT> {
        }

        /**
         * Called once at the beginning of the task.
         */
        protected void setup(Context context) throws IOException, InterruptedException {
            // NOTHING
        }

        /**
         * Called once for each key/value pair in the input split. Most applications
         * should override this, but the default is the identity function.
         */
        @SuppressWarnings("unchecked")
        protected void map(KEYIN key, VALUEIN value, Context context) throws IOException, InterruptedException {
            context.write((KEYOUT) key, (VALUEOUT) value);
        }

        /**
         * Called once at the end of the task.
         */
        protected void cleanup(Context context) throws IOException, InterruptedException {
            // NOTHING
        }

        /**
         * Expert users can override this method for more complete control over the
         * execution of the Mapper.
         */
        public void run(Context context) throws IOException, InterruptedException {
            setup(context);
            try {
                while (context.nextKeyValue()) {
                    map(context.getCurrentKey(), context.getCurrentValue(), context);
                }
            } finally {
                cleanup(context);
            }
        }
    }
    ```

    ### Combiner

    - MapReduce 프레임워크의 자원은 유한하기 때문에, Map 작업의 결과를 Reduce 작업의 입력으로 넣기 전에, 정리를 통해 데이터 전송에 필요한 자원을 줄일 수 있다.
    - 이때, Combiner 함수를 통해 처리할 수 있다.
    - 하지만 Combiner 작업에 사용할 수 있는 함수는 제약이 있다. 최대값, 최소값, 카운트같은 함수는 가능하나 평균 함수는 Mapper 작업의 결과를 평균 내어, Reduce 작업에 사용하면 최종 결과가 달라지게 되므로 사용할 수 없다.
    - 리듀스 함수를 컴바이너 함수로 사용할 수도 있지만, 컴바이너 함수를 리듀스 함수를 완전히 대체할 수도 없다.

        ➡️ 컴바이너 함수를 쓰더라도 리듀스 함수는 서로 다른 맵에서 오는 **같은 키의 레코드를 여전히 처리해야 한다**.

    ### Partitioner & Shuffle

    - Map 작업이 종료되면 작업이 **끝난 노드부터 Reducer로 데이터를 전달**하는데 이를 셔플이라고 한다.
    - Reducer로 데이터를 전달하기 위해서 **Map의 결과 key를 Reducer로 분배하는 기준을 만드는 것**을 파티션이라고 한다.
    - 기본적으로 사용되는 해쉬파티션을 보면 key의 값을 이용하여 Reducer의 개수만큼 파티션을 생성한다 → 같은 파티션은 같은 리듀서로 전달!

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a1b92945-13de-4e71-98a1-2f7fe06df996/Untitled.png)

    - 기본 파티셔너에서 Integer.MAX_VALUE값을 이용하여 논리합(&) 연산하는 이유는 파티션의 번호가 양수여야 하기 때문이다.

    ### Sort

    - Reduce 작업 전에 전달받은 key를 이용하여 정렬을 수행한다.
    - 또한, 실제 Reduce 작업을 수행하기 위해 데이터를 key-ValueList 형태로 만들기 위한 그룹핑 작업도 함께 수행한다.
    - 데이터를 정렬할 때, 파티션의 기준이 되는 주키(Primary Key)외 다른 값을 기준으로 정렬할 수 있도 있다 **➡️ Secondary Sort!**

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4bef3b3c-4f5c-4e7b-a1d6-91804d8eb950/Untitled.png)

    ### Reduce

    - Reducer는 key별로 정렬된 데이터를 이용하여 리듀서 작업을 진행한다.
    - 사용자는 Reducer 클래스를 상소하고 reduce() 메소드를 구현하면 된다

    ```java
    setup() : Reducer 초기화
    run() : 데이터를 읽어서 리듀스 작업 처리. 데이터를 모두 처리할 때까지 반복.
    cleanup() : 사용한 리소스의 반환
    ```

    ### 출력

    **1) OutputFormat**

    - 출력파일에 기록되는 형식을 설정한다.
    - Text 형식, CSV 형식 등을 설정하고 파일을 쓰는 방법을 설정한다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/379367ae-7a7e-4c4b-b8a3-1c9fb0742f48/Untitled.png)

    **2) RecordWriter**

    - 실제 파일 기록한다.

    ### 1-1. 워드 카운트

    - 입력된 파일의 문자수를 세는 프로그램이다.
    - 워드 카운트의 작업 단계는 다음과 같다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b8010a46-4ff2-4e2e-aff7-885710e0d7a6/Untitled.png)

    - 워드 카운트의 전체 소스코드는 크게 세부분으로 나눌 수 있다.

        1) Mapper를 구현한 TokenizerMapper

        2) Reducer를 구현한 IntSumReducer

        3) Job을 설정하는 main

        ### 작업준비

        - 먼저 입력으로 사용할 파일을 생성하고 HDFS에 복사한다.
        - 입력 파일의 형식은 다음과 같다.

        ```java
        $ cat word.txt 
        Deer Bear River
        Car Car River
        Deer Car Bear

        # 작업 파일 복사 
        $ hadoop fs -put ./word.txt /user/word/input/
        ```

        ### 실행

        - Hadoop 맵리듀스의 실행은 jar파일을 이용한다.
        - 워드카운트 예제를 빌드하여 jar파일을 생성하고 실행하면 된다.

        ```java
        $ hadoop jar Mapreduce.jar sdk.WordCount /user/word/input /user/word/output
        ```

    ### 작업 결과

    - 작업 결과는 part 파일로 생성된다.
    - _SUCCESS는 작업의 성공 여부를 알려주는 파일이다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f31f29d7-5d31-4e01-9bdc-dea2e9bc0df6/Untitled.png)

    ### 워드카운트 처리 단계

    **1) 입력**

    - 워드카운트는 파일로 데이터를 입력받는다.
    - 파일 위치를 파라미터로 전달하면 FileInputFormat을 이용해서 데이터를 읽는다.
    - FileInputFormat은 지정한 위치의 파일을 **라인단위**로 읽어서 맵에 전달한다.

    ```java
    FileInputFormat.addInputPath(job, new Path(args[0]));
    ```

    **2) 맵 (Map)**

    - Map은 전달받은 <key, value>에서 value 데이터를 공백 단위로 분할하여 문자수를 세어준다.
    - Map에서 데이터를 정리하지는 않는다.

    ```java
    [입력, <버퍼번호, 라인>]
    1, Deer Bear River
    16, Car Car River
    30, Deer Car Bear

    [출력, <문자, 1>]
    Dear 1
    Bear 1
    River 1
    Car 1
    Car 1
    River 1
    Dear 1
    Car 1
    Bear 1
    ```

    **3) 컴바이너 (Combiner)**

    - 컴바이너는 Job에 설정한다.
    - 설정이 필수는 아니지만, Reducer로 전달하는 결과를 줄일 수 있기 때문에 사용하면 성능을 향상 시킬 수 있다.

    ```java
    [입력, <문자, 1>]
    Dear 1
    Bear 1
    River 1
    Car 1
    Car 1
    River 1
    Dear 1
    Car 1
    Bear 1

    [출력, <문자, List(1)>]
    Dear List(1, 1)
    Bear List(1, 1)
    Car List(1, 1, 1)
    River List(1, 1)
    ```

    **4) 파티셔너, 셔플, 소트**

    - 워드카운트 예제에서 파티셔너, 셔플, 소트 단계는 따로 설정하지 않는다.

    **5) 리듀서 (Reducer)**

    - Reducer는 key별로 전달된 등장횟수를 모두 더하여 전체 횟수를 생성한다.
    - 문자가 key로 들어오고 등장횟수가 Iterable 형태로 전달된다.
    - 이 값들을 모두 더하여 결과를 출력한다.

    ```java
    [입력, <문자, List(1)>]
    Dear List(1, 1)
    Bear List(1, 1)
    Car List(1, 1, 1)
    River List(1, 1)

    [출력, <문자, 횟수>]
    Deer    2
    Bear    2
    Car 3
    River   2
    ```

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b5515d82-7184-47e9-9e0e-9cba186b1672/Untitled.png)

    **6) 출력**

    - 지정한 위치에 파일로 출력한다.
    - 이 때, 출력 파일의 개수는 Reducer의 개수와 동일하다

    ```java
    FileOutputFormat.setOutputPath(job, new Path(args[1]));
    ```

    ### 1-2. 워드카운트2 - 카운터, 분산캐쉬

    - 기존 워드카운트 예제를 보강한 내용이다

    ```java
    1) setup() 함수를 이용하는 방법
    2) 카운터를 이용하는 방법
    3) 분산 캐쉬를 이용하는 방법
    4) 옵션 파서를 이용하는 방법
    ```

    **1) setup() 함수를 이용하는 방법**

    - 맵리듀스 프레임워크는 map, reduce 함수전에 setup 함수를 호출한다.
    - 작업에 필요한 설정값과 전처리를 여기서 처리한다.
    - context에서 설정값을 가져와서 설정에 이용하면 된다

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6ce5b680-a57a-40f3-b16a-c1fc705ac3d6/Untitled.png)

    **2) 카운터를 이용하는 방법**

    - 카운터(Counter)는 enum을 이용하여 카운터를 등록하고, 컨텍스트에서 카운터를 가져와서 사용하면 된다.
    - 사용한 카운터는 로그에서 확인할 수 있다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c7db9cfb-0058-49c7-9adc-fe05f75506d6/Untitled.png)

    **3) 분산 캐쉬를 이용하는 방법**

    - 분산 캐쉬는 job에 `addCacheFile`을 이용하여 등록한다.
    - 맵 리듀스에서 이용할 때는 `getCacheFiles`를 이용한다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ea3d7245-eeba-4f5a-9a3a-74420d32ce25/Untitled.png)

    **4) 옵션 파서를 이용하는 방법**

    - 옵션 파서를 이용하면 개별적으로 설정을 추가하지 않아도 설정값을 효율적으로 추가할 수 있다.
    - 실행시점에 사용자가 입력한 설정값의 접두어를 분석하여 `conf`에 설정해준다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/238e4f03-c67e-410f-91a1-1baea3e282b7/Untitled.png)

### 2. 보조도구

- `더보기`
    - 맵리듀스 작업을 처리하는데 도움을 주는 유틸리티성 도구

    ### 카운터

    - Hadoop은 맵리듀스 잡의 진행상황을 확인할 수 있는 counter를 제공한다.

        ➡️ 맵리듀스의 작업상황, 입출력 상황을 확인할 수 있다.

    - 사용자가 카운터를 생성하여 사용할 수 도 있다.

    ### 분산 캐쉬(Distributed Cache)

    - 맵리듀스 Job에서 공유되는 데이터를 이용해야 할 때 분산 캐쉬를 사용한다.
    - 맵리듀스 Job에서 데이터를 조인해야 할 경우 이용할 수 있다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c7ff9332-3df4-4275-9431-d05a776c816e/Untitled.png)

### 3. 메모리 설정

- `더보기`
    - 맵리듀스의 메모리 설정은 `mapred-site.xml` 파일을 수정하여 변경할 수 있다.
    - [기본값 보러가기](https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml)

    ### Mapper와 Reducer 메모리 설정

    ```java
    yarn.app.mapreduce.am.resource.mb
    : 노드에서 애플리케이션 마스터를 실행할 때 할당하는 메모리

    yarn.app.mapreduce.am.command-opts
    : 애플리케이션 마스터의 힙사이즈

    mapreduce.map.memory.mb
    : 맵 컨테이너를 생성할 때 설정하는 메모리

    mapreduce.map.java.opts
    : 맵 컨테이너를 생성할 때 설정하는 자바 옵션

    Xmx 옵션을 이용하여 힙사이즈를 설정
    : 맵 컨테이너 메모리(mapreduce.map.momory.mb)의 80%로 설정

    mapreduce.map.cpu.vcores
    : 맵 컨테이너에서 사용 가능한 가상 코어 개수 (기본값은 1)

    mapreduce.reduce.memory.mb
    : 리듀스 컨테이너를 생성할 때 설정하는 메모리  
      맵 컨테이너 메모리(mapreduce.map.memory.mb)의 2배로 설정하는 것이 일반적

    mapreduce.reduce.java.opts
    : 리듀스 컨테이너를 생성할 때 설정하는 자바 옵션
      Xmx 옵션을 이용하여 힙사이즈를 설정  
      리듀스 컨테이너 메모리의 80%로 설정

    mapreduce.reduce.cpu.vcores
    : 리듀스 컨테이너의 코어 개수

    mapred.child.java.opts
    : 맵과 리듀스 태스크의 JVM 실행 옵션, Heap 사이즈 설정
      mapreduce.map.java.opts, mapreduce.reduce.java.opts 설정이 이 설정을 오버라이드 하여 설정
      기본 설정은 -Xmx200m
    ```

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c8768cc7-ebba-4831-bde5-3c877209f72b/Untitled.png)

    ### MR 엔진 메모리 설정

    - MR 컨테이너의 map을 reduce로 바꾸면 리듀서의 메모리 설정이다.

    ### TEZ 엔진 메모리 설정

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3d6d0301-6e21-45d2-a399-20adac03adf9/Untitled.png)

    ### Mapper 설정

    - 매퍼 처리 중 발생하는 임시 데이터의 처리를 위한 설정이다.

    ```java
    mapreduce.task.io.sort.mb
    : 맵의 출력 데이터를 저장할 환형 버퍼의 메모리 크기
      맵의 처리 결과를 설정한 메모리에 저장하고 있다가, io.sort.spill.percent 이상에 도달하면 임시 파일로 출력
      split/sort 작업을 위한 예약 메모리
      매퍼가 소팅에 사용하는 버퍼 사이즈를 설정
      디스크에 쓰는 횟수가 줄어듬

    mapreduce.map.sort.spill.percent
    : 맵의 출력데이터를 저장하는 버퍼(mapreduce.task.io.sort.mb)가 설정한 비율에 도달하면 로컬 디스크에 임시 파일 출력

    mapreduce.task.io.sort.factor
    : 하나의 정렬된 출력 파일로 병합하기 위한 임시 파일의 개수

    mapreduce.cluster.local.dir
    : 임시 파일이 저장되는 위치
    ```

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/84ecee3c-b950-436b-a063-b74b95c9a6cc/Untitled.png)

    ### Suffle 설정

    - 셔플단계의 설정은 맵에서 전달받은 임시데이터를 복사하는 메모리와 데이터를 복사하는 스레드의 개수 등이 있다.

    ```java
    mapreduce.reduce.shuffle.parallelcopies
    : 셔플단계에서 맵의 결과를 리듀서로 전달하는 스레드의 개수

    mapreduce.reduce.memory.total.bytes
    : 셔플단계에서 전달된 맵의 데이터를 복사하는 메모리 버퍼의 크기
      기본값은 1024MB

    mapreduce.reduce.shuffle.input.buffer.percent
    : 메모리 버퍼 크기의 비율을 넘어서면 파일로 저장하는데 이 비율을 지정해주는 설정 값
      mapreduce.reduce.memory.total.bytes * mapreduce.reduce.shuffle.input.buffer.percent 의 사이즈를 넘어서면 파일로 저장 
      기본값은 0.7

    mapreduce.reduce.shuffle.memory.limit.percent
    : 메모리 버퍼의 크기에 비해 파일의 비율이 이 설정을 넘어서면 바로 디스크에 쓰여짐
      mapreduce.reduce.memory.total.bytes * mapreduce.reduce.shuffle.memory.limit.percent 크기를 넘어서면 파일로 저장
      기본값은 0.25
    ```

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6377b5f1-beea-45b1-bd33-77087aa61b30/Untitled.png)

    ### Mapper, Reducer 개수 설정

    - 매퍼와 리듀서의 개수를 원하는 대로 설정할 수 있다.

    ```java
    mapreduce.job.maps
    : 매퍼의 개수 설정

    mapreduce.job.reduces
    : 리듀서 개수 설정
    ```

    - 입력 사이즈에 따라 매퍼의 개수를 설정하기 위해서는 다음과 같이 설정하면 된다.

    ```java
    mapreduce.input.fileinputformat.split.maxsize
    : 매퍼에 입력가능한 최대 사이즈
      처리하려고 하는 총 size/mapreduce.input.fileinputformat.split.maxsize = 매퍼 개수

    mapreduce.input.fileinputformat.split.minsize
    : 매퍼에 입력가능한 최소 사이즈
    ```

    ### 단계별 메모리 설정

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8536791d-4065-4b01-b30b-cda9fb398e77/Untitled.png)

### 4. 성능 최적화

- `더보기`
    - 맵리듀스는 여러 단계를 거치기 때문에 설정 하나를 바꾸었다고 성능을 높이기가 쉽지 않다.
    - 작업 속도가 느린 이유는 여러가지가 있을 수 있기 때문에 다양한 방식으로 수정해보는 것이 좋다.

    ### Mapper, Reducer 수 설정

    - Mapper 수와 Reducer 수에 따라 작업 속도가 빨라질 수 있다.
    - Mapper 하나에 많은 파일이 몰리거나, 메모리를 많이 사용하는 작업이어서 GC(Garbage Collect)에 많은 시간이 걸려서 느려질 수 있다.
    - 따라서, 원천 데이터의 입력 사이즈에 따라 Mapper, Reducer의 개수를 조절해주는 것이 좋다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/390b4835-2c70-4706-ab9a-cf3b08138c52/Untitled.png)

    ### 정렬 속성 튜닝

    - Map 작업은 임시 결과 파일 개수를 줄이는 것으로 성능을 개선할 수 있다.
    - **로컬 디스크에 저장된 파일이 줄어들수록** 맵 출력 데이터의 병합, 네트워크 전송, 리듀서의 병합작업 시간이 단축된다.
    - 메모리 버퍼 크기인 `io.sort.mb`를 늘리면 메모리 버퍼가 커져서 로컬에 저장될 출력 데이터가 줄어들게 된다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/afed728d-b923-4084-a37b-50a2be631c29/Untitled.png)

    ### 컴바이너 클래스 적용

    - Combiner를 적용하면 Reducer의 입력이 줄어줄어서 네트워크 사용량을 줄이고 Reducer의 작업 속도를 향상시킬 수 있다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/faf83973-7c26-4807-9d24-5fcf77f53bed/Untitled.png)

    ### Map 출력 데이터 압축

    - 맵 출력 데이터를 **압축하여** 네트워크 트래픽을 줄여 주면 플레인 텍스트를 이용할 때보다 속도가 증가 할 수 있다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7b31f2b5-bc2b-4342-b130-d45a212c85e6/Untitled.png)

    ### 작은 파일문제 수정

    - 네임노드는 파일의 메타데이터와 블록을 관리하는 데 많은 메모리를 사용한다.
    - 이 때, 작은 사이즈의 파일이 여러 개 존재하게 되면 이 파일들을 관리하는데 **많은 메모리가 사용**되고, 맵리듀스 작업 처리중 많은 요청을 처리하기 되어 **네임노드에 병목현상**이 발생하게 되어 작업 속도가 느려지게 된다.

        ➡️ 이를 방지하기 위해서 **작은 사이즈의 파일을 하나의 파일로 합쳐서** HDFS 블록사이즈 크기의 파일로 설정하는 것이 좋다!

### 5. 예제

- `더보기`

    ```java

    1. 관리 기관명 기준으로 건수 확인
    	1) 매퍼
    	2) 리듀서
    	3) Job
    	4) 실행결과
    2. 관리 기관명(키) 기준으로 정렬하여 건수 확인
    	1) 파티셔너
    	2) job
    	3) 실행결과
    3.관리기관, 설치목적 기준으로 정렬하여 건수 확인
    	1) 매퍼
    	2) 복합키
    	3) 파티셔너
    	4) SortComparator
    	5) GroupingComparator
    	6) 리듀서
    	7) Job
    	8) 실행결과
    ```

    예제는 참고하는 게 좋을 듯 → [링크](https://wikidocs.net/23717)

## # 4. YARN

---

### 0. YARN

- `더보기`
    - YARN(Yet Another Resource Negotiator)은 하둡2에서 도입한 클러스터 리소스 관리와 애플리케이션 라이프사이클 관리를 위한 아키텍쳐이다.

    ### 등장배경

    - 하둡1에서는 Job Tracker가 애플리케이션의 라이프 사이클 관리와 클러스터 리소스 관리를 모두 담당하여 **병목 현상**이 일어났다.

        ➡ 또한, Job Tracker는 슬롯 단위로 리소스를 관리하여, 시스템의 전체 자원을 효율적으로 사용할 수 없었다.

        ➡ 100GB의 메모리를 가지는 클러스터를 1G로 구분하여 100개의 슬롯을 만들고, 60개의 맵 슬롯, 40개의 리듀서 슬롯으로 구분한다. 이 때, 각 슬롯은 역할에 맞게 동작하므로 맵 슬롯이 동작하는 동안 리듀서 슬롯은 대기하게 된다.

    - Job Tracker의 애플리케이션은 맵리듀스 작업만 처리할 수 있어서 유연성이 부족했다.

        ➡ 때문에 SQL 기반 작업의 처리나, 인메모리 기반의 작업 처리에 어려움이 있었다.

        ➡ 이런 단점을 극복하기 위해 **YARN 아키텍처**가 도입되었다.

    ### YARN 구성

    - Job Tracker의 기능을 분리하여 다음과 같이 나누어졌다.

    ```java
    - **자원 관리** : 리소스 매니저, 노드매니저
    - **애플리케이션 라이프 사이클 관리** : 애플리케이션 마스터, 컨테이너
    ```

    ### 자원관리

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b5e5a2aa-9deb-4826-aa94-a36a332ced5b/Untitled.png)

    - 클러스터 자원 관리는 **리소스 매니저**와 **노드매니저**를 이용하여 처리한다.
    - 노드 매니저는 클러스터의 각 노드마다 실행되며, 현재 노드의 자원상태를 관리하고 **리소스매니저에 현재 상태를 보고**한다.
    - 리소스매니저는 노드매니저로 부터 전달받은 정보를 이용하여 **클러스터 전체의 자원을 관리**한다.
        - 자원 상태를 모니터링하고, 애플리케이션 마스터에서 자원을 요청하면 비어있는 자원을 사용할 수 있도록 처리한다.
        - 자원을 분배하는 규칙을 설정하는 것이 스케쥴러(Scheduler)이다.
        - 스케줄러에 설정된 규칙에 따라 자원을 효율적으로 분배한다.

    ### 라이프 사이클 관리

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5b6d492d-8163-47a5-90bf-3cfb7bf77b01/Untitled.png)

    - 애플리케이션 라이프 사이클 관리는 **애플리케이션 마스터**와 **컨테이너**를 이용한다.
    - 클라이언트가 리소스 매니저에 애플리케이션을 제출하면, **리소스 매니저는 비어 있는 노드에서 애플리케이션 마스터를 실행**한다.
    - 애플리케이션 마스터는 작업에 필요한 자원을 리소스 매니저에게 요청하고, 자원을 할당받아서 각 노드에 컨테이너를 실행하고 실제 작업을 진행한다.
    - 즉, 컨테이너는 실제 작업이 실행되는 단위이다!
    - 컨테이너에서 작업이 종료되면 결과를 애플리케이션 마스터에게 알리고 애플리케이션 마스터는 모든 작업이 종료되면 리소스 매니저에 알리고 자원을 해제한다.

    ### 다양한 애플리케이션

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6a4fed33-4868-4d1e-b2f0-c9f291ef3185/Untitled.png)

    - YARN에서는 맵리듀스 API로 구현된 프로그램 외에도 다양한 애플리케이션을 실행할 수 있다.
    - 맵리듀스, 피그, 스톰, 스파크 등 하둡 에코 시스템의 다양한 데이터 처리 기술을 이용할 수 있다.

### 1. YARN-Scheduler

- `더보기`
    - 리소스 매니저는 클러스터 자원을 관리하고, 애플리케이션 마스터의 요청을 받아서 자원을 할당한다.
    - 자원 할당을 위한 정책을 스케쥴러라고 한다.
    - 하둡에서 제공하는 기본 스케쥴러는 다음과 같다.

    ```java
    - FIFO 스케줄러
    - Fair 스케줄러
    - Capacity 스케줄러
    ```

    ### 스케줄러 설정

    - 스케줄러는 yarn-site.xml 파일에 설정한다.
    - `yarn.resourcemanager.scheduler.class` 파일에 다음과 같은 클래스명을 적어주면 된다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d778facf-c22f-42bf-a018-a412ba918be8/Untitled.png)

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/46a4acc5-cbf5-4937-88b7-3cbde8388892/Untitled.png)

    ### FIFO Scheduler

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d2fb8d86-9441-4cbf-871e-110edbebf4e9/Untitled.png)

    - FIFO 스케줄러는 이름과 같이 **먼저 들어온 작업이 먼저 처리**된다.
    - 먼저 들어온 작업이 종료될 때 까지 다음 작업은 대기한다.

        ➡️ 이로 인해, 작업을 효율적으로 사용할 수 없기 때문에 FIFO는 테스트 목적으로만 사용!

    ### Capacity Scheduler

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9fc98a79-756a-4f50-b659-29528ec0d48c/Untitled.png)

    - **하둡 2의 기본 스케줄러**이다.
    - 트리 형태로 큐를 선언하고 **각 큐별로 이용할 수 있는 자원의 용량을 정해주면** 그 용량에 맞게 자원을 할당해준다.

    ### Fair Scheduler

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e105fc3b-9f20-46bf-a186-b7486128bd41/Untitled.png)

    - Fair 스케줄러는 제출된 작업이 **동등하게** 리소스를 점유한다.
    - 작업 큐에 작업이 제출되면 클러스터는 자원을 조절하여 작업에 균등하게 자원을 조절하여 할당해준다.

    ### Capacity Scheduler

    ```java
    1. 스케줄러 설정 값
    	- capacity-scheduler.xml 주요 설정
    2. 사용자 큐 매핑(queue-mappings)
    3. 스케줄러 설정
    	- 큐의 계층 구조
    	- capacity-scheduler.xml 설정
    4. 스케줄러 설정 확인
    	- 큐 설정 변경
    5. 스케줄러 설정시 주의 사항
    	- 참고
    ```

    - Capacity Scheduler는 트리 형태로 계층화된 큐를 선언하고, 큐별로 사용가능한 용량을 할당하여 자원을 관리한다.
    - 만약 클러스터의 자원에 여유가 있다면 설정을 이용하여 각 큐에 설정된 용량 이상의 자원을 이용할 수 있다.
    - 운영중에도 큐를 추가할 수 있는 유연성도 가지고 있다.

    ### 사용자 큐 매핑 (queue=mappings)

    - 사용자가 큐를 지정하지 않아도, 자동으로 사용자와 큐가 매핑되게 설정할 수 있다.

    ```java
    - u:유저명:큐
    	➡ 유저와 큐를 매핑
    - g:그룹명:큐
    	➡ 그룹과 큐를 매핑
    - u:%user:%user
    	➡ 유저를 유저명 큐에 매핑
    - u:%user:%primary_group
    	➡ 유저를 유저의 프라이머리 그룹명 큐에 매핑
    ```

    - `queue-mappings` 설정에 정의된 순서에 따라 큐를 적용한다.
    - 큐가 생성되어 있지 않으면 오류가 발생하게 된다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6f8bda6e-11dc-48cb-9369-68a4dd793373/Untitled.png)

    ### 스케줄러 설정

    - 다음은 계층구조로 큐를 설정하는 예제이다.
    - 아래에서 사용하는 큐는 `prod`, `dev`, `eng`, `science`이다.
    - root 아래 prod, dev가 등록되고, dev 아래 eng, science 큐가 등록되어 있다.

    ### capacity-scheduler.xml 설정

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ead9e72a-c6c5-4765-a3ce-7a9ff06a815d/Untitled.png)

    ### 스케줄러 설정 확인

    - 설정된 큐의 목록과 현재 사용중인 용량을 확인하는 명령어는 다음과 같다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bdf53064-b41d-43cc-a5ff-4b04a9ce7647/Untitled.png)

    ### 큐 설정 변경

    - 운영중 스케줄러의 설정을 변경할 때는 `capacity-scheduler.xml` 파일을 수정하고 다음 명령을 통해 설정을 변경하면 된다.
    - 큐를 삭제할 때는 큐를 STOPPED 상태로 변경 후 **처리중인 작업이 없을 때** 삭제 가능하다!

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9e41bc33-5f19-42c0-aa06-4a6bbe17d08d/Untitled.png)

    ### 스케줄러 설정 시 주의 사항

    - Capacity-Scheduler를 설정할 때는 큐의 역할에 따라 용량을 잘 배분하고, 최대 사용 가능 용량과 사용자 제한을 이용하여 사용할 수 있는 자원의 용량을 제한해주는 것이 중요하다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/356b1943-c564-471f-bf69-543cef2264aa/Untitled.png)

    - 애플리케이션 마스터에 할당되는 용량 비율을 잘 조절해야한다.

        ➡ 제출된 작업이 많아지면서 애플리케이션 마스터가 많은 자원을 가져가면, 컨테이너를 생성할 자원이 부족해지기 때문이다.

        ➡ 작업의 종류에 따라 적절한 비율을 찾는 것이 중요하다!

    ### Fair Scheduler

    ```java
    1. 스케줄러 설정 값
     - yarn-site.xml
     - fair-scheduler.xml
     - 참고
    ```

    - Fair Scheduler는 제출된 작업이 동등하게 리소스를 점유할 수 있도록 지원한다.
    - 작업 큐에 작업이 제출되면 클러스터는 자원을 조절하여 모든 작업에 균등하게 자원을 할당한다.
    - 트리 형태로 계층화된 큐를 선언하고, 큐별로 사용가능한 용량을 할당하여 자원을 관리한다.

    ### 스케줄러 설정 값

    - Fair Scheduler는 `yarn-site.xml`에 **스케줄러 관련 설정**을 하고, `fair-scheduler.xml`에 큐 관련 설정을 저장한다.
    - 10초마다 설정파일을 읽어서 큐 설정을 갱신한다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0622725d-84e0-43c4-96f4-efc41a7e7f3f/Untitled.png)

    ### fair-scheculer.xml

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/390936ba-451f-457d-a57a-425704c4d1f4/Untitled.png)

### 2. YARN-메모리 설정

- `더보기`
    - YARN의 메모리 설정은 `yarn-site.xml`파일을 수정하여 변경할 수 있다.
    - 노드 매니저의 메모리, CPU 개수, 컨테이너에 할당할 수 있는 최대, 최소 메모리 등을 설정할 수 있다.

    ### 리소스 매니저 설정

    ```java
    yarn.nodemanager.resource.memory-mb
    : 클러스터의 각 노드에서 컨테이너 운영에 설정할 수 있는 메모리의 총량
      노드의 OS를 운영할 메모리를 제외하고 설정
      기본값은 장비에 설정된 메모리의 80% 정도를 설정
      노드의 메모리가 32G인경우 운영체제를 위한 4G를 제외하고 28G를 설정

    yarn.nodemanager.resource.cpu-vcores
    : 클러스터의 각 노드에서 컨테이너 운영에 설정할 수 있는 CPU의 개수
      기본값은 장비에 설치된 CPU의 80% 정도를 설정
      노드에 설치된 CPU가 40개일 경우 32를 설정

    yarn.scheduler.maximum-allocation-mb
    : 하나의 컨테이너에 할당할 수 있는 메모리의 최대값
      8G가 기본 값

    yarn.scheduler.minimum-allocation-mb
    : 하나의 컨테이너에 할당할 수 있는 메모리의 최소값
      1G가 기본값

    yarn.nodemanager.vmem-pmem-ratio
    : 실제 메모리 대비 가상 메모리 사용 비율
      mapreduce.map.memory.mb * 설정값의 비율로 사용 가능 
      메모리를 1G로 설정하고, 이 값을 10으로 설정하면 가상메모리를 10G 사용

    yarn.nodemanager.vmem-check-enabled
    : 가상 메모리에 대한 제한이 있는지 확인하여, true 일 경우 메모리 사용량을 넘어서면
      컨테이너를 종료
      false 로 설정하여 가상메모리는

    yarn.nodemanager.pmem-check-enabled
    : 물리 메모리에 대한 제한이 있는지 확인하여, true 일 경우 메모리 사용량을 넘어서면 
      컨테이너를 종료
    ```

    ### yarn-site.xml 설정 예제

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c2860f88-912d-46a8-8e03-f6105b73e2bd/Untitled.png)

### 3. YARN 명령어

- `더보기`
    - HDFS 커맨드는 **사용자 커맨드**, **운영자 커맨드**로 구분된다.

    ```java
    **1. 사용자 커맨드**
     - 사용자 커맨드 목록
     - application 커맨드
       - 작업 목록 확인
       - 작업 상태 확인
       - 작업 종료
     - applicationattempt 커맨드
     - container 커맨드
     - logs 커맨드

    **2. 운영자 커맨드**
    - 운영자 커맨드 목록
    - rmadmin
       - 큐정보 갱신
    ```

    ### 사용자 커맨드

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/047084fd-3cfa-4914-881b-5405c0bcddc4/Untitled.png)

    ### application 커맨드

    - 애플리케이션의 정보를 확인하고 작업을 종료하는 명령어다.

    **1) 작업 목록 확인**

    - `-list` 옵션을 이용해서 현재 작업중인 작업 목록을 확인할 수 있다.
    - 작업 상태, 진행 상황, 작업 큐, 트래킹 URL 정보를 확인할 수 있다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/aab67866-a436-4bb9-986b-b644297fc3f9/Untitled.png)

    **2) 작업 상태 확인**

    - 작업 목록에서 확인한 Application-ID를 통해 현재 작업 상태를 확인할 수 있다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c5fc8f91-0af4-4cd6-9c03-32e009841556/Untitled.png)

    **3) 작업 종료**

    - -kill 옵션을 이용해서 작업을 강제로 종료할 수 있다.
    - 작업 목록에서 확인한 Application-ID 혹은 Job-ID를 이용하면 된다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cea23dc2-74e3-43a5-8780-38aaa4953b16/Untitled.png)

    ### applicationattempt 커맨드

    - 애플리케이션의 현재 시도(attempt) 정보를 확인할 수 있다.
    - 애플리케이션은 설정에 따라 오류가 발생하면 자동으로 재작업하기 때문에 관련된 정보를 확인할 수 있다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8dfa4ddd-1586-498e-b7ae-91e2075ec564/Untitled.png)

    ### container 커맨드

    - 현재 애플리케이션이 동작중인 컨테이너의 정보를 확인할 수 있다.
    - Attempt-ID를 이용하여 정보를 확인한다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fe2a23a1-b753-472c-8df5-9d87301ea29b/Untitled.png)

    ### logs 커맨드

    - logs 커맨드는 작업이 종료된 Job의 로그를 확인하는 명령어다.
    - 작업중인 Job은 히스토리 서버에 저장되기 전이라서 로그를 확인할 수 있다.

        ➡ 작업중인 Job은 작업목록에서 확인한 **트래킹 URL**에 접속해서 확인할 수 있다!

        ➡ CLI 환경이라면 `lynx` 커맨드를 이용하면되고, GUI라면 웹으로 접속해서 확인하면 된다.

    ### 운영자 커맨드

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0e75ac1b-e3e5-455f-b9e7-16435ec58b19/Untitled.png)

    ### rmadmin

    - `rmadmin` 옵션을 이용해서 리소스 매니저에 등록된 **큐와 노드 정보를 갱신**할 수 있다.

    **1) 큐 정보 갱신**

    - Capacity Scheduler의 설정 파일인 capacity-scheduler.xml을 다시 읽어서 정보를 갱신한다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6faaf974-3e12-49ec-9d34-c632b66a19aa/Untitled.png)

### 4. YARN REST API

- `더보기`
    - 리소스 매니저는 REST API를 제공한다.
    - 이를 통해 클러스터의 **상태정보**, **운영정보**를 ****확인할 수 있다.
    - 응답 형태는 JSON, XML 형태로 제공된다.

    ### 클러스터 메트릭 정보 확인

    - 메트릭 정보를 확인하는 URI는 다음과 같고 해당 URL를 `GET 방식`으로 호출하면 된다.
    - 헤더에 `{ 'Content-Type': 'application/json' }`로 정보를 설정하면 JSON 형식으로 값을 반환한다.

        ➡ 설정하지 않으면 XML!

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cb2a2eb0-8fc4-410a-9aa0-9bc8b31980c8/Untitled.png)

    ### 메트릭 확인 REST API 예제

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e5a804ea-af9f-417b-b489-9b8c8ef91019/Untitled.png)

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ff5db158-3484-4637-88f9-b2100c19e957/Untitled.png)

### 5. YARN Node Labels

- `더보기`
    - YARN은 2.6 버전부터 **노드 레이블(Node Label)** 기능을 제공한다.

    ```java
    **📌 노드 레이블이란?
    :** 서버를 특성에 맞게 구분하여 작업을 처리하는 기능을 제공하는 것

    ****
    예를들어 어느 회사에서 클러스터를 Batch, Stream 큐로 구분하여 사용한다고 하자.
    여기에 SSD를 설치한 서버와 GPU를 설치한 서버를 추가한다.
    SSD를 설치한 서버는 IO가 많은 작업에 유리하고, 
    GPU를 설치한 서버는 연산이 많은 작업에 유리할 것이다.
    기존의 방식대로 클러스터에 서버를 추가하면 
    리소스 매니저는 서버의 특성에 대한 구분없이 여유가 있는 서버에서 작업을 진행할 것이다.

    이때 노드 레이블을 이용하여 각 서버에 유리한 작업을 등록할 수 있다!

    SSD를 설치한 서버는 SSD, GPU를 설치한 서버는 GPU 레이블로 설정하고
    Batch 큐는 SSD 레이블, Stream 큐는 GPU 레이블을 이용하도록 설정하면
    리소드 매니저는 Batch 큐에 들어온 작업을 처리할 때 SSD 레이블이 설정된 서버에서 작업하도록 한다.

    ➡ 이를 통해 **서버의 특성에 따라 작업을 구분**할 수 있게 되고 **작업 성능**을 향상 시킬 수 있다!
    ```

    ### 특징

    - 하나의 노드는 하나의 파티션을 가질 수 있다.
    - 접근 제어 (ACL)
        - 큐에서 설정된 노드만 작업에 이용할 수 있다.
    - 클러스터 사용량 제어
        - 각 노드마다 사용할 수 있는 자원의 비율을 지정하여 사용량을 제어할 수 있다.

    ### 파티션의 종류

    - Exclusive 파티션
        - 클러스터의 자원에 여유가 있더라도 지정한 파티션만 이용하여 작업을 처리
    - Non-Exclusive 파티션
        - 클러스터에 여유가 있다면 Default 파티션으로 요청한 작업에 대해서 처리 가능

    ### 설정

    ### 노드 레이블 설정

    - **리소스 매니저에서 노드 레이블을 지원하도록** 하기 위해서 `yarn-site.xml`에 다음과 같이 설정한다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/db6fa2ad-155c-44f6-a5ac-0edfb27ed216/Untitled.png)

    ### 노드 레이블 관련 명령어

    - `yarn` 명령어를 통해 노드 레이블을 추가한다.

    **1) 클러스터의 레이블 정보 확인**

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3a1bc930-a3b8-4711-a54a-2b89c71a0b70/Untitled.png)

    **2) 클러스터의 노드 레이블 추가 및 삭제**

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a611c125-6456-4aa6-be98-b9fad7b1f22d/Untitled.png)

    **3) 노드 레이블 추가 및 삭제**

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/719cb054-790b-42f1-b4ce-534392c14d6d/Untitled.png)

    ### 스케줄러 설정

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/15922ec3-37ac-496b-9a43-060da9a75960/Untitled.png)

### 6. YARN 고가용성

- `더보기`
    - YARN은 **리소스 매니저가 단일 실패 지점**이다.

        ➡ 리소스 매니저에 문제가 발생하면 자원관리, 작업관리 기능을 사용할 수 없다! ❌

        ➡ 따라서, 하둡 2.4 버전부터는 **HA 기능**을 제공한다.

        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5abeba18-f48b-47ba-adaa-73030ce4397d/Untitled.png)

    ### 리소스 매니저 HA

    - 리소스 매니저 고가용성은 주키퍼와 액티브, 스탠바이 리소스 매니저를 이용하여 제공한다.
    - 각 리소스 매니저는 클러스터와 작업상태 보관을 위한 **메타 데이터 저장소를 공유**한다.
    - 주키퍼를 이용한 `ZKRMStateStore`와 파일 시스템을 이용한 `FileSystemRMStateStore`가 있다.

        기본적으로는 `ZKRMStateStore` 를 추천한다!

    ### 장애 극복

    - 리소스 매니저에 장애가 발생하면 사용자가 수동으로 CLI 커맨드를 이용하여 액티스 리소스 매니저로 변경할 수 있다.
    - 주키퍼를 이용하면 자동으로 리소스 매니저의 상태가 변경된다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3fbfc812-173e-446d-8245-440d055f1cb6/Untitled.png)

    ### 설정

    - 고가용성 설정은 `yarn.resourcemanager.ha.enable` 설정 여부로 사용된다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a5640205-6ffd-4f65-8e2d-a87d73f38992/Untitled.png)

### 7. 타임라인 서비스 v.2

- `더보기`
    - 하둡 3버전에서 YARN은 **타임라인 서비스 v.2를 제공**한다.

    ### 특징

    - v.2는 v.1의 두 가지 주요 과제를 해결하기 위해 만들어졌다.

        ➡ **확장성 &** **유용성 향상**

    ### 확장성

    ```java
    [ v.1 ]
    - writer / reader 및 스토리지의 단일 인스턴스로 제한된다
    - 소규모 클러스터 이상으로 확장되지 않는다

    [ v.2]
    - 더 확장 가능한 분산 작성기 아키텍처를 사용한다
    - 확장 가능한 백엔드 저장소를 사용한다
    - 데이터 수집과 데이터 제공을 분리한다
    	➡ 기본적으로 각 YARN 애플리케이션에 대해 하나의 수집기인 분산 수집기를 사용한다.
    	➡ Reader는 REST API를 통해 쿼리를 제공하는 전용 인스턴스이다.
    - Apache HBase가 읽기 및 쓰기에 대한 응답 시간을 유지하면서 큰 크기로 잘 확장되기 때문에
    	➡ 'Apache HBase'를 기본 백업 스토리지로 선택한다!
    	
    ```

    ### 유용성 향상

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/68a60702-5656-4e4f-8834-c864670b2373/Untitled.png)

    - 타임 라인 서비스 v.2는 진행 흐름 개념을 명시적으로 지원한다.
    - 또한 흐름 수준에서 메트릭 집계를 지원한다.
    - 위 그림은 서로 다른 YARN 항목 모델링 흐름간의 관계를 보여준다.

    ### 구조

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/77271b82-7e19-4c03-9580-b2bdc6d5eb9b/Untitled.png)

    - 타임라인 서비스는 수집기(Writer) 세트를 사용하여 백엔드 스토리지에 데이터를 쓴다.

## # 5. 작업 지원 도구 (Hadoop Common)

---

### 1. DistCp

- `더보기`
    - 하둡은 클러스터내의 **대규모 데이터 이동**을 위한 `DistCp(Distribute Copy)` 기능을 제공한다.
    - 기본 파일 복사 명령인 `hadoop fs -cp`로는 파일을 **하나씩** 복사한다.
    - 하지만, DistCp 기능을 이용하면, 맵리듀스를 이용하여 대규모의 파일을 **병렬로 복사**할 수 있다.
    - 맵리듀스를 이용하여 작업을 처리하기 때문에 클러스터의 리소스를 이용하게 된다.
    - 또한, 대규모 병렬 처리 작업이기 때문에 네트워크 사용량을 잘 확인해야 한다!

        ```java
        📌 DistCP시 주의점!

        운영 상황에서 DistCp 작업시 네트워크 사용량을 꼭 확인하는 것이 좋고, 
        매퍼의 개수를 점진적으로 늘리는 것이 좋습니다. 

        또한 클러스터의 설정이 다르다면 복사 위치의 블록사이즈, 복제개수를 확인해야한다.
        ```

    ### 주요 옵션

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/057a11ef-788b-40fc-9382-d33a612b617a/Untitled.png)

    ### 사용법

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c000e7f4-519b-4eab-9808-e590f7d3ed1e/Untitled.png)

    **1) 매퍼 개수 설정**

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3d5ffa61-f80a-49ea-b0db-04a9b0364594/Untitled.png)

    **2) AWS S3간 데이터 이동**

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4509bb46-30b2-4091-91c9-65856df22c91/Untitled.png)

    **3) 일반 하둡과 커버로스 하둡간 데이터 이동**

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/274d58ef-496f-47b9-bc12-8578a84fc960/Untitled.png)

    **4) 커버로스 하둡간 데이터 이동**

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bac90fd4-bcf2-42d2-8763-ef3fd4157ed9/Untitled.png)

### 2. 하둡 아카이브

- `더보기`
    - 하둡 HDFS는 작은 사이즈의 파일이 많아지면 네임노드에서 이를 관리하는데 많은 문제가 발생한다.
    - 따라서 블록사이즈 크기로 파일을 유지해주는 것이 좋다.
    - 이를 위해, 하둡은 **파일을 묶어서 관리**하고 사용할 수 있는 **하둡 아카이브(Hadoop Archive)** 기능을 제공한다.

    ```java
    **📌 하둡 아카이브 기능**

    - 하둡 아카이브로 묶은 파일은 **har 스키마**를 이용해서 접근합니다. 
    - 하둡 아카이브는 맵리듀스 작업을 이용해서 파일을 생성합니다. 
    - 생성된 아카이브는 **ls 명령**으로는 디렉토리처럼 보입니다. 
    - **har 스키마**를 이용해서 파일을 확인할 수 있습니다. 
    - 맵리듀스 작업의 입력으로 전달하면 하둡에서 데이터를 읽어서 처리합니다.
    ```

    ### 생성

    - 하둡 아카이브 생성은 `archive` 커맨드를 이용한다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/54365696-3834-4b57-91c3-d4b4e947193b/Untitled.png)

    ### 해제

    - 하둡 아카이브 파일의 압축해제는 `distcp`를 이용한다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8d51d5b7-937d-4204-93c6-7a3ac5382485/Untitled.png)

## # 6. 오브젝트 저장소(Hadoop Ozone)

---

### 0. 오브젝트 저장소

- `더보기`
    - 오존(Ozone)은 하둡을 위한 확장성(scalable) 있는 **분산 객체 저장소**(distributed object store)이다.
    - 스파크, 하이브, YARN은 별도의 수정 없이 오존을 이용할 수 있다.
    - 오존은 자바 라이브러리와 CLI환경을 지원하며, 자바 라이브러리는 RPC와 REST 프로토콜을 지원한다.

    ### Ozone 구성요소

    - 오존은 `볼륨`, `버켓`, `키`로 구성됩니다.
    - 볼륨 ~ 사용자 계정
        - 관리자만 볼륨을 생성하거나 삭제할 수 있다.
    - 버켓 ~ 디렉토리
        - 여러 개의 키를 저장할 수 있지만, 다른 버켓은 저장할 수 없다.
    - 키 ~ 파일
        - 여러 개의 키를 저장할 수 있다.
    - `볼륨 > 버켓 > 키` 단위로 구성된다.

## # 7. 설정

---

### 0. 설정

- `더보기`
    - 하둡 관련 주요 설정은 `${HADOOP_HOME}/conf` 아래 설정한다.

    ```java
    core-site.xml
    : 공통으로 사용하는 주요 설정

    hdfs-site.xml
    : HDFS 관련 설정

    mapred-site.xml
    : 맵리듀스 작업 관련 설정

    yarn-site.xml
    : YARN 관련 설정

    hadoop-env.sh
    : 하둡을 이용하는데 필요한 환경변수 설정
    ```

### 1. 보안설정

- `더보기`
    - 암호화 설정시 주의할 점은 암호화에 따른 오버헤드로 인해서 읽기, 쓰기 성능이 떨어질 수 있다.

    ### 암호화 설정

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/422e259d-0e3c-48df-99d0-4cff8f7dcc83/Untitled.png)

    **1) dfs.encrypt.data.transfer**

    - HDFS 데이터 전송 채널 및 클라이언트가 **HDFS에 액세스할 수 있는 채널이 암호화되었는지**에 대한 여부를 나타낸다.
    - true면 채널이 암호화 되었음을 나타낸다. (default는 false)

    **2) dfs.encrypt.data.transfer.algorithm**

    - HDFS 데이터 전송 채널 및 클라이언트가 HDFS에 액세스할 수 있는 채널이 암호화되었는지에 대한 여부를 나타냅니다. (위에랑 똑같은,,?🤔)
    - 이 매개변수는 dfs.encrypt.data.transfer가 **true로 설정된 경우에만** 유효하다.
    - 기본값은 `3des`이며, 값을 `rc4`로 설정할 수도 있습니다

        ❗ 그러나, 보안 위험을 방지하려면 매개변수를 `rc4`로 설정하지 말자!

    **3) dfs.data.transfer.protection**

    - Hadoop에 있는 **각 모듈의 RPC 채널이 암호화되었는지** 여부를 나타낸다.

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e6a736b3-6069-463f-8793-6ac07801eba6/Untitled.png)

    - 개인 정보 채널은 기본적으로 암호화되어 있고, 인증 채널은 암호화되어 있지 않다.
