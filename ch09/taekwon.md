# CHAPTER 9

# Overview

아키텍처 스타일은 종종 아키텍처 패턴이라고도 부르며, 다양한 아키텍처 특성을 다루는 컴포넌트의 명명된 관계를 기술한다.

아키텍처 스타일은 각 명칭마다 설계 패턴의 존재 이유이기도한, 많은 세부 내용이 함축되어 있다. 또 아키텍처 스타일은 토폴로지와 기본 전제된 아키텍처 특성을, 이로웃것과 해로운 것 모두 기술한다.

# 기초 패턴

무 하나 뚜렷한 아키텍처 구조가 전무한 상태를 진흙잡탕 이라고 표현한다. 요즘에는 보통 실제 내부 구조라 할 만한 것은 하나도 없는, 데이터베이스를 직접 호출하는 이벤트 핸들러를 가진 단순한 스크립팅 애플리케이션을 가리킨다.

안타깝게도 이러한 아키텍처 안티패턴은 실제로 아주 흔히 발생한다. 많은 프로젝트가 코드 품질 및 구조에 관한 거버넌스가 결여된 탓에 본의 아니게 그렇게 된다.

## 유니터리 아키텍처

유니터리(단일, 통일) 시스템은 임베디드 시스템과 그 밖에 매우 제약이 많은 극소수 환경을 제외하면 거의 쓰이지 않는다. 소프트웨어 시스템은 시간이 지날수록 점점 기능이 늘어나기 마련이므로 성능, 확장 등의 운영 아키텍처 특성을 유지하려면 관심사를 분리할 필요가 있다.

### 데스크톱 + 데이터베이스 서버

초창기 PC 아키텍처는 윈도우 같은 UI를 기반으로 리치 데스크톱 애플리케이션을 개발하도록 적극 지원했으며, 이는 표준 네트워크 프로토콜을 통해 접속 가능한 스탠드얼론(돌립형) 데이터베이스와 잘 맞았다.

덕분에 프레젠테이션 로직은 데스크톱에 두고 계산이 많은 액션은 사양이 좋은 데이터베이스 서버에서 실행했다.

### 브라우저 + 웹 서버

현대 웹 개발 시대가 도래하면서 웹 브라우저가 웹 서버에 접속하는 형태로 분리되는 것이 일반화 됐다.

이로써 클라이언트는 데스크톱보다 훨씬 가벼운 브라우저로 대체되었고 내외부 방화벽 모두 더 넓은 범위로 배포가 가능해 졌다.

데이터베이스는 웹 서버와 분리되어 있지만 두 서버 모두 운영 센터 내부의 동급 머신에서 운영되고 유저 인터페이스는 브라우저에서 실행되므로 여전히 이 구조를 2티어 아키텍처로 바라보는 아키텍트도 많다.

### 3티어

3티어 아키텍처는 더 많은 레이어로 분리한다. 분산 아키텍처에 적합한 공통 객체 요청 브로커 아키텍처, 분산 컴포넌트 객체 모델 같은 네트워크 수준의 프로토콜과 잘 맞는다.

## 모놀리식 대 분산 아키텍처

아키텍처 스타일은 크게 모놀리식(전체 코드를 단일 단위로 배포) 분산형(원격 액세스 프로토콜을 통해 여러 단위로 배포하는) 두 종류이다.

분산 아키텍처 스타일은 모놀리식 아키텍처 스타일에 비해 성능, 확장성, 가용성 측면에서 훨씬 강력하지만, 이런 파워에도 결코 무시할 수 없는 트레이드오프가 수반된다.

### 네트워크는 믿을 수 있다.

네트워크의 신뢰도는 점점 좋아지고 있긴 하나 아직도 미덥지 못한게 사실이다. 분산 아키텍처는 그 특성상 서비스를 오가는, 또 서비스 간에 이동하는 네트워크에 의존하므로 이것은 아주 중요한 문제이다.

그래서 타임아웃 같은 장치를 마련하거나 서비스 사이에 회로 차단기(circuit breaker)를 두는 것이다. 시스템이 네트워크에 더 의존할수록 시스템의 신뢰도는 잠재적으로 떨어질 가능성이 있다.

<aside>
💡 Fault Tolerance
단일 마이크로서비스의 장애가 전체시스템에 영향을 미치지 않도록 장애가 발생해도 견딜 수 있는 `내결함성` 을 의미하며 그 방법중 하나가 circuit breaker가 있다.

</aside>

### 레이턴시는 0이다.

메서드나 함수를 이용해 다른 컴포넌트를 호출하면 그 소요 시간은 대개 나노 초 내지 밀리초 단위로 측정되지만, 동일한 호출을 (Rest, Message, RPC) 원격 액세스 프로토콜을 통해서 수행하면 서비스 액세스 시간이 밀리초 단위로 측정된다.

따라서 모든 부산 아키텍처에서 레이턴시는 0이 아니다. 어떤 분산 아키텍처를 구축하든지 간에 평군 레이턴시는 반드시 알아야 하며, 이것이 분산 아키텍처가 실현 가능한지 판단하는 유일한 방법이다.

특히, 마이크로서비스는 서비스가 잘게 나뉘기 떄문에 서비스 간 통신량도 만만치 않고 보통 이런 긴 꼬리 레이턴시가 분산 아키텍처의 성능을 저해하는 주범이 된다.

### 대역폭은 무한하다.

모놀리식 아키텍처는 비즈니스 요청을 처리하는 데 그리 큰 대역폭이 필요하지 않으므로 대역폭이 문제가 될 일은 별로 없다. 하지만 분산 아키텍처의 경우 시스템이 자잘한 배포 단위로 쪼개지면서 이 서비스들 간에 주고받는 통신이 대역폭을 상당히 점유하여 네트워크가 느려지고, 결국 레이턴시와 신뢰성에도 영향을 미친다.

필요한 데이터 외의 불필요한 데이터까지 받는 형태의 커플링을 스탬프 커플링이라고 하는데, 이는 분산 아키텍처에서 상당히 많은 대역폭을 차지한다.

[스탬프 커플링 해결방법]

- 프라이빗 REST API 엔드포인트를 둔다.
- 계약에 필드 셀렉터를 사용한다.
- GraphQL로 계약을 분리한다.
- 컨슈머 주도 계약(CDC)와 값 주도 계약(VBC)를 병용한다.
- 내부 메시징 엔드포인트를 사용한다.

어떤 기법을 적용하든, 분산 아키텍처의 서비스 또는 시스템 감에 최소한의 데이터만 주고받도록 하는 것이 이 오류를 바로잡는 최선의 길이다.

### 네트워크는안전하다.

아키텍트와 개발자는 대부분 가상사설망(VPN), 신뢰할 수 있는 네트워크, 방화벽에 너무 익숙해진 나머지, 네트워크가 간전하지 않다는 사실을 망각한다. 보안은 분산 아키텍처에서 훨씬 더 어려운 문제이다.

모놀리식에서 분산 아키텍처로 옮아가면서 더 넓은 영역이 악의적인 외부인의 위협과 공격에 노출된다. 따라서 이러한 요청이 서비스로 유입되지 않게 철저한 보안 대책을 강구해야 한다.

### 토폴로지는 절대 안 바뀐다.

네트워크를 구성하느 모든 라우터, 허브, 스위치, 방화벽, 네트워크, 어플라이언스 등 전체 네트워크 토폴로지가 불변일 거란 가정은 섣부른 오해이다.

이키텍트는 운영자, 네트워크 관리자와 항시 소통을 하면서 무엇이, 언제 변경되는지 알고 있어야 한다.

### 관리자는 한 사람 뿐이다.

대기업에서 일하는 네트워크 관리자는 보통 수십 명에이른다. 분산 아키텍처는 이러한 소통에 복잡할 수밖에 없고 모든 것을 정상 궤도에 올려놓으려면 상당히 많은 조율 과정이 불가피하다.

### 운송비는 0 이다.

레이턴시 오류와는 다르다. 여기서 운송비는 레이턴시가 아닌 단순한 REST 호출을 하는 데 소요되는 진짜 비용을 말한다.

분산 아키텍처는 하드웨어, 서버, 게이트웨이, 방화벽, 신규 서브넷, 프록시 등 리소스가 더 많이 동원되므로 모놀리식 아키텍처보다 비용이 훨씬 더 든다.

### 네트워크는 균일하다.

실제로 많은 회사의 인프라는 여러 업체의 네트워크 하드웨어 제품들이 얽히고 설켜 있다. 요는, 온갖 종류의 하드웨어가 서로 다 잘  맞물려 동작하는 건 아니라는 것이다.

### 다른 분산 아키텍처 고려 사항

분산 로깅

분산 아키텍처는애플리케이션과 시스템 로그가 분산되어 있으므로 어떤 데이터가 누락된 근본 원인을 밝혀내기가 대단히 어렵고 시간도 많이 걸린다.

스플렁크 같은 로깅 통합 도구를 사용하면 다양한 소스와 시스템에서 통합된 로그 및 콘솔로 데이터를 취합할 수 있지만 복잡한 분산 로그를 확인하기에는 역부족이다.

<aside>
💡 PInPoint 와 Global Trace Id(분산 시스템에서의 공통된 Trace Id로 로그추적)

</aside>

분산 트랜잭션

퍼시스턴스 프레임워크가 대신 실행하는 포준 커밋/롤백 기능은 ACID 트랜잭션을 통해 업데이트 시 데이터 일관성과 무결성을 강제한다.

분산 아키텍처는 사정이 좀 다르다. `최종 일관성`(언젠가는 맞춰진다.) 이라는 개념을 바탕으로 별도로 분리된 배포 단위에서 처리된 데이터를 미리 알 수 없는 어느 시점에 모두 일관된 상태로 동기화 한다. 이는 확장성, 성능, 가용성을 얻는 대가로, 데이터 일관성과 무결성을 희생하는 트레이드 오프인 셈이다.

<aside>
💡 CAP 이론
CAP 를 만족하는 시스템은 없다.

**일관성(Consistency)
가용성(Availability)
파티션 허용(Partition tolerance)**

</aside>

분산 트랜잭션을 관리하는 한 가지 방법으로 트랜잭셔널 사가가 있다. 사가는 보상을 위해 이벤트를 소싱하거나 트랜잭션 상태를 관리하기 위해 유한 상태 기계를 활용한다.

<aside>
💡 보상 트랜잭션
다른 이유(네트워크 오류 등)로 결제 실패시 실패한 결제에 대한 취소 트랜잭션을 요청 하는 방식

메시징 등을 활용하더라도 `메시징의 전달 보장` 측면도 고려해야한다.(아웃박스 패턴 등)
</aside>

<aside>
**사가 패턴의 두 가지 종류**
**Choreography based SAGA pattern**

![image](https://github.com/akfls221/24-software-architecture-101-2/assets/71249347/c9d46af2-7ad3-49d8-a098-1aadb8009106)

**Orchestration based SAGA pattern**

![image](https://github.com/akfls221/24-software-architecture-101-2/assets/71249347/8c37d138-f5b4-442e-ae76-7e260ae95d2d)

</aside>

사가 외에 BASE 트랜잭션도 사용하며 BASE 는 (BA)sic availability(기본 가용성), (S)oft state(소프트 상태),

(E)ventual consistency(최종적 일관성)의 약자이다. 이는 어떤 소프트웨어 조각이 아닌 일종의 기법이다.

시스템 또는 서비스의 기본 가용성에 따라 시스템은 아키텍처 패턴과 메시징을 사용하여 결국 언젠가 일관되게 맞춰질 것이다.
