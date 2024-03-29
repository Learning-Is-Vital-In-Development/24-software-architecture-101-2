# 챕터 5. 아키텍처 특성 식별

아키텍처를 구축하거나 기존 아키텍처의 타당성을 검증할 때 제일 먼저 해야 할 일은 아키텍처 특성을 식별하는 것이다. 주어진 문제 영역이나 애플리케이션에서 아키텍처 특성을 정확하게 식별하기 위해 아키텍트는 해당 도메인을 잘 이해하고 있어야 하며, 도메인 이해관계자들과 협력하여 도메인 관점에서 정말 중요한 것들을 결정해야 한다. **아키텍트는 적어도 도메인 관심사, 요구사항, 암묵적 도메인 지식, 이렇게 세 가지 출처에서 아키텍처 특성을 밝혀낸다.**

> (내 생각) 향후 추후 유입이 많을 것이라 예상되는 신규 서비스 개발을 한다면 아키텍트로서 확장성, 성능, 내고장성 등을 가장 중요한 특성으로 뽑아볼 수 있을 것 같다. 혹은 트랜잭션의 정합성을 완벽하게 보장해야 하는 결제 등의 도메인에서는 보안, 내고장성, 성능을 뽑아볼 수 있을 것 같다.
> 

## **5.1 도메인 관심사에서 아키텍처 특성 도출**

아키텍트는 도메인의 핵심 목표와 현재 상황을 고려하여 **도메인 관심사를 '~성'으로 해석한 후, 그에 따라 정확하고 합리적인 아키텍처 결정을 내려야 한다**. 

지원 가능한 아키텍처 특성 하나하나가 전체 시스템 설계를 복잡하게 만드는 요인이기 때문에 너무 많은 아키텍처 특성을 수용하면 아키텍트, 개발자가 당초 의도했던 문제 영역의 해결을 시도하기도 전엔 아키텍처가 너무 복잡해져 버린다. `아키텍처 특성의 개수에 연연하지 말고 가급적 설계를 단순화하는게 좋다`.

대부분의 아키텍처 특성은 핵심 도메인 이해관계자들의 의견을 듣고 도메인 관점에서 무엇이 중요한지 의견을 교환하면서 정리된다. 이런 과정이 언뜻 보기에는 대수롭지 않게 느껴지지만, 아키텍트와 도메인 이해관계자들이 서로 다른 언어로 말을 한다는 게 문제이다. **아키텍트는 확장성, 상호운용성, 내고장성, 학습성, 가용성을 운운하는데, 도메인 이해관계자는 인수 병합, 고객 만족, 출시 시점, 경쟁 우위를 논하는 식이다**. 

> 도메인에 대한 이해를 얻기 위해, 도메인 전문가와의 대화/질문은 필수적이다. 아키텍트로서 도메인 전문가가 이야기하는 도메인 관심사를 아키텍처 특성(비기능적 요구사항)으로 치환할 수 있는 능력이 필요하다.
> 

그래서 도메인 관심사를 아키텍처 특성으로 옮겨야 하는데, 아래 표는 일반적인 도메인 관심사와 이를 뒷받침하는 아키텍처 특성을 정리한 표이다.

| 도메인 관심사 | 아키텍처 특성 |
| --- | --- |
| 인수 합병 | 상호운용성, 확장성, 적응성, 신장성 |
| 출시 시기 | 민첩성, 시험성, 배포성 |
| 유저 만족 | 성능, 가용성, 내고장성, 시험성, 배포성, 민첩성, 보안 |
| 경쟁 우위 | 민첩성, 시험성, 배포성, 확장성, 가용성, 내고장성 |
| 시간 및 예상 | 단순성, 실행성 |

이 표에서 **민첩성이 곧 출시 시기를 의미하는 것이 아니라는 것을 눈 여겨 보자**. 외려 출시 시기는 민첩성 + 시험성 + 배포성이다. 이것이 도메인 관심사를 해석하는 아키텍트가 곧잘 빠지는 함정이다. 어느 한 가지 성분에만 집착하면 밀가루도 안 넣고 케이크를 반죽하게 될지도 모른다. 

## **5.2 요구사항에서 아키텍처 특성 도출**

요구사항 정의서에 명시된 문장에서 도출한 아키텍처 특성도 있다. **예를 들어, 예상 유저수와 그에 따른 확장 문제는 보통 도메인 관심사에서 빠지지 않는 단골 손님이다**. 아키텍트가 알고 있는 도메인 지식에서 도출되는 특성들도 있는데, 이것이 아키텍트가 도메인 지식을 갖고 있으면 이로운 이유이다. 

예를 들어, 아키텍트가 대학교 학사 관리 시스템에서 수강 등록을 처리하는 애플리케이션을 설계한다고 해보자. 보통 대학생들의 성향과 습관에 대한 지식을 바탕으로 **마감 시간 전에 대다수의 학생이 신청을 하는 것을 알면, 그에 맞는 시스템 설계를 할 수 있다**. 이런 시시콜콜한 내용까지 요구사항 정의서에 적혀 있지는 않지만 아키텍트는 어떤 식으로든 이런 것까지 설계 결정에 반영해야 한다. 

> 아키텍처 카타(architectural katas)의 유래
> 
> - **설명, description :** 시스템으로 해결하려는 전체 도메인 문제.
> - **유저, users :** 시스템을 사용할 것으로 예상되는 유저의 유형과 인원수.
> - **요구사항, requirements :** 아키텍트가 도메인 유저 및 도메인 전문가의 말을 듣고 예상하는 도메인/도메인 수준의 요구사항.
> - **부가 컨텍스트, additional context :** 요구사항만에는 명확하게 기술되어 있지 않지만 아키텍트가 암묵적인 문제 영역에 관한 지식을 이용해 반드시 고려해야 할 여러 가지 항목들.

> (내 생각) `장차 아키텍트가 될 사람들에게 열심히 카타 연습을 하라고 권장합니다.` 이와 관련해서 ****가상 면접 사례로 배우는 대규모 시스템 설계 기초 1,2**** 권이 도움이 많이 되는 것 같다. 처리율 제한기, NoSQL, URL 단축기 등 각각 다른 아키텍처 특성을 만족해야 하는 여러 서비스들을 가상으로 설계해보는 연습을 진행한다. 이와 관련해서 요구사항을 정해두고, 발표하는 형태로 스터디를 진행해봐도 좋겠다.
> 

## **5.3 사례 연구: 실리콘 샌드위치**

실리콘 샌드위치 카타를 예로 들어 아키텍트가 요구사항을 바탕으로 아키텍처 특성을 도출하는 방법을 알아보자. 

### **설명**

- 실리콘 샌드위치는 전국 가맹점을 보유한 샌드위치 브랜드로, 온라인 주문 시스템을 구축하려고 한다.

### **유저**

- 앞으로 수천 명, 어쩌면 수백만 명에 달할지도 모른다.

### **요구사항**

- 유저가 주문을 하면 샌드위치를 픽업하러 갈 시간 및 상점까지 가는 경로를 전달받는다.
- 배달 서비스가 가능한 지점은 기사를 보내 샌드위치를 유저에게 배달한다.
- 모바일 기기에서도 이용가능하다.
- 전국 어느지점이나 이용 가능한 프로모션/스폐셜 행사를 제공한다.
- 특정 지점에서만 이용 가능한 로컬 프로모션/스폐셜 행사를 제공한다.
- 온라인 결제, 대면 결제, 배송 시 결제 등 다양한 결제 옵션을 제공한다.
- 샌드위치 지점은 가맹점이므로 소유자가 다 다르다.
- 모회사는 머지않아 해외 시장도 진출할 계획이다.
- 이 회사는 값싼 인력을 고용해 이윤을 최대화하는 것이 목표다.

### **부가 콘텍스트**

- 샌드위치 지점은 가맹점이므로 소유자가 다 다르다.
- 모회사는 머지않아 해외 시장도 진출할 계획이다.
- 이 회사는 값싼 인력을 고용해 이윤을 최대화하는 것이 목표다.

**아키텍트의 임무는 엄청난 노력을 기울여 도메인 요구사항을 충족하는 전체 시스템을 코드 레벨로 설계하는 것이 아니라, 설계, 특히 구조와 관련이 있거나 영향을 미치는 것들을 찾아내는 것이다.** **`우선, 아키텍처 특성이 될만한 것들을 명시적인 것과 암묵적인 것으로 분류한다`.**

## **5.3.1 명시적 특성**

**명시적 아키텍처 특성은 필요한 설계의 일부로서 요구사항 정의서에 기술된다.** 예를 들어, 쇼핑몰 사이트는 지원해야 할 최대 동시 접속자 수를 특정하여 도메인 분석자가 요구사항으로 명시한다. 아키텍트는 요구사항의 각 항목이 어떤 아키텍처 특성과 관련되어 있는지 검토해야 하는데, 그 전에 카타 유저 섹션에 기술된 것처럼 도메인 레벨에서의 기대치를 먼저 고려해야 한다. 

아키텍트가 가장 먼저 눈 여겨 봐야 할 부분은 유저 수이다. 지금은 수천 명 정도이지만 향후 수백만 명에 이를 수 있다고 한다. 따라서, **확장성**, 즉 유의미한 성능 저하 없이 다수의 동시 유저를 처리하는 능력이 무엇보다 중요한 아키텍처 특성으로 보인다. 순간적으로 폭증한 유저 요청을 처리하려면 탄력성도 필요하다. 확장성과 탄력성, 이 두 특성은 한몸처럼 나타날 때도 많지만 제약조건은 각각 다르다. 

확장성은 좋지만 탄력적이지 않은 시스템도 있다. 호텔 예약 시스템이 그렇다. 특별 할인 행사가 없어도 투수객 수는 대체로 일정하다. 하지만 콘서트 티켓 예약 시스템은 다르다. 티켓 발매가 시작되면 팬들이 왕창 몰려들 텐데, 탄력성이 크지 않으면 시스템이 버텨낼 재간이 없다. 

탄력성이 필수라는 문구는 실리콘 샌드위치 요구사항 정의서 어디에도 없지만, 아키텍트는 탄력성을 중요한 고려 사항으로 식별해야 한다. 이처럼 아키텍처 특성을 직접 가리키진 않지만 문제 영역 안에 요구사항이 숨어 있는 경우가 많다. 

| 요구사항 | 분석 |
| --- | --- |
| 유저가 주문을 하면 샌드위치를 픽업하러 갈 시간 및 상점까지 가는 경로를 안내받는다. | 외부 지도 서비스는 곧 연계 지점에 해당하므로 신뢰성과 같은 특성에 영향을 미칠 수 있다. 서드파티 시스템에 의존하는 시스템은 서드파티 시스템 호출에 실패할 경우 전체 시스템 안정성이 타격을 입는다. 그러나 아키텍트는 과도하게 아키텍처 특성을 설정하지 않도록 유의할 필요도 있다.외부 지도 서비스가 다운되면 어떻게 될까?실리콘 샌드위치 사이트 전체가 멎을까, 아니면 교통 정보가 빠진 조금 덜 쓸모 있는 서비스가 제공될까? 아키텍트는 불필요한 불안정성 또는 취약점이 설계에 반영되지 않도록 항상 조심해야 한다. |
| 배달 서비스가 가능한 지점은 기사를 보내 샌드위치를 유저에게 배달한다. | 별다른 아키텍처 특성이 필요하지 않은 것 같다. |
| 모바일 기기에서도 이용가능하다. | 예산은 한정되어 있고 프로그램은 단순해야하므로 다수의 애플리케이션 제작은 무리라고 판단해서 모바일에 최적화된 웹 애플리케이션 개발로 방침을 정하고 페이지 로딩 시간 또는 기타 모바일 환경에서 민감한 성능 아키텍처 특성을 정의할 수 있다. |
| 전국 어느지점이나 이용 가능한 프로모션/스폐셜 행사를 제공한다. |  |
| 특정 지점에서만 이용 가능한 로컬 프로모션/스폐셜 행사를 제공한다. | 4,5번은 프로모션과 스폐셜 행사에 관한 커스터마이징 가능 여부에 관한 요구 사항이다.1번 요구사항은 유저 주소에 맞는 교통 정보의 커스타마이징을 의미하므로세 요구사항을 토대로 맞춤성(customizability)을 아키텍처 특성으로 도출할 수 있다. |
| 온라인 결제, 대면 결제, 배송 시 결제 등 다양한 결제 옵션을 제공한다. | 온라인 결제는 봉안이 필요하지만, 이 요구사항만 봐서는 어느 정도의 보안이 필요한지 잘 모른다. |
| 샌드위치 지점은 가맹점이므로 소유자가 다 다르다. | 이 요구사항은 아키텍처 비용에 제약을 가한다. 아키텍트는 단순하거나 기능이 조금 빠진 아키텍처로도 충분한지 타당성(feasibility)을 미리 확인해야 한다. |
| 모회사는 머지않아 해외 시장도 진출할 계획이다. | 해외 시장 진출 계획이 있다면 국제화, 즉 i18n이 필요하다.이 요구사항을 처리하는 설계 기법은 워낙 다양하고 특별한 구조를 요하는 것도 아니지만 어떤 식으로든 설계 결정에 영향을 미칠 것이다. |
| 이 회사는 값싼 인력을 고용해 이윤을 최대화하는 것이 목표다. | 이 요구사항은 사용성이 중요하지만 아키텍처 특성보다는 설계와 더 깊은 연관이 있다. |

1번 요구사항은 유저 주소에 맞는 교통 정보의 커스타마이징을 의미하므로 4,5번 요구사항과 묶어서 맞춤성(customizability)을 아키텍처 특성으로 도출할 수 있다.

요구사항 정의서에서 도출한 세 번째 아키텍처 특성은 바로 성능이다. 성능이 떨어져 주문이 몰리는 시간에 느려지는 샌드위치 사이트에서 주문하고 싶은 사람은 없을 것이다.

확장성 수와 성능 수도 함께 정의하는게 좋다. 즉, 성능의 기준선을 먼저 그어 놓고 특정 수의 유저가 접속할 때 성능은 어느 정도가 나와야 수용 가능한지 정해야 한다.

## **5.3.2 암묵적 특성**

요구사항 정의서에 따로 없는 아키텍처 특성도 있지만 이들은 각각 중요한 설계 요소가 된다. 가용성(availability)은 시스템에서 마땅히 지원되어야 할 암묵적인 아키텍처 특성으로 유저는 샌드위치 사이트에 접속할 수 있어야 한다. 유저가 사이트에 방문해서 문제없이 사용하려면 신뢰성(reliability) 역시 필요하다.

**보안은 모든 시스템에 공통적인 암묵적 특성이다**. 안전하지 않은 소프트웨어를 원하는 사람은 아무도 없다.

> [설계 대 아키텍처, 그리고 트레이드 오프] 실리콘 샌드위치 카타에서 맞춤성을 시스템의 일부로 식별하는 아키텍트도 있겠지만, 문제는 이 것이 아키텍처냐 설계냐, 하는 것이다. 아키텍처는 구조적 컴포넌트를 나타내지만, 설계는 아키텍처 내부에 속한다. 실리콘 샌드위치의 맞춤성은 마이크로커널 같은 아키텍처 스타일을 선택해서 구조적으로 지원할 수 있지만, 만약 상충되는 다른 관심사 때문에 아키텍트가 다른 스타일을 선택한다면, 개발자는 템플릿 메서드 같은 설계 패턴으로 커스텀 로직을 직접 구현해서 자식 클래스에서 오버라이드 가능한 워크플로를 부모 클래스에 정의하는 식으로 개발할 것이다. 어떤 설계가 나을까?
> 
> 
> 모든 아키텍처가 그렇듯 이 질문의 대답도 수많은 팩터마다 달라진다. 첫째, 마이크로커널 아키텍처로 구현하지 않을 타당한 사유가 있는가? 둘째, 이렇게 설계하면 다르게 설계할 때보다 특별히 더 구현하기 어려운 아키텍처 특성이 있나? 셋째, 각 '설계 대 패턴'에서 모든 아키텍처 특성을 지원하려면 비용이 얼마나 들어가나? 이와 같은 아키텍처 트레이드 오프 분석은 아키텍트가 하는 중요한 업무의 일부이다.
> 

아키텍트는 아키텍처 특성에 우선순위를 매겨 가장 단순한 필수 세트로 정리해야 한다. 암묵적 특성은 대부분 일반적인 성공을 지원하기 때문에 아키텍트는 일반적으로 명시적 아키텍처 특성을 추려낼 가능성이 높다. 어떤 아키텍처 특성이 애플리케이션에 정말 필요한지 의문이라면 시스템이 성공하기 위해서 무엇이 필요한지, 무엇이 중요한지 먼저 생각해 보자.

실리콘 샌드위치의 아키텍처 특성 중 가장 덜 중요한 것은 무엇일까? 물론, 정답은 없겠지만 **그래도 맞춤성과 성능 둘 중 하나는 포기해야 할 것이다**. 운영 아키텍처 특성 중에서 성공에 가장 덜 중요한 특성은 성능으로 보인다. 그렇다고 개발자가 성능이 떨어지는 애플리케이션을 개발해도 된다는 건 아니다. **확장성이나 가용성 같은 다른 특성들보다 성능을 우선시하지 않는 애플리케이션을 구축한다는 의미이다**.