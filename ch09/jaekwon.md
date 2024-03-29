## 9장. 기초

아키텍처 스타일

* Big Ball of Mud
    * 아키텍처 구조가 없음
* 유니터리 아키텍처
    * 한대에 모든 소프트웨어가 구동
    * 현재는 임베디드 시스템 정도에서 쓰임
* 클라이언트/서버. 2-tier
    * 데스크톱 + 데이터베이스 서버
    * 브라우저 + 웹 서버
    * 3 tier
        * 프론트 + 애플리케이션 + 데이터베이스
        * CORBA
        * DCOM

### 9.2 모놀리식 대 분산 아키텍처

* 모놀리식
    * 레이어드
    * 파이프라인
    * 마이크로커널
* 분산형
    * 서비스 기반
    * 이벤트 기반
    * 공간 기반
    * 서비스 지향
    * 마이크로서비스

분산 아키텍처의 트레이드오프. 장애물들

* 오류#1: 네트워크는 믿을 수 있다
    * 네트워크는 믿을 수 없다. 타임아웃, circuit breaker 같은 장치가 필요
* 오류#2: 레이턴시는 0이다
* 오류#3: 대역폭은 무한하다
* 오류#4: 네트워크는 안전하다
    * 보안이 필요하다
* 오류#5: 토폴로지는 절대 안 바뀐다
* 오류#6: 관리자는 1명이다
* 오류#7: 운송비는 0이다
    * 레이턴시가 아니라 진짜 비용
    * 분산 아키텍처는 하드웨어, 서버, 게이트웨이, 방화벽, 신규 서브넷, 프록시 등이 더 필요
* 오류#8: 네트워크는 균일하다
    * 네트워크 장비들은 제조사가 제각각이다. 어떤 일이 일어날지 모른다


분산 아키텍처에서 고려해야할 사항

* 분산 로깅
* 분산 트랜잭션
    * 분산 아키텍처는 확장성, 성능, 가용성을 얻는 대가로 데이터 일관성, 무결성을 확보 할 수 없다. 그래서 최종적 일관성을 주로 보장한다
    * 분산 트랜잭션을 달성하기 위해서는..
        * transactional saga
        * base 트랜잭션
* 계약 관리 및 버저닝

> '계약'이라는 단어가 자주 나오는데..contract인데 API 인터페이스를 말하는건가? 바로 와닿지는 않는다

------------------------------

> "20230401_MSA와 트랜잭션.md" 일부

## 요약

* 글로벌 트랜잭션
    - 2PC
    - 분산락을 위한 코디네이터
    - > Saga Pattern (로컬 트랜잭션의 시퀀스, 이벤트 기반)
        + 분산 트랜잭션 없이도 여러 서비스에서 데이터 일관성을 유지하는게 가능
        + 프로그래밍 모델이 더 복잡
        + 보상 트랜잭션 필요
* 네트워크 요청의 예외상황
    - 같은 요청이 두번 들어옴
    - 요청의 순서가 뒤집힘 (취소 -> 결제)
    - 타임아웃 (성공했는지 실패했는지 모름)
    - 각종 인프라 문제...
* 클라이언트 입장에서 설계해야하는 것 -> 재시도, 무효화 API, 보상 트랜잭션
    - 무한 재시도는 방법이 될 수 없다. 적당한 선에서 끊고 운영대응 할 수 밖에 없다
* > 서버 입장에서 설계해야하는 것 -> 멱등성 API
    - 멱등성 키, 트랜잭션 키
    - UUID v4, 유효기간 존재, 동일을 비교할 scope

------------------------------
