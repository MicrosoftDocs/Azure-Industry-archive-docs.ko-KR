---
title: 보험 통계 위험 분석 및 모델링 솔루션 가이드
author: scseely
ms.author: scseely
ms.date: 08/23/2018
ms.topic: article
ms.service: industry
description: 이 솔루션 가이드에서는 보험 통계 개발자가 기존 솔루션과 지원 인프라를 Azure로 이동할 수 있는 방법을 설명합니다.
ms.openlocfilehash: 82cb53d529f6d7524ae1f9c148118b5edddc648b
ms.sourcegitcommit: 76f2862adbec59311b5888e043a120f89dc862af
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "51654290"
---
# <a name="actuarial-risk-analysis-and-financial-modeling-solution-guide"></a>보험 통계 위험 분석 및 금융 모델링 솔루션 가이드

지난 몇 년 동안 보험 회사 및 보험 유사 상품을 제공하는 기업들은 여러 새로운 규정을 적용 받게 되었습니다. 이러한 새 규정에서는 보험 회사에 더 광범위한 재무 모델링을 요구합니다. 유럽 연합은 보험 회사가 연말에 지급 능력이 있음을 검증하기 위해 적합한 분석을 수행했음을 입증하도록 요구하는 [Solvency II](https://eur-lex.europa.eu/legal-content/EN/ALL/?uri=celex%3A32009L0138)를 시행했습니다. 변액연금을 제공하는 보험 회사는 광범위 자산 및 부패 현금 흐름 분석을 포함하는 [Actuarial Guideline XLIII](https://eur-lex.europa.eu/legal-content/EN/ALL/?uri=celex%3A32009L0138)를 따라야 합니다. 유사 보험 상품을 취급하는 회사를 포함한 모든 형태의 보험 회사는 2021년까지 IFRS 17([International Financial Reporting Standard 17](https://www.ifrs.org/supporting-implementation/supporting-materials-by-ifrs-standard/ifrs-17/))을 이행해야 합니다. 보험 회사가 영업하는 사법 관할지에 따라 다른 규정도 존재합니다. 이러한 표준 및 규정에 따라 보험 계리인은 자산 및 부채를 모델링할 때 계산 집약적 기술을 사용해야 합니다. 대부분의 분석에서는 자산 및 부채 같은 순차적 입력 항목에 대해 확률적으로 생성된 시나리오 데이터를 사용합니다. 규정 요구 사항 이외에도 보험 계리인은 규정 보고서를 생성하는 모델에 대한 입력 테이블을 생성하기 위해 상당한 분량의 재무 모델링과 연산을 수행합니다. 내부 그리드로는 계산 요구를 만족하지 못하기 때문에 보험 계리인은 꾸준히 클라우드로 이동하고 있습니다.

보험 계리인은 결과를 검토, 평가 및 검증할 시간을 더 많이 확보하기 위해 클라우드로 이동합니다. 규제 기관에서는 보험 회사를 감사할 때 보험 계리인은 결과를 설명할 수 있어야 합니다. 클라우드로 이동하면 병렬 처리 능력을 통해 24~120시간에 20000시간 분량의 분석을 수행할 수 있는 연산 자원을 사용할 수 있게 됩니다. 이런 규모에의 요구를 지원하기 위해 보험 계리 소프트웨어를 제작하는 많은 기업에서는 Azure에서의 계산 실행을 허용하는 솔루션을 제공하고 있습니다. 이중 일부 솔루션은 [HPC Pack](https://docs.microsoft.com/powershell/high-performance-computing/overview?view=hpc16-ps&WT.mc_id=riskmodel-docs-scseely) 같은 Azure 및 온-프레미스에서 실행되는 기술을 바탕으로 구축됩니다. 다른 솔루션은 Azure 네이티브로, [Azure Batch](https://docs.microsoft.com/azure/batch?WT.mc_id=riskmodel-docs-scseely), [Virtual Machine Scale Sets](https://docs.microsoft.com/azure/virtual-machine-scale-sets?WT.mc_id=riskmodel-docs-scseely) 또는 사용자 지정 크기 조정 솔루션을 사용합니다.

이 문서에서는 보험 계리 관련 개발자들이 어떻게 Azure를 모델링 패키지와 함께 사용하여 위험을 분석할 수 있는지 살펴보겠습니다. 이 문서에서는 모델링 패키지가 Azure 규모에서실행하기 위해 사용하는 일부 Azure 기술에 대해 설명합니다. 같은 기술을 사용하여 데이터를 더 심층적으로 분석할 수 있습니다. 다음 항목을 살펴보겠습니다.

- Azure에서 짧은 시간에 더 큰 모델 실행
- 결과에 대한 보고
- 데이터 보존 관리

생명, 재산, 상해, 건강 또는 기타 보험 등, 어떤 보험을 서비스하던 보험 회사로서의 지급 능력을 유지할 수 있게 투자와 보험료를 조정하려면 자산과 부채에 관한 재무 및 위험 모델을 만들어야 합니다. IFRS 17 보고에 따라 CSM(약정 서비스 이익) 산출 등, 보험 계리인이 만드는 모델의 변화도 필요한데 이로 인해 보험 회사가 시간에 따라 수익을 관리하는 방식도 변화하게 됩니다.

## <a name="running-more-in-less-time-in-azure"></a>Azure에서 더 적은 시간에 더 많은 항목 실행

클라우드가 재무 및 위험 모델을 더 빠르고 쉽게 실행한다는 사실을 믿어 의심치 않습니다. 많은 보험 회사들은 외면적 계산의 이면에서 여러 문제를 확인할 수 있었습니다. 이러한 계산을 시작부터 끝까지 모두 실행하려면 몇 년, 심지어 몇십 년의 시간이 필요합니다. 이러한 실행 시간 문제를 해결하기 위한 기술이 필요합니다. 전략은 다음과 같습니다.

- 데이터 준비: 일부 데이터는 느리게 변경됩니다. 정책 또는 서비스 계약이 적용되면 청구가 예측 가능한 단계로 진행됩니다. 도착하면 모델 실행에 필요한 데이터를 준비할 수 있으므로 데이터 정리 및 준비를 위해 상당한시간을 계획할 필요가 없어집니다. 가중치가 적용된 표현을 통해 순차적 데이터에 대한 대용물을 만드는 데 클러스터링을 사용할 수도 있습니다. 레코드 수가 적을수록 보통 계산 시간이 줄어듭니다.
- 병렬 처리: 둘 이상의 항목에 대해 같은 분석을 수행해야 할 경우 동시에 분석을 수행할 수 있습니다.

이 항목을 개별적으로 살펴보겠습니다.

### <a name="data-preparation"></a>데이터 준비

여러 다른 원본에서의 데이터 흐름. 비즈니스 장부에 반 구조화된 약관 데이터가 있습니다. 다양한 신청서에 등장하는 피보험자, 회사, 항목에 대한 정보입니다. ESG(경제 시나리오 생성기)는 모델이 사용할 수 있는 양식으로의 변환이 필요할 수 있는 다양한 형식의 데이터를 생산합니다. 자산 가치에 관한 현재 데이터도 정규화가 필요합니다. 주식 시장 데이터, 임대 현금 흐름 데이터, 모기지 납부 정보, 기타 자산 데이터는 모두 원본에서 모델로 이동할 때 어떤 준비가 필요합니다. 마지막으로, 최신 환경 데이터를 기준으로 가정을 업데이트할 필요가 있습니다. 모델 실행 속도를 높이기 위해 미리 데이터를 준비합니다. 실행 시간이 되면 마지막 예약 업데이트 이후 변경을 추가하기 위해 필요한 업데이트를 수행합니다.

자, 어떻게 데이터를 준비할까요? 먼저 공통된 부분을 살펴본 다음, 데이터가 표시되는 다양한 방식을 작업하는 방법에 대해 알아보겠습니다. 먼저 지난 동기화 이후 모든 변경 내용을 확보하기 위한 메커니즘이 필요합니다. 이 메커니즘은 정렬 가능한 값을 포함해야 합니다. 최근 변경 내용에 대해 이 값은 이전 변경 내용보다 커야 합니다. 가장 일반적인 두 메커니즘은 계속 증가하는 ID 필드 또는 타임스탬프입니다. 레코드에 증가하는 ID 키가 있으나 나머지 레코드는 업데이트 가능한 필드를 포함할 경우 &quot;마지막으로 수정된&quot; 타임스탬프 같은 항목을 사용하여 변경 내용을 찾아야 합니다. 레코드를 처리한 후에는 지난 업데이트 항목의 정렬 가능한 값을 기록합니다. _lastModified_라고 하는 필드의 타임스탬프일 수 있는 이 값은 데이터 저장소에 대한 후속 쿼리에 사용되는 워터마크가 됩니다. 데이터 변경 내용은 여러 가지 방법으로 처리할 수 있습니다. 최소 리소스를 사용하는 일반적인 두 가지 메커니즘은 다음과 같습니다.

1. 처리해야 할 변경 내용이 수백 또는 수천 개인 경우: 데이터를 Blob 스토리지에 업로드합니다. [Azure Data Factory](https://docs.microsoft.com/azure/data-factory?WT.mc_id=riskmodel-docs-scseely)에서 이벤트 트리거를 사용하여 변경 세트를 처리합니다.
2. 처리해야 할 변경 내용 세트가 소규모이거나 변경 발생 즉시 데이터를 업데이트하려는 경우 각 변경 내용을 [Service Bus](https://docs.microsoft.com/azure/service-bus-messaging?WT.mc_id=riskmodel-docs-scseely)나 [스토리지 큐](https://docs.microsoft.com/azure/storage/queues/storage-queues-introduction?WT.mc_id=riskmodel-docs-scseely)에서 호스트하는 큐 메시지에 넣습니다. [이 문서에서는](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted?WT.mc_id=riskmodel-docs-scseely) 두 큐 기술 간의 절충 문제를 잘 설명하고 있습니다. 메시지가 큐에 들어가면 Azure Functions 또는 Azure Data Factory에서 메시지를 처리하기 위해 트리거를 사용할 수 있습니다.

다음 그림은 일반적인 시나리오를 보여 줍니다. 먼저 예약된 작업이 일부 데이터 세트를 수집하고 파일을 스토리지에 배치합니다. 예약된 작업은 온-프레미스에서 실행되는 CRON 작업이거나, 타이머에서 실행되는 [스케줄러 작업](https://docs.microsoft.com/azure/scheduler?WT.mc_id=riskmodel-docs-scseely), [논리 앱](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview?WT.mc_id=riskmodel-docs-scseely) 또는 기타 항목이 될 수 있습니다. 파일이 업로드되면 [Azure Function](https://docs.microsoft.com/azure/azure-functions?WT.mc_id=riskmodel-docs-scseely) 또는 **Data Factory** 인스턴스를 트리거하여 데이터를 처리할 수 있습니다. 파일이 단시간에 처리될 경우 **함수**를 사용합니다. 처리가 복잡하여 AI 또는 기타 복합 스크립팅이 필요한 경우 [HDInsight](https://docs.microsoft.com/azure/hdinsight?WT.mc_id=riskmodel-docs-scseely), [Azure Databricks](https://docs.microsoft.com/azure/azure-databricks?WT.mc_id=riskmodel-docs-scseely) 또는 다른 사용자 지정 항목이 더 효율적일 수 있습니다. 마쳤으면 파일이 데이터베이스의 새 파일이나 레코드 형태의 사용 가능한 양식으로 정리됩니다.

 ![](./assets/insurance-risk-assets/process-files.png)

데이터가 Azure에 있으면 모델링 애플리케이션을 통해 사용할 수 있게 만들어야 합니다. 사용자 지정 변환을 수행하고, **HDInsight** 또는 Azure **Databricks**을 통해 항목을 실행하여 더 큰 항목을 주입하거나 데이터를 올바른 데이터 세트에 복사하는 코드를 작성할 수 있습니다. 빅 데이터 도구를 사용하는 것도 비정형 데이터를 구조화된 데이터로 변환하거나 수신된 데이터에서 AI 및 ML을 실행하는 등의 작업에 유용할 수 있습니다. 가상 머신을 호스트하고, 데이터를 직접 온-프레미스에서 데이터 원본에 업로드하고, Azure Functions를 직접 호출하는 등의 작업도 수행할 수 있습니다.

나중에 이 데이터를 모델에서 사용해야 합니다. 이를 위한 방법은 계산에서 데이터에 액세스하는 방법에 따라 크게 좌우됩니다. 일부 모델링 시스템에서는 모든 데이터 파일이 계산을 실행하는 노드에 있어야 합니다. 다른 경우 [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/?WT.mc_id=riskmodel-docs-scseely), [MySQL](https://docs.microsoft.com/azure/mysql/?WT.mc_id=riskmodel-docs-scseely), 또는 [PostgreSQL](https://docs.microsoft.com/azure/postgresql/?WT.mc_id=riskmodel-docs-scseely) 같은 데이터베이스를 사용할 수 있습니다. 이러한 항목의 저비용 버전을 사용한 다음, 모델링 실행 중에 성능을 강화할 수 있습니다. 이렇게 하면 일상 업무에 필요한 가격을 적용하면서도 수천 개 코어가 데이터를 요청할 때만 추가 속도를 확보할 수 있습니다. 일반적으로 이 데이터는 모델링 실행 중에 읽기 전용 상태입니다. 여러 지역에서 계산이 발생할 경우 [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/distribute-data-globally?WT.mc_id=riskmodel-docs-scseely) 또는 [Azure SQL 지역 복제](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview?WT.mc_id=riskmodel-docs-scseely)를 사용할 수 있습니다. 모두 낮은 대기 시간에서 지역 간에 데이터를 자동으로 복제하는 메커니즘을 제공합니다. 개발자가 알고 있는 도구, 데이터를 모델링한 방법, 모델링 실행에 사용된 지역 수에 따라 메커니즘을 선택할 수 있습니다.

데이터를 저장할 위치에 관해서는 세심히 생각할 필요가 있습니다. 동일한 데이터에 대한 동시 요청 수가 얼마나 될지를 이해해야 합니다. 정보 배포는 방법을 고려해야 합니다.

- 각 계산 노드에 자체 사본이 있나요?
- 이 사본이 고대역폭 위치를 통해 공유되나요?

Azure SQL을 사용하여 데이터를 중앙 집중식으로 유지한다면 데이터베이스를 대부분의 시간에는 더 낮은 가격의 계층에 유지할 가능성이 높습니다. 데이터가 모델링 실행 중에만 사용되며 자주 업데이트되지 않는다면 Azure 고객은 데이터를 백업하고 실행 사이에는 데이터베이스 인스턴스를 끄는 방식으로 진행하게 됩니다. 이렇게 하면 큰 금액을 절약할 수 있습니다. 고객은 [Azure SQL Elastic Pool](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool?WT.mc_id=riskmodel-docs-scseely)도 이용할 수 있습니다. 이들은 데이터베이스 비용을 억제하기 위해 설계되었습니다. 특히 서로 다른 시간대에 어떤 데이터베이스에 로드가 클지 모르는 상태에서 그렇습니다. 탄력적 풀을 사용하면 데이터베이스 컬렉션이 필요한 만큼의 능력을 사용한 다음, 시스템 어딘가에서 수요가 바뀌면 다시 규모를 되돌릴 수 있습니다.

나중에 프로세스에서 계산이 같은 데이터를 사용할 수 있게 모델링 실행 중에 데이터 동기화를 사용하지 않게 설정해야 할 수 있습니다. 큐를 사용할 경우 메시지 프로세서를 사용하지 않게 하면서 큐가 데이터를 수신할 수 있게 허용합니다.

또한 경제 시나리오를 생성하고, 보험 계리 가정을 업데이트하며, 일반적으로 다른 고정 데이터를 업데이트하기 위해 실행 전 시간을 사용할 수도 있습니다. ESG(경제 시나리오 생성)를 살펴보겠습니다. [보험 계리인 협회](https://www.soa.org/)는 미국 재무부 결과를 모델링하는 [AIRG(Academy Interest Rate Generator)](https://www.soa.org/tables-calcs-tools/research-scenario/)를  제공합니다. AIRG는 VM-20(Valuation Manual 20) 계산 같은 항목에서 사용하기 위해 미리 지정됩니다. 다른 ESG는 주식 시장, 모기지, 상품 가격 등을 모델링할 수 있습니다.

환경에서 데이터를 미리 처리하므로 다른 부분도 일찍 실행할 수 있습니다. 예를 들어, 더 큰 모집단을 나타내기 위해 레코드를 사용하는 yv1ou 모델링 항목이 있을 수 있습니다. 보통은 레코드 클러스터링을 통해 이를 수행합니다. 데이터 세트가 하루 한 번처럼 산발적으로 업데이트될 경우 주입 프로세스의 일환으로 모델에서 사용되는 레코드 세트를 줄일 수 있습니다.

실제 예제를 살펴보겠습니다. IFRS-17에서는 두 계약 시작일 간의 최대 차이가 1년 미만이 되도록 계약을 하나로 그룹화해야 합니다. 이를 간편하게 하기 위해 계약 연도를 그룹화 메커니즘으로 사용한다고 가정해 보겠습니다. 이러한 구분은 데이터를 Azure에 로드하는 동안 파일을 통해 레코드를 읽고 적합한 연도 그룹으로 이동하여 수행할 수 있습니다.

데이터 준비에 집중하면 모델 구성 요소 실행에 필요한 시간이 줄어듭니다. 초기에 데이터를 가져오면 모델 실행을 위한 클록 시간을 절약할 수 있습니다.

### <a name="parallelization"></a>병렬 처리

적절한 단계의 병렬 처리는 클록 실행 시간을 크게 줄일 수 있습니다. 이러한 속도 향상은 구현하는 부분을 간소화하고 둘 이상의 작업이 동시에 실행되게 하는 방식으로 모델을 표현하는 방식을 인지함으로써 가능합니다. 여기서의 핵심은 작업 요청 크기와 개별 노드 생산성 간의 균형을 찾는 것입니다. 작업이 평가에서보다 설정 및 정리에 더 많은 시간이 소요된다면 너무 적게 지정한 것입니다. 작업이 너무 크면 실행 시간이 향상되지 않습니다. 여러 노드에 분산되는 정도의 작업이 필요하고 실행 소요 시간에서 긍정적인 변화를 주고자 할 수 있습니다.

시스템을 최대한 활용하려면 모델 워크플로와, 계산이 확장 기능과 상호 작용하는 방식을 이해해야 합니다. 소프트웨어에는 작업, 임무 또는 유사한 항목에 대한 개념이 있을 수 있습니다. 이런 지식을 통해 작업을 분할할 수 있는 무언가를 설계합니다. 모델에 사용자 지정 단계가 있다면 처리를 위해 입력을 더 작은 그룹으로 분할할 수 있게 설계합니다. 종종 이를 분산-수집 패턴이라고 합니다.

- 분산: 자연스러운 흐름에 따라 입력을 분할하고 별도 작업의 실행을 허용합니다.
- 수집: 작업이 완료되면 해당 출력을 수집합니다.

분할 시 진행에 앞서 프로세스를 동기화해야 하는 위치도 인지합니다. 분할을 수행하는 공통 위치가 몇 가지 있습니다. 중첩된 통계 실행의 경우 백 가지 시나리오의 내부 루프를 실행하는 굴곡 지점 세트와 천 개의 외부 루프가 있을 수 있습니다. 각 외부 루프는 동시에 실행될 수 있습니다. 굴곡 지점에서 멈춘 다음, 내부 루프를 동시에 실행하고 정보를 다시 가져와 외부 루프에 대한 데이터를 조정하고, 다시 이어갑니다. 다음 그림은 이 워크플로를 보여 줍니다. 충분한 계산이 있으면 100,000개 코어에서 100,000개 내부 루프를 실행할 수 있으므로 처리 시간을 다음 시간의 합계까지 단축할 수 있습니다.

![](./assets/insurance-risk-assets/timing.png)

배포는 수행 방법에 따라 다소 증대할 수 있습니다. 올바른 매개 변수에서는 작은 작업 구조화처럼 간단할 것이고 올바른 위치에 100K 파일을 복사하는 것처럼 복잡할 수도 있습니다. HD Insight의 Apache Spark, Azure Databricks 또는 자체 배포를 사용하여 결과 집계를 배포할 수 있는 경우 처리 결과는 더 단축될 수 있습니다. 예를 들어, 평균 계산은 지금까지 나타난 항목 수와 합계를 기억하기만 하면 되는 간단한 사안입니다. 다른 계산은 수천 개 코어의 단일 머신에서 더 잘 작동할 수 있습니다. 이 경우 Azure에서 GPU 지원 머신을 사용할 수 있습니다.

대부분의 보험 계리 팀은 모델을 Azure로 이동하는 것부터 시작합니다. 그런 다음, 프로세스의 다양한 단계에서 시간 데이터를 수집합니다. 다음으로 각 단계의 클록 시간을 가장 긴 소요 시간에서 가장 짧은 소요 시간으로 정렬합니다. 어떤 항목은 수천 코어 시간을 사용하면서도 소요 시간은 20분에 지나지 않을 수 있으므로 총 실행 시간은 살피지 않습니다. 가장 길게 실행되는 작업의 각 단계에 대해 보험 계리 개발자들은 소요 시간을 줄이면서 올바른 결과를 가져오는 방법을 모색합니다. 이 프로세스는 정기적으로 반복됩니다. 일부 보험 계리 팀에서는 목표 실행 시간을 정할 수 있습니다. 8시간 미만으로 실행되는 것을 목표로 하는 야간 방어 분석을 가정해 보겠습니다. 시간이 8.25시간을 넘어선 즉시 일부 보험 계리 팀을 전환하여 분석에서 가장 길게 실행되는 부분의 시간을 개선합니다. 이 시간을 7.5시간으로 되돌리면 다시 개발로 전환됩니다. 복귀 및 최적화를 위한 추론은 보험 계리마다 다릅니다.

이를 모두 실행하기 위해 여러 가지 옵션이 있습니다. 대부분의 보험 계리 소프트웨어는 계산 그리드에서 작동합니다. 온-프레미스 및 Azure에서 작동하는 그리드는 [HPC Pack](https://docs.microsoft.com/azure/virtual-machines/windows/hpcpack-cluster-options?WT.mc_id=riskmodel-docs-scseely), 타사 패키지 또는 사용자 지정 항목을 사용합니다. Azure에 최적화된 그리드는 [Virtual Machine Scale Sets](https://docs.microsoft.com/azure/virtual-machine-scale-sets/?WT.mc_id=riskmodel-docs-scseely), [Batch](https://docs.microsoft.com/azure/batch/?WT.mc_id=riskmodel-docs-scseely) 또는 사용자 지정 항목을 사용합니다. Scale Sets 또는 Batch를 사용할 경우 우선 순위가 낮은 VM([Scale Sets](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-use-low-priority?WT.mc_id=riskmodel-docs-scseely) 낮은 우선 순위 문서, [Batch](https://docs.microsoft.com/azure/batch/batch-low-pri-vms?WT.mc_id=riskmodel-docs-scseely) 낮은 우선 순위 문서)에 대한 지원을 살펴야 합니다. 우선 순위가 낮은 VM은 정상가의 일부 금액으로 대여할 수 있는 하드웨어에서 실행되는 VM입니다. 우선 순위가 낮은 VM은 용량에서 필요할 때 선점될 수 있기 때문에 더 낮은 가격이 가능합니다. 시간 예산에 유연성이 있다면 우선 순위가 낮은 VM은 모델링 실행 가격을 낮출 수 있는 좋은 방법이 됩니다.

다양한 지역에서 실행되는 등, 실행 및 배포를 여러 머신에서 조정해야 한다면 CycleCloud를 활용할 수 있습니다. CycleCloud에는 추가 비용이 발생하지 않습니다. 필요한 경우 데이터 이동을 조정합니다. 여기에는 머신 할당, 모니터링, 종료가 포함됩니다. 우선 순위가 낮은 머신을 처리하여 비용을 억제할 수도 있습니다. 필요한 머신의 조합을 설명하는 것도 가능합니다. 예를 들어, 고급 머신이 필요하나 2개 이상의 코어가 있는 어느 버전에서나 잘 실행될 수 있습니다. 주기는 이러한 머신 유형에서 코어를 할당할 수 있습니다.

## <a name="reporting-on-the-results"></a>결과에 대한 보고

보험 계리 패키지가 실행되어 결과를 생성한 후에는 여러 규제 당국에 제공할 보고서가 필요합니다. 규제 당국이나 감사 기관에서 요구하지 않는 인사이트를 생성하는 분석을 수행하기 위해 수많은 새 데이터가 있을 수도 있습니다. 최고 고객의 프로필을 이해하고자 할 수 있습니다. 인사이트를 사용하면 마케팅 및 영업 부서가 더 신속하게 저비용 고객을 파악할 수 있게 저비용 고객의 양태를 알려줄 수 있습니다. 마찬가지로, 보험 보유의 수혜가 가장 큰 그룹을 밝히는 데도 데이터를 사용할 수 있습니다. 예를 들어, 건강 문제의 조기 진단과 관련하여 연간 건강 검사를 활용하는 사람들을 파악할 수 있습니다. 이렇게 하면 보험 회사의 시간과 비용이 절감됩니다. 이 데이터를 활용하여 고객 베이스의 행동을 촉진할 수 있습니다.

이를 위해 여러 데이터 과학 도구와 여러 데이터 시각화에 액세스하고자 할 수 있습니다. 수행하려는 조사 규모에 따라 Azure Marketplace에서 프로비전되는 [데이터 과학 VM](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview?WT.mc_id=riskmodel-docs-scseely)에서 출발할 수 있습니다. 이 VM에는 Windows와 Linux 버전이 모두 있습니다. 설치하고 나면 Microsoft R Open, Microsoft ML Server, Anaconda, Jupyter 및 기타 도구를 사용할 수 있습니다. 약간의 R 또는 Python 작업을 통해 데이터를 시각화하고 동료들과 인사이트를 공유합니다.

상세 분석이 필요한 경우 Spark, Hadoop 같은 Apache 데이터 과학 도구와 기타 HDInsight 또는 Databricks를 통한 도구를 사용할 수 있습니다. 분석을 정기적으로 실행해야 하며 워크플로를 자동화하려는 경우 이런 도구가 더욱 필요합니다. 대형 데이터 세트의 라이브 분석에도 유용합니다.

관심 있는 항목을 찾은 후에는 결과를 제시할 필요가 있습니다. 많은 보험 계리인들은 샘플 결과를 취해 Excel에 가져와 차트, 그래프 및 기타 시각화 요소를 만드는 것에서 시작합니다. 데이터 상세 분석을 위한 훌륭한 인터페이스를 갖춘 [Power BI](https://docs.microsoft.com/power-bi/?WT.mc_id=riskmodel-docs-scseely)를 살펴보겠습니다. Power BI는 훌륭한 시각화와 원본 데이터 표시가 가능하며, [정렬되어 주석이 적용된 책갈피](https://docs.microsoft.com/power-bi/desktop-bookmarks?WT.mc_id=riskmodel-docs-scseely)추가를 통해 판독자들에게 데이터를 설명할 수 있습니다.

## <a name="data-retention"></a>데이터 보존

시스템에 가져온 대부분의 데이터는 향후 감사를 위해 보존되어야 합니다. 데이터 보존 요구 사항은 보통 7~10년이나 요구 사항에 따라 다릅니다. 최소 보존에는 다음이 포함됩니다.

- 모델에 대한 원래 입력의 스냅숏 자산, 부채, 가정, ESG 및 기타 입력이 포함됩니다.
- 최종 출력의 스냅숏. 규제 당국에 제시하는 보고서 만들기에 사용되는 데이터가 포함됩니다.
- 다른 중요한 중간 결과입니다. 감사자는 모델에서 어떤 결과가 나온 이유에 대해 질문할 것입니다. 모델이 특정 선택을 내린 이유나 특정 수치에 대한 증거를 보존해야 합니다. 많은 보험 회사는 원래 입력에서 최종 출력을 생성하는 데 사용한 바이너리를 보관합니다. 그런 다음, 질문을 받으면 중간 결과의 새 사본을 얻기 위해 모델을 다시 실행합니다. 출력이 일치하면 중간 파일에 필요한 설명이 포함될 것입니다.

모델 실행 중에 보험 계리인은 실행에서 요청 로드를 처리할 수 있는 데이터 전달 메커니즘을 사용합니다. 실행이 완료되어 데이터가 더 이상 필요하지 않으면 일부 데이터를 유지합니다. 최소한 보험 회사는 재생산 가능 요구 사항에 대해 런타임 구성과 입력을 유지해야 합니다. 데이터베이스는 Azure Blob Storage에서 백업으로 유지되며 서버는 종료됩니다. 고속 스토리지의 데이터도 더 저렴한 Azure Blob Storage로 이동됩니다. Blob Storage에서는 핫, 쿨 또는 보관 등 각 Blob에 사용되는 데이터 계층을 선택할 수 있습니다. 핫 스토리지는 자주 액세스하는 파일에 적합합니다. 쿨 스토리지는 자주 발생하지 않는 데이터 액세스에 최적화되었습니다. 보관 스토리지는 감사 가능한 파일을 보관하는 데 적합하지만 대기 시간 비용에서 절약이 가능합니다. 보관 계층 데이터 대기 시간은 시간 단위로 측정됩니다. 서로 다른 스토리지 계층을 제대로 이해하려면 [Azure Blob storage: 핫, 쿨 및 보관 스토리지 계층](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers?WT.mc_id=riskmodel-docs-scseely)을 읽어 보세요. 수명 주기 관리를 통해 만들기에서 삭제까지의 데이터를 관리할 수 있습니다. Blob에 대한 URI는 고정 상태를 유지하나 Blob 저장 위치는 점점 더 저렴해집니다. 이런 특징 때문에 많은 Azure Storage 사용자들의 비용과 고민을 크게 줄일 수 있습니다. [Azure Blob Storage 수명 주기 관리](https://docs.microsoft.com/azure/storage/common/storage-lifecycle-managment-concepts?WT.mc_id=riskmodel-docs-scseely)에서 장단점을 확인할 수 있습니다. 자동으로 파일을 삭제할 수 있다는 점이 매력적입니다. 즉 파일 자체가 자동으로 제거될 수 있기 때문에 범주 외의 파일을 참조하여 우발적으로 감사를 확대하지 않게 됩니다.

## <a name="next-steps"></a>다음 단계

실행하는 회계 계리 시스템에 온-프레미스 그리드 구현이 있다면 해당 그리드 구현이 Azure에서도 실행될 수 있습니다. 일부 공급 업체의 경우 하이퍼스케일로 실행되는 Azure 구현을 전문으로 하고 있습니다. Azure로의 전환 과정에서 내부 도구도 함께 이동합니다. 어느 지역에서나 보험 계리인은 랩톱이나 대규모 환경 등, 데이터 과학 기술이 어디에서나 잘 작동하는 것을 알 수 있었습니다. 기존에 팀이 하고 있는 것을 살펴봅니다. 딥 러닝을 사용하나 한 GPU에서 실행되는 데 몇 시간에서 며칠까지 걸릴 수 있습니다. 하이엔드 GPU 4개가 있는 머신에서 같은 워크로드를 실행하고 런타임을 살펴보면 기존 항목의 속도 향상이 두드러진 것을 알 수 있습니다.

향상이 되면 모델링 데이터 공급을 위한 일부 데이터 동기화도 구축해야 합니다. 모델 실행은 데이터가 준비되어야 시작할 수 있습니다. 여기에는 변경된 데이터만 보내는 작업 추가가 포함됩니다. 실제 방법은 데이터 크기에 따라서도 달라집니다. 일부 MB만 업데이트하는 것은 큰 결과를 내지 않지만 기가바이트 업로드 수를 줄이면 속도가 크게 향상됩니다.

### <a name="tutorials"></a>자습서

- R 개발자: [Azure Batch를 사용하여 병렬 R 시뮬레이션 실행](https://docs.microsoft.com/azure/batch/tutorial-r-doazureparallel?WT.mc_id=riskmodel-docs-scseely)
- Azure Function을 사용하여 스토리지와 상호 작용하는 방법을 보여 주는 자습서: [Azure Functions를 사용하여 Blob Storage에 이미지 업로드](https://docs.microsoft.com/azure/functions/tutorial-static-website-serverless-api-with-database?tutorial-step=2&WT.mc_id=riskmodel-docs-scseely)
- Databricks를 사용하는 ETL: [Azure Databricks를 사용하여 데이터 추출, 변환 및 로드](https://docs.microsoft.com/azure/azure-databricks/databricks-extract-load-sql-data-warehouse?WT.mc_id=riskmodel-docs-scseely)
- HDInsight를 사용하는 ETL: [Azure HDInsight에서 Apache Hive를 사용하여 데이터 추출, 변환 및 로드](https://docs.microsoft.com/azure/hdinsight/hdinsight-analyze-flight-delay-data-linux?toc=%2Fen-us%2Fazure%2Fhdinsight%2Fhadoop%2FTOC.json&amp;bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json&WT.mc_id=riskmodel-docs-scseely)
- 데이터 과학 VM 방법(Linux): [https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/linux-dsvm-walkthrough](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/linux-dsvm-walkthrough?WT.mc_id=riskmodel-docs-scseely)
- 데이터 과학 VM 방법(Windows): [https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/vm-do-ten-things](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/vm-do-ten-things?WT.mc_id=riskmodel-docs-scseely)
