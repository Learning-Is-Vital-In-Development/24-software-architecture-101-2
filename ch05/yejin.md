# Chatper 05 아키텍처 특성 식별

----

아키텍트는 도메인 관심사, 요구사항, 암묵적 도메인 지식에서 아키텍처 특성을 밝혀낸다.

## 5.1 도메인 관심사에서 아키텍처 특성 도출
아키텍트는 도메인 관심사를 올바르게 해석하여 정확한 아키텍처 특성을 식별해야 한다. 도메인의 핵심 목표와 현재 상황을
고려하여 아키텍처 특성을 식별한 후, 그에 따른 합리적인 아키텍처 결정을 내려야 한다. 모든 아키텍처 특성을 수용하려는 제너릭 아키텍처는 
아키텍처를 복잡하게 만드는 안티패턴이다. 가급적 설계를 단순화하는게 좋다.
## 5.2 요구사항에서 아키텍처 특성 도출
요구사항 정의서에 명시된 문장에서 도출한 아키텍처 특성도 있다. 이 때는 도메인 지식이 중요하다.
## 5.3 사례 연구: 실리콘 샌드위치
실리콘 샌드위치는 전국 가맹점을 보유한 샌드위치 브랜드로, 온라인 주문 시스템을 구축하려 한다.
유저는 수천, 수백만명을 기대한다.
### 5.3.1 명시적 특성
요구사항 정의서에 기술되는 특성. 유저 수 등으로 동시 유저를 처리하는 능력 등을 도출해 낼 수 있어야 함.
수천에서 수백만명까지 예상하므로 확장성이 중요한 아키텍처 특성이라고 볼 수 있음.
### 5.3.2 암묵적 특성
요구사항 정의서에 따로 없는 특성. 가용성은 시스템에서 마땅히 지원되어야 할 암묵적인 아키텍처 특성.
보안, 신뢰성 등도 암묵적 특성에 해당.
