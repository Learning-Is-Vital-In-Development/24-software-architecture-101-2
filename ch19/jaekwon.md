## 19장. 아키텍처 결정

### 19.1 아키텍처 결정 안티패턴

* 네 패를 먼저 보여주지마
    * 원인
        * 아키텍트가 잘못된 선택을 하는 것을 두려워서 결정을 회피하거나 미루는 현상
    * 해결
        * 마지막으로 책임 질 수 있는 순간까지 기다리기 (?)
        * 개발팀과 지속적으로 협력하면서 문제가 생기면 빠르게 아키텍트 결정을 바꾸기
* 무한반복 회의
    * 원인
        * 아키텍트가 자신이 내린 결정을 정당화하는데 실패
    * 해결
        * 기술적인 명분도 중요하지만, 비즈니스적으로 정당화하는데 성공해야한다
        * 비즈니스 가치가 있는지 고민 (비용, 출시 시기, 유저 만적도, 전략적 포지셔닝)
* 이메일 기반 아키텍처
    * 원인
        * 아키텍처 결정을 잊어버리거나, 그러한 사실을 아예 모른다
    * 해결
        * 이메일 본문에 아키텍처 결정을 포함하지 않는다
        * 아키텍처 결정은 정말 관심있는 사람에게만 통지

### 19.2 아키텍처적으로 중요한

아키텍처적으로 중요한 architecturally significat 결정이란?
아래 5가지에 영향을 미치는 결정

* 구조
    * ex. bounded context에 따라서 애플리케이션의 구조가 바뀐다
* 비기능 특성
    * ex. 어떤 기술을 선택하냐에 따라 성능이 달라진다
* 의존성
    * ex. 서비스간의 커플링
* 인터페이스
    * ex. 게이트웨이, 통합 허브, 서비스 버스, API 프록시를 통해 서비스와 컴포넌트에 엑세스하고 조정하는 수단
* 구현 기술
    * 플랫폼, 프레임워크, 도구, 프로세스에 관한 결정

### 19.3 아키텍처 결정 레코드

Architecture Decision Record ADR

5가지 섹션으로 구성
* 제목
* 상태
* 컨텍스트
* 결정
* 결과

아스키독, 마크다운 등으로 작성
도구도 있음. ADR-tools

* 제목
    * 일련번호 + 제목
* 상태
    * 제안됨. 수락됨. 대체됨
* 컨텍스트
    * 불가항력적인 요소
    * ex. 어떤 사정 때문에 그렇게 결정할 수 밖에 없었나?
    * ex. '현재 주문의 총 결제 금액을 주문 서비스가 처리하려면 반드시 결제 서비스에 정보를 전달해야 하는데, 이는 REST 또는 비동기 메시징을 통해 수행할 수 있다'
* 결정
    * 결정하게된 사유
    * how 방법보다 why 이유
* 결과
    * 결과, 트레이드오프 등

RFC Request for Comments
ADR을 잘 사용하면 이후에 똑같은 논의나 고민을 하지 않게된다

비공식 섹션
* 컴플라이언스
    * 표준 섹션은 아니지만 추가하는게 좋음
    * 아키텍처 결정을 어떻게 측정/관리 할 수 있을지
        * ArchUnit, NetArchTest 등
* 노트
    * ADR에 대한 메타데이터

저장
문서화
표준화

### 예시

GGG 경매 시스템에서 BidCapture, BidStreamer, BidTracker 서비스 간의 메시지 발행/구독을 응용한 것
입찰 서비스 간에 펍/섭 메시지를 주고 받는다

* ADR 76. 입찰 서비스 간 펍/섭 메시징
    * 상태
        * 수락됨
    * 콘텍스트
        * BidCapture 서비스는 온라인 입찰자 또는 경매인을 통한 라이브 입찰자로부터 입찰을 받아 BidStreamer 서비스, BidTracker 서비스에 전달해야 한다. 입찰은 비동기 점대점(p2p), 비동기 펍/섭, 온라인 경매 API를 통한 REST 방식으로 전달할 수 있다
    * 결정
        * BidCapture, BidStreamer, BidTracker 세 서비스는 비동기 펍/섭 메시지를 주고받기로 결정할 예정이다. BidStreamer 서비스는 BidCapture 서비스가 받은 순서 그대로 입찰을 수신해야 한다. 메시징과 큐는 스트림의 입찰 순서를 자동으로 보장한다. 비동기 펍/섭 메시징을 사용하면 입찰 프로세스의 성능이 개선되고 입찰 정보를 확장할 수 있다
    * 결과
        * 메시징 큐의 고가용성이 필요하고, 클러스터 구성을 해야 할 것이다
        * 내부 입찰 이벤트는 API 레이어에서 수행되는 보안 체크를 우회할 것이다
        * 업데이트: 2020년 4월 14일 ARB 회의에서 검토한 결과, ARB는 위와 같은 트레이드오프를 수용하기로 판단했으며, 세 서비스 간 입찰 이벤트는 추가적으로 보안 체크는 필요하지 않다고 결론지었다
    * 컴플라이언스
        * 주기적으로 수동 코드 검사 및 설계 리뷰를 실시하여 BidCapture, BidStreamer, BidTracker 세 서비스 간에 비동기 펍/섭 메시징이 제대로 이뤄지고 있는지 확인할 예정이다


