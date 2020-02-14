---
title: Azure ML 및 분석을 통한 소비자 브랜드의 SKU 최적화
author: scseely
ms.author: scseely
ms.date: 11/20/2019
ms.topic: article
ms.service: industry
description: 소매 산업 분류 최적화. AI 및 ML의 인사이트를 통한 SKU 최적화.
ms.openlocfilehash: 22411776e830bb3c71f8c1277b30ec4331a3ef17
ms.sourcegitcommit: 3b175d73a82160c4cacec1ce00c6d804a93c765d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77054374"
---
# <a name="sku-optimization-for-consumer-brands-solution-guide"></a>소비자 브랜드에 대한 SKU 최적화 솔루션 가이드

## <a name="introduction"></a>소개

소매점 및 소비자 브랜드는 소비자가 마켓플레이스 내에서 구매하려는 올바른 제품 및 서비스를 보유하는 데 중점을 두고 있습니다. 또한 매출 극대화를 고려할 때, 제품(또는 제품 조합)이 쇼핑 경험의 주요 부분임은 명백합니다. 소비자 브랜드에서 제품의 가용성(재고)은 지속적인 관심 영역입니다.

SKU 분류라고도 하는 제품 재고는 공급 및 물류 가치 체인에 걸쳐 있는 복잡한 주제입니다. 이 문서에서는 SKU 분류를 최적화하여 소비자 상품 관점에서 매출을 극대화하는 문제에 특히 중점을 둡니다. SKU 분류 최적화 문제는 다음 질문에 응답하는 알고리즘을 개발하여 해결할 수 있습니다.

- 지정된 시장 또는 상점에서 가장 성과가 높은 SKU는 무엇인가요? 
- 해당 성과를 기준으로 지정된 시장 또는 상점에 할당해야 하는 SKU는 무엇인가요?
- 성과가 낮아서 성과가 더 높은 SKU로 대체해야 하는 SKU는 무엇인가요?
- 소비자 및 시장 세그먼트에 대해 파생할 수 있는 다른 인사이트는 무엇인가요?

## <a name="automate-decision-making"></a>의사 결정 자동화

일반적으로 소비자 브랜드는 SKU 포트폴리오의 SKU 수를 늘리는 방법으로 소비자 수요에 접근했습니다. SKU 수가 급증하고 경쟁이 커지면서 매출의 90%가 포트폴리오에 있는 제품 SKU의 겨우 10%에서 발생하는 것으로 추정됩니다. 일반적으로 매출의 80%가 SKU의 20%에서 발생합니다. 그리고 이 비율이 수익성을 향상하기 위한 후보입니다. 

기존의 정적 보고 방법은 기록 데이터를 활용하므로 인사이트가 제한됩니다. 잘해야 의사 결정이 여전히 수동으로 수행되고 구현됩니다. 이는 사용자 작업 및 처리 시간을 의미합니다. AI(인공 지능) 및 클라우드 컴퓨팅의 최신 기술을 활용하면 고급 분석을 사용하여 다양한 선택 사항과 예측을 제공할 수 있습니다. 이러한 종류의 자동화는 결과와 고객에게 제공되는 속도를 개선합니다.

## <a name="sku-assortment-optimization"></a>SKU 분류 최적화

SKU 분류 솔루션은 판매 데이터를 의미 있는 상세 비교로 분할하여 수백만 개의 SKU를 처리해야 합니다. 솔루션의 목표는 고급 분석을 사용하여 제품 분류를 조정함으로써 모든 전문 매장 또는 상점에서 판매를 극대화하는 것입니다. 두 번째 목표는 재고 부족을 제거하고 분류를 향상하는 것입니다. 회계 목표는 5~10%의 매출 증대입니다. 이러한 목표와 관련해서 인사이트를 사용하여 다음을 수행할 수 있습니다.

- SKU 포트폴리오 성과를 파악하고 성과가 낮은 요소를 관리합니다.
- SKU 분배를 최적화하여 재고 부족을 줄입니다.
- 새 SKU가 단기 및 장기 전략을 지원하는 방식을 파악합니다.
- 기존 데이터에서 반복 가능하고 확장 가능하며 작업 가능한 인사이트를 만듭니다.

## <a name="descriptive-analytics"></a>설명 분석

설명 모델은 데이터 요소를 집계하고 제품 판매에 영향을 줄 수 있는 요소 간 관계를 탐색합니다. 이 정보는 위치, 날씨, 인구 조사 데이터 등과 같은 일부 외부 데이터 요소로 확대 될 수 있습니다. 시각화를 통해 사용자는 데이터를 해석 하 여 통찰력을 얻을 수 있습니다. 그러나 이러한 방식에서는 이전 판매 주기에서 발생한 사항이나 현재 기간에 발생하는 사항(데이터를 새로 고치는 빈도에 따라)을 파악하는 것으로 제한됩니다.

이 경우 기존의 데이터 웨어하우징 및 보고 접근 방식으로도 일정 기간 동안 성과가 가장 높은 SKU 및 성과가 가장 낮은 SKU 등을 파악하는 데 충분합니다.

아래 그림은 기록 데이터인 판매 데이터의 일반적인 보고서를 보여 줍니다. 이 보고서는 결과를 필터링하기 위한 기준을 선택하는 확인란이 있는 여러 개의 블록을 포함합니다. 가운데에는 시간 경과에 따른 매출을 보여 주는 두 개의 막대형 차트가 표시됩니다. 첫 번째 차트는 주별 평균 매출을 표시하고, 두 번째 차트는 주별 수량을 표시합니다.

 ![기록 판매 데이터를 보여 주는 대시보드 예제.](assets/sku-optimization-solution-guide/sku-max-model.png)
  
## <a name="predictive-analytics"></a>예측 분석

기록 보고는 발생한 사항을 파악하는 데 유용합니다. 궁극적으로, 발생 가능성이 높은 사항을 예측하려고 합니다. 과거 정보는 이러한 목적에 유용할 수 있습니다. 예를 들어 계절별 추세를 파악할 수 있습니다. 그러나 가령 새 제품의 도입을 모델링하기 위한 “what-if” 시나리오는 처리할 수 없습니다. 이 작업을 수행하려면 판매를 결정하는 궁극적인 요인인 고객 행동 모델링으로 중점을 전환해야 합니다.

### <a name="an-in-depth-look-at-the-problem-choice-models"></a>문제에 대한 심층 조사: 선택 모델

먼저 원하는 사항과 보유한 데이터를 정의하겠습니다.

분류 최적화는 예상 매출을 극대화할 제품의 하위 집합을 찾아서 판매하는 것을 의미합니다. 이것이 원하는 사항입니다.

**트랜잭션 데이터**는 재무 목적을 위해 정기적으로 수집됩니다. 

**분류 데이터**에는 SKU와 관련될 수 있는 다음과 같은 모든 사항이 포함됩니다. 

- SKU 수
- SKU 설명
- 할당 수량
- SKU 및 구매 수량
- 이벤트(예: 구매)의 타임스탬프
- SKU 가격
- POS의 SKU 가격
- 모든 SKU의 상시 재고 수준

하지만 이러한 데이터는 트랜잭션 데이터로 안정적으로 수집되지 않습니다. 

이 문서에서는 간단한 설명을 위해 트랜잭션 데이터 및 SKU 데이터만 고려하고 외부 요인은 고려하지 않습니다.

그럼에도 불구하고, n개 제품 집합이 제공될 경우 가능한 분류는 2n개입니다. 따라서 최적화 문제는 계산을 많이 사용하는 프로세스입니다. 가능한 모든 조합을 평가하는 것은 많은 제품에서 비현실적입니다. 따라서, 일반적으로 분류는 변수 개수를 줄이기 위해 범주(예: 시리얼), 위치 및 기타 기준별로 분할됩니다. 최적화 모델은 순열 개수를 작업 가능한 하위 집합으로 “정리”하려고 합니다.

문제의 가장 중요한 부분은 효과적인 **소비자 행동 모델링**입니다. 이상적일 경우 제공된 제품이 구입하려는 제품과 일치합니다.

소비자의 선택을 예측하기 위한 수학적 모델이 수십년 동안 개발되었습니다. 모델 선택에 따라 궁극적으로 가장 적합한 구현 기술이 결정되므로 모델 선택 사항을 요약하고 몇 가지 고려 사항을 제공하겠습니다.

## <a name="parametric-models"></a>파라메트릭 모델

파라메트릭 모델은 한정된 매개 변수 집합과 함께 함수를 사용하여 고객 행동을 근사화합니다. 데이터에 가장 적합한 매개 변수 집합을 임의로 추정합니다. 가장 오래되었으며 가장 잘 알려진 모델 중 하나는 [다항 로지스틱 회귀](https://en.wikipedia.org/wiki/Multinomial_logistic_regression)(MNL, 다중 클래스 로짓 또는 softmax 회귀라고도 함)입니다. 분류 문제에서 가능한 여러 결과의 확률을 계산하는 데 사용됩니다. 이 경우 MNL을 사용하여 다음을 계산할 수 있습니다.

- 알려진 유용성을 사용한 분류(a)에서 해당 범주의 항목 집합이 고객(v)에게 제공될 경우 소비자(c)가 특정 시간(t)에 항목(i)을 선택할 확률. 

또한 항목 유용성이 해당 기능의 함수일 수 있다고 가정합니다. 유틸리티 측정값에 외부 정보가 포함될 수도 있습니다(예를 들어 우산은 비가 올 때 더 유용함).

매개 변수 추정과 결과 평가 시의 추적 기능 때문에 MNL을 다른 모델의 벤치마크로 사용하는 경우가 많습니다. 즉, MNL보다 성과가 나쁘면 알고리즘은 소용이 없습니다.
여러 모델이 MNL에서 파생되었지만 이 문서의 범위를 벗어나므로 여기서는 설명하지 않습니다. 

R 및 Python 프로그래밍 언어에 대한 라이브러리가 있습니다. R의 경우 glm(및 파생물)을 사용할 수 있습니다.  Python의 경우 [scikit-learn](http://scikit-learn.org/stable/), [biogeme](http://biogeme.epfl.ch/) 및 [larch](https://pypi.org/project/larch/)가 있습니다. 이러한 라이브러리는 MNL 문제 및 다양한 플랫폼에서 해결 방법을 찾기 위한 병렬 해결기를 지정하는 도구를 제공합니다.

가장 최근에는 GPU의 MNL 모델 구현이 다른 방법으로는 추적 불가능한 많은 매개 변수가 있는 복잡한 모델을 계산하기 위해 제안되었습니다.

softmax 출력 계층이 있는 인공신경망은 큰 다중 클래스 문제에서 효과적으로 사용되었습니다. 이러한 인공신경망은 여러 다른 결과에 대한 확률 분포를 나타내는 출력 벡터를 생성합니다. 다른 구현에 비해 학습 속도는 느리지만 다수의 클래스 및 매개 변수를 처리할 수 있습니다. 

## <a name="non-parametric-models"></a>비파라메트릭 모델

널리 사용되기는 하지만 MNL은 인간 행동에 대한 몇 가지 중요한 추정을 전제로 하기 때문에 유용성이 제한될 수 있습니다. 특히, 두 옵션 중에서 선택하는 상대 확률이 나중에 집합에 추가되는 대체 방법과 관계가 없다고 가정합니다. 이것은 대부분의 경우에서 비현실적입니다.

예를 들어 제품 “A”와 제품 “B”를 동일하게 좋아한다면 둘 중에서 한 제품을 선택할 확률은 전체 시간의 50%입니다. 제품 “C”를 이 혼합에 추가하겠습니다. 여전히 제품 “A”를 전체 시간의 50% 동안 선택할 수 있지만, 이제 선호도 25%가 “B”에, 25%가 “C”에 분할됩니다. 상대 확률이 변경되었습니다.

또한 MNL 및 파생물에는 재고 부족 또는 분류의 다양성으로 인한 대체를 고려할 간단한 방법이 없습니다(즉, 명확한 선호도가 없고 재고 항목 중에서 임의로 선택하는 경우).
비파라메트릭 모델은 대체를 고려하고 고객 행동에 더 적은 제약 조건을 적용하도록 설계되었습니다. 

이 모델은 소비자가 분류의 제품에 대해 엄격한 선호도를 표시하는 “순위” 개념을 도입합니다. 따라서 선호도의 내림차순으로 제품을 정렬하여 구매 행동을 모델링할 수 있습니다. 

분류 최적화 문제는 다음과 같이 매출의 극대화로 표시할 수 있습니다.

<center><img style="float: center;" src="assets/sku-optimization-solution-guide/assortment-optimization-problem.png" width="150"/></center>
 
- $r_i$는 제품 i의 매출을 나타냅니다. 
- 순위 k에서 제품 i를 선택한 경우 $y_i^k$는 1이고, 선택하지 않은 경우 0입니다.  
- $\lambda_k$는 고객이 순위 k에 따라 선택할 확률입니다.
- 제품이 분류에 포함된 경우 $x_i$는 1이고, 포함되지 않은 경우 0입니다.
- K는 순위 개수입니다. 
- n은 제품 개수입니다.

다음 제약 조건이 적용됩니다.

- 각 순위에 해당하는 선택 사항이 정확히 한 개 있을 수 있습니다.
- 순위 k에서 제품 i는 분류의 일부인 경우에만 선택할 수 있습니다.
- 제품 i가 분류에 포함된 경우, 순위 k에서 선호도가 더 낮은 옵션을 선택할 수 없습니다. 
- no-purchase는 옵션이므로, 순위에서 선호도가 더 낮은 옵션을 선택할 수 없습니다.

이러한 공식에서 문제는 [혼합된 정수 최적화](https://en.wikipedia.org/wiki/Integer_programming)로 간주될 수 있습니다.

n개 제품이 있을 경우 no-choice 옵션을 포함하여 가능한 순위의 최대 개수는 계승값 (n+1)!입니다.

공식의 제약 조건에 따라 가능한 옵션을 비교적 효율적으로 정리할 수 있지만(예: 가장 선호하는 옵션만 선택되어 1로 설정되고 나머지는 0으로 설정됨), 가능한 대안 개수를 감안할 때 구현의 확장성이 중요하다는 것을 짐작할 수 있습니다.

### <a name="the-importance-of-data"></a>데이터 중요도

판매 데이터는 바로 사용할 수 있다고 앞에서 언급했습니다. 이 데이터를 사용하여 분류 최적화 모델에 대해 알려 드리겠습니다. 특히, 확률 분포 λ를 찾으려고 합니다.

POS 시스템의 판매 데이터는 타임스탬프를 포함하는 트랜잭션과 해당 시점 및 위치에서 고객에게 표시되는 제품 집합으로 구성됩니다. 판매 데이터에서, 분류 $S_m$이 제공된 경우 요소 vi,m이 고객에게 항목 i를 판매할 확률을 나타내는 실제 판매 벡터를 구성할 수 있습니다.

다음과 같이 매트릭스를 구성할 수도 있습니다.

<center><img style="float: center;" src="assets/sku-optimization-solution-guide/matrix-construction.png" width="300"/></center>

판매 데이터가 제공된 경우 확률 분포 λ를 찾는 것이 또 다른 최적화 문제가 됩니다. 벡터 λ를 찾아서 판매 예측 오류를 최소화하려고 합니다.

$$min_\lambda|\Lambda\lambda - v|$$

계산을 회귀로 표현할 수도 있으므로 다변형 의사 결정 트리와 같은 모델을 사용할 수 있습니다. 

## <a name="implementation-details"></a>구현 세부 정보

위의 공식에서 유추할 수 있듯이 최적화 모델은 데이터 기반인 동시에 계산을 많이 사용합니다.

Neal Analytics와 같은 Microsoft 파트너는 이러한 조건을 충족하기 위해 강력한 아키텍처를 개발했습니다. [SKU Max](https://appsource.microsoft.com/product/web-apps/neal_analytics.8066ad01-1e61-40cd-bd33-9b86c65fa73a?tab=Overview?WT.mc_id=invopt-article-gmarchet)(SKU 최댓값)를 참조하세요. 이러한 아키텍처를 예제로 사용하고 몇 가지 고려 사항을 제공합니다.

- 첫째, 해당 아키텍처는 (1) 강력하고 확장 가능한 데이터 파이프라인을 사용하여 모델을 피드하고 (2) 강력하고 확장 가능한 실행 인프라를 사용하여 모델을 실행합니다.
- 둘째, 계획자가 대시보드를 통해 결과를 쉽게 이용할 수 있습니다.

그림 2는 예제 아키텍처를 보여 줍니다. 여기에는 캡처, 프로세스, 모델 및 운용의 네 가지 주요 블록이 포함되어 있습니다. 각 블록에는 주요 프로세스가 포함됩니다. 캡처에는 “데이터 사전 처리”가 포함되고, “프로세스”에는 “데이터 저장” 기능이 포함되며, 모델에는 “기계 학습 모델 학습” 기능이 포함되고, 운용에는 “데이터 저장” 및 보고 옵션(예: 대시보드)이 포함됩니다.

![캡처, 프로세스, 모델 및 운용의 네 부분으로 이루어진 아키텍처.](assets/sku-optimization-solution-guide/architecture-sku-optimization.png)<center><font size="1">_그림 2: SKU 최적화 아키텍처 - Neal Analytics 제공_</font></center>

## <a name="the-data-pipeline"></a>데이터 파이프라인

아키텍처는 모델의 학습 및 작업 둘 다에 사용되는 데이터 파이프라인 설정의 중요성을 강조합니다. 통합 워크플로를 설계하고 실행할 수 있는 관리되는 ETL(추출, 변환 및 로드) 서비스인 [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction?WT.mc_id=invopt-article-gmarchet)를 사용하여 파이프라인의 작업을 오케스트레이션합니다.

Azure Data Factory는 데이터 세트를 이용 및/또는 생성하는 “작업”이라는 구성 요소가 있는 관리되는 서비스입니다.

작업은 다음으로 분할될 수 있습니다.

- 데이터 이동(예: 원본에서 대상으로 복사)
- 데이터 변환(예: SQL 쿼리를 사용하여 집계 또는 저장 프로시저 실행)

데이터 팩터리 서비스를 통해 작업 집합을 연결하는 워크플로를 예약, 모니터링 및 관리할 수 있습니다. 전체 워크플로를 “파이프라인”이라고 합니다.

캡처 단계에서는 복사 작업(Data Factory에 기본 제공됨)을 활용하여 다양한 원본(온-프레미스 및 클라우드)의 데이터를 Azure SQL Data Warehouse로 전송할 수 있습니다. 문서에 제공된 수행 방법의 예는 다음과 같습니다.

- [Azure SQL DW에(서) 데이터 복사](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-data-warehouse?WT.mc_id=invopt-article-gmarchet)
- [Azure SQL DW에 데이터 로드](https://docs.microsoft.com/azure/data-factory/load-azure-sql-data-warehouse?WT.mc_id=invopt-article-gmarchet)

아래 그림은 파이프라인의 정의를 보여 줍니다. 균일한 크기의 연속된 블록 세 개로 구성되어 있습니다. 처음 두 블록은 데이터 세트이고 화살표로 연결된 작업이 데이터 흐름을 나타냅니다. 세 번째 블록은 “파이프라인”으로 레이블이 지정되며, 단순히 처음 두 블록을 가리켜 캡슐화를 나타냅니다. 

 ![Azure Data Factory 개념: 작업 파이프라인에서 이용되는 데이터 세트.](assets/sku-optimization-solution-guide/azure-data-factory.png)<center><font size="1">_그림 3: Azure Data Factory의 기본 개념_</font></center>

Neal Analytics 솔루션에서 사용하는 데이터 형식의 예는 Microsoft Appsource 페이지에서 확인할 수 있습니다. 솔루션에는 다음 데이터 세트가 포함됩니다.

- 상점 및 SKU의 각 조합에 대한 판매 기록 데이터
- 상점 및 소비자 기록
- SKU 코드 및 설명
- 제품의 기능을 캡처하는 SKU 특성(예: 크기, 재질). 일반적으로 파라메트릭 모델에서 제품 변형을 구별하는 데 사용됩니다.

데이터 원본이 특정 형식으로 표현되지 않은 경우 Data Factory는 일련의 변환 작업을 제공합니다. 

프로세스 단계에서는 SQL Data Warehouse가 기본 스토리지 엔진입니다. 따라서 이러한 변환 작업을 파이프라인의 일부로 자동 호출될 수 있는 SQL 저장 프로시저로 표현하는 것이 좋습니다. 문서에서는 다음과 같은 자세한 지침을 제공합니다.

- [SQL 저장 프로시저로 데이터 변환](https://docs.microsoft.com/azure/data-factory/transform-data-using-stored-procedure?WT.mc_id=invopt-article-gmarchet)

Data Factory는 SQL Data Warehouse 및 SQL 저장 프로시저로 제한하지 않습니다. 실제로는 다양한 플랫폼과 통합됩니다. 예를 들어 Databricks를 사용하고 변환을 위해 Python 스크립트를 실행할 수도 있습니다. 이 경우 다음 “모델” 단계에서 기계 학습 알고리즘의 스토리지, 변환 및 학습에 하나의 플랫폼을 모두 사용할 수 있기 때문에 장점이 됩니다.

## <a name="training-the-ml-algorithm"></a>ML 알고리즘 학습

파라메트릭 모델 및 비파라메트릭 모델을 구현하는 데 도움이 되는 몇 가지 도구가 있습니다. 사용자의 선택은 확장성 및 성능 요구 사항에 따라 다릅니다.

[Azure ML Studio](https://studio.azureml.net/?WT.mc_id=invopt-article-gmarchet)는 프로토타입을 제작하기 위한 훌륭한 도구입니다. 코드 모듈(R 또는 Python)을 사용하거나 미리 정의된 ML 구성 요소(예: 다중 클래스 분류자, 승격된 의사 결정 트리 회귀)를 그래픽 환경에서 사용하여 학습 워크플로를 쉽게 빌드하고 실행할 수 있는 방법을 제공합니다. 추가 이용을 위해 학습된 모델을 마우스만 몇 번 클릭해서 웹 서비스로 게시하여 REST 인터페이스를 생성할 수도 있습니다. 

그러나 처리할 수 있는 데이터 크기가 10GB(현재)로 제한되며, 각 구성 요소에 사용 가능한 코어 수도 2개(현재)로 제한됩니다.

아래 그림은 사용 중인 ML Studio의 예를 보여 줍니다. 기계 학습 실험의 그래픽 표시입니다. 그림은 여러 개의 블록 그룹을 보여 줍니다. 각 블록 집합은 실험의 단계를 나타내고, 각 블록이 하나 이상의 블록에 연결되어 데이터 입력 및 출력을 나타냅니다.

![사용 중인 기계 학습 스튜디오의 예.](assets/sku-optimization-solution-guide/ml-training-pipeline-example.png)<center><font size="1">_그림 4: R 및 미리 빌드된 구성 요소를 사용한 ML 학습 파이프라인의 예_</font></center>

추가 확장이 필요하지만 일반적인 기계 학습 알고리즘(예: 다항 로지스틱 회귀)에 대한 Microsoft의 일부 빠른 병렬 구현을 여전히 사용하려는 경우 Azure Data Science Virtual Machine에서 실행되는 Microsoft ML Server를 고려하는 것이 좋습니다.

대용량 데이터 크기(TB)의 경우, 스토리지 및 계산 요소가 다음과 같을 수 있는 플랫폼을 선택하는 것이 좋습니다.

- 모델을 학습하지 않을 때 비용을 제한하기 위해 독립적으로 확장됩니다.
- 여러 코어에 계산을 분배합니다.
- 스토리지에 “가까운” 곳에서 계산을 실행하여 데이터 이동을 제한합니다.

Azure [HDInsight](https://azure.microsoft.com/services/hdinsight/?WT.mc_id=invopt-article-gmarchet) 및 [Databricks](https://azure.microsoft.com/services/databricks/?WT.mc_id=invopt-article-gmarchet)는 둘 다 이러한 요구 사항을 충족합니다. 또한 둘 다 Azure Data Factory 편집기 내에서 지원되는 실행 플랫폼입니다. 둘 다 비교적 간단하게 워크플로에 통합할 수 있습니다.

ML Server 및 해당 라이브러리를 HDInsight에 배포할 수 있지만, 플랫폼 기능을 최대한 활용하려면 SparkML, Python의 Microsoft ML Spark 라이브러리 또는 기타 전문가 선형 프로그래밍 해결기(예: TFoCS, Spark-LP 또는 SolveDF)를 사용하여 선택한 ML 알고리즘을 구현하는 것이 좋습니다. 

그러면 Data Factory 워크플로에서 적절한 pySpark 스크립트 또는 Notebook을 호출하여 학습 프로세스를 시작할 수 있습니다. 이 기능은 그래픽 편집기에서 완전히 지원됩니다. 자세한 내용은 [Azure Data Factory에서 Databricks Notebook 작업으로 Databricks Notebook 실행](https://docs.microsoft.com/azure/data-factory/transform-data-using-databricks-notebook?WT.mc_id=invopt-article-gmarchet)을 참조하세요.

아래 그림은 Azure Portal을 통해 액세스되는 Data Factory 사용자 인터페이스를 보여 줍니다. 워크플로의 다양한 프로세스에 대한 블록을 포함합니다. 

![Databricks Notebook 작업을 표시하는 Data Factory 인터페이스.](assets/sku-optimization-solution-guide/data-factory-pipeline-databricks.png)<center><font size="1">_그림 5: Databricks Notebook 작업을 사용한 Data Factory 파이프라인의 예_</font></center>

또한 [인벤토리 최적화 솔루션](https://gallery.azure.ai/Solution/Inventory-Optimization-3?WT.mc_id=invopt-article-gmarchet)에서는 [Azure Batch](https://azure.microsoft.com/services/batch/?WT.mc_id=invopt-article-gmarchet)를 통해 확장되는 컨테이너 기반 해결기 구현을 제안합니다. [pyomo](http://www.pyomo.org/about/)와 같은 전문가 최적화 라이브러리를 사용하면 Python 프로그래밍 언어로 최적화 문제를 표시한 다음, [bonmin](https://projects.coin-or.org/Bonmin)(오픈 소스) 또는 [gurobi](http://www.gurobi.com/)(상업용)와 같은 독립 해결기를 호출하여 솔루션을 찾을 수 있습니다.

인벤토리 최적화 문서는 분류 최적화가 아닌 다른 문제(주문 수량)를 다루지만, Azure의 해결기 구현은 유사하게 적용됩니다.

지금까지 제안된 것보다 복잡하긴 하지만 이 기술은 대체로 사용할 수 있는 코어 수로 제한되는 최대 확장성을 허용합니다.

## <a name="running-the-model-operationalize"></a>모델 실행(운용)

모델을 학습한 후 실행하려면 일반적으로 배포에 사용된 인프라와 다른 인프라가 필요합니다. 쉽게 이용할 수 있도록 REST 인터페이스를 사용하여 웹 서비스로 배포할 수도 있습니다. Azure ML Studio와 ML Server는 둘 다 이러한 서비스를 만드는 프로세스를 자동화합니다. ML Server의 경우 지원 인프라 배포를 위한 템플릿을 제공합니다. 관련 [문서](https://docs.microsoft.com/machine-learning-server/what-is-operationalization?WT.mc_id=invopt-article-gmarchet)를 참조하세요.

아래 그림은 배포의 아키텍처를 보여 줍니다. R 언어 및 Python을 실행하는 서버 표시를 포함합니다. 두 서버는 모두 계산을 수행하는 웹 노드의 하위 섹션과 통신합니다. 큰 데이터 저장소가 계산 블록에 연결되어 있습니다.

![ML Server 배포 다이어그램. 부하 분산 장치가 실행할 여러 노드 앞에 옵니다.](assets/sku-optimization-solution-guide/ml-server-deployment-example.png)<center><font size="1">_그림 6: ML Server 배포의 예_</font></center>


HDInsight 또는 Databricks에 생성되어 Spark 환경(라이브러리, 병렬 기능 등)에 종속된 모델의 경우 클러스터에서 실행하는 것이 좋습니다. 이 지침은 [여기](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/spark-model-consumption?WT.mc_id=invopt-article-gmarchet)에서 제공합니다.

이 경우 운영 모델 자체가 채점을 위해 Data Factory 파이프라인 작업을 통해 호출될 수 있다는 장점이 있습니다.

컨테이너를 사용하기 위해 모델을 패키지하여 Azure Kubernetes Service에 배포할 수 있습니다. 프로토타입에는 [Azure Data Science VM](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/?WT.mc_id=invopt-article-gmarchet)을 사용해야 합니다. Azure ML [명령줄](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/model-management-service-deploy?WT.mc_id=invopt-article-gmarchet) 도구도 VM에 설치해야 합니다.

## <a name="data-output-and-reporting"></a>데이터 출력 및 보고

일단 배포되면 모델은 재무 트랜잭션 워크플로 및 재고 판독값을 처리하여 최적 분류 예측을 생성할 수 있습니다. 따라서 추가 분석을 위해 생성된 데이터를 Azure SQL Data Warehouse에 다시 저장할 수 있습니다. 특히, 다양한 SKU의 기록 성과를 연구하여 최고 매출 생성 SKU 및 손실 생성 SKU를 확인할 수 있습니다. 그런 다음, 이러한 결과를 모델에서 제안하는 분류와 비교하여 성과 및 재학습 필요성을 평가할 수 있습니다.

[PowerBI](https://powerbi.microsoft.com/get-started/?&OCID=AID719832_SEM_uhlWLg3x&lnkd=Google_PowerBI_Brand&gclid=CjwKCAjw5ZPcBRBkEiwA-avvkyOLMJCrhqH8iac84aLX7EcUQIirSSqUCostzGi8y_XntJTCD73ZixoCQ4sQAvD_BwE?WT.mc_id=invopt-article-gmarchet)는 프로세스에서 생성된 데이터를 분석하고 표시하는 방법을 제공합니다. 

아래 그림은 일반적인 Power BI 대시보드를 보여 줍니다. SKU 재고 정보를 표시하는 두 개의 그래프가 있습니다. 

![12개월간의 결과를 보여 주는 대시보드의 예.](assets/sku-optimization-solution-guide/sku-max-model.png)<center><font size="1">_그림 7: 모델 결과 보고서의 예 - Neal Analytics 제공_</font></center>

## <a name="security-considerations"></a>보안 고려사항

중요한 데이터를 다루는 솔루션에는 재무 레코드, 재고 수준 및 가격 정보가 포함되어 있습니다. 이러한 중요한 데이터를 보호해야 합니다. 데이터의 보안 및 개인 정보 보호에 관한 염려를 종식하려면 다음 사항에 유의하세요.

- Azure Integration Runtime을 사용하여 온-프레미스에서 일부 Azure Data Factory 파이프라인을 실행할 수 있습니다. 런타임은 온-프레미스 원본에(서) 데이터 이동 작업을 실행합니다. 또한 온-프레미스에서 실행하기 위해 작업을 디스패치합니다.
  - 특히, Azure에 전송될 데이터를 익명화하는 사용자 지정 작업을 개발하고 온-프레미스에서 실행하는 것이 좋습니다.
- 언급한 모든 서비스는 전송 중인 데이터와 저장된 데이터의 암호화를 지원합니다. Azure Data Lake에 데이터를 저장하도록 선택하면 기본적으로 암호화가 사용됩니다. Azure SQL Data Warehouse를 사용하는 경우 TDE(투명한 데이터 암호화)를 사용하도록 설정할 수 있습니다.
- ML Studio를 제외하고 언급한 모든 서비스는 인증 및 권한 부여를 위해 Azure Active Directory와의 통합을 지원합니다. 고유 코드를 작성하는 경우 애플리케이션에 해당 통합을 빌드해야 합니다.

GDPR에 대한 자세한 내용은 [준수](https://www.microsoft.com/trustcenter?WT.mc_id=invopt-article-gmarchet) 페이지를 참조하세요.

## <a name="technologies-mentioned"></a>언급한 기술

- [Azure Batch](https://azure.microsoft.com/services/batch/?WT.mc_id=invopt-article-gmarchet)
- [Azure Active Directory](https://azure.microsoft.com/services/active-directory/?&OCID=AID719825_SEM_w1MNAVjn&lnkd=Google_Azure_Brand&gclid=CjwKCAjw5ZPcBRBkEiwA-avvk4bGtyQo11KBY-u2skor1SydsSl1vrYUmhyGhhwyJhDlAYpnMmIcRRoCTfsQAvD_BwE&dclid=CMn6lvfRkd0CFRwBrQYdtIoJOA?WT.mc_id=invopt-article-gmarchet)
- [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction?WT.mc_id=invopt-article-gmarchet)
- [Azure Integration Runtime](https://docs.microsoft.com/azure/data-factory/concepts-integration-runtime?WT.mc_id=invopt-article-gmarchet)
- [HDInsight](https://azure.microsoft.com/services/hdinsight/?WT.mc_id=invopt-article-gmarchet)
- [Databricks](https://azure.microsoft.com/services/databricks/?WT.mc_id=invopt-article-gmarchet)
- [Azure SQL 데이터 웨어하우스](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is?WT.mc_id=invopt-article-gmarchet)
- [Azure ML Studio](https://studio.azureml.net/?WT.mc_id=invopt-article-gmarchet)
- [Microsoft ML Server](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server?WT.mc_id=invopt-article-gmarchet)
- [Azure Data Science VM](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/?WT.mc_id=invopt-article-gmarchet)
- [Azure Kubernetes Service](https://azure.microsoft.com/services/kubernetes-service/?WT.mc_id=invopt-article-gmarchet)
- [Microsoft PowerBI](https://powerbi.microsoft.com/?WT.mc_id=invopt-article-gmarchet)
- [Pyomo Optimization Modelling Language](http://www.pyomo.org/)
- [Bonmin Solver](https://projects.coin-or.org/Bonmin)
- [TFoCS solver for Spark](https://github.com/databricks/spark-tfocs)
