---
title: 개요 - 그리드 컴퓨팅 위험 분석 Azure Batch, Azure Data Lake
author: dstarr
ms.author: dastarr
ms.date: 11/20/2019
ms.topic: article
ms.service: industry
description: 금융의 위험 그리드 컴퓨팅용 Azure Batch를 구현하는 기술적 측면을 소개합니다.
ms.openlocfilehash: 542fb820870048ac2ec2cb67c2bbf13988588ea1
ms.sourcegitcommit: f030566b177715794d2ad857b150317e72d04d64
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74234669"
---
# <a name="risk-grid-computing-in-banking-solution-guide"></a>금융의 위험 그리드 컴퓨팅 솔루션 가이드

이 문서에서는 Microsoft Azure를 사용하여, 권장된 시스템 및 높은 수준의 아키텍처를 포함해 금융의 위험 그리드 컴퓨팅을 지원하고 개선하는 기술적 개요를 제공합니다.

이 문서는 솔루션 설계자는 물론, 경우에 따라서는 위험 컴퓨팅용으로 제안된 솔루션에 대한 심층 분석을 원하는 기술 관련 의사 결정자를 위해 제작되었습니다.

## <a name="introduction"></a>소개

재무 위험 분석 모델은 일반적으로 과도한 계산 부하가 컴퓨팅 성능, 데이터 액세스 및 분석에 대한 높은 수요를 생성하는 Batch 작업으로 처리됩니다. 위험 그리드 컴퓨팅 계산에 대한 수요는 시간에 따라 증가하고, 이에 따라 계산 리소스에 대한 필요도 증가하는 경우가 많습니다.

광범위한 사용 가능한 Azure 제품 및 서비스는 대부분의 문제에 둘 이상의 솔루션이 있을 수 있음을 의미합니다. 이 문서에서는 [Microsoft Azure Batch](/azure/batch/batch-dotnet-get-started?WT.mc_id=gridbanksg-docs-dastarr)를 사용하여 금융의 위험 그리드 컴퓨팅 솔루션에 가장 효과적인 것으로 추정된 기술, 패턴 및 사례에 대한 개요를 제공합니다.

Azure Batch는 인프라와 일반적으로 위험 그리드 컴퓨팅 모델에 사용되는 일괄 처리의 다양한 단계 모두에 비용 효과적이고 안전한 솔루션을 제공하는 무료 서비스입니다. Azure Batch는 하이브리드 네트워크를 사용하거나, 전체 Batch 프로세스를 Azure로 전환시켜 현재의 온-프레미스 컴퓨팅 리소스 투자를 늘리거나 확장하거나, 심지어 대체할 수 있습니다.  데이터는 클라우드에서 상하 이동하거나, 온-프레미스 리소스가 느리게 실행될 경우 클라우드 버스트 모델의 컴퓨팅 노드에서 다른 데이터가 처리되는 동안 온-프레미스에 머물 수 있습니다.

## <a name="anatomy-of-a-batch-run"></a>Batch 실행의 분석

일반적으로 Batch 실행에는 두 개 이상의 애플리케이션이 관여합니다. 둘 중 하나의 애플리케이션은 일반적으로 “헤드 노드”에서 실행되어 작업을 풀로 제출하고 때때로 컴퓨팅 노드를 오케스트레이션합니다. 또한 Azure Portal을 통해 오케스트레이션을 구성할 수도 있습니다. 다른 애플리케이션은 컴퓨팅 노드에서 태스크로 실행됩니다(그림 1 참조).

컴퓨팅 노드 애플리케이션은 병렬 처리 위험 모델링 파일의 태스크를 수행합니다. 둘 이상의 애플리케이션을 설치하여 컴퓨팅 노드에서 실행할 수 있습니다.

이러한 애플리케이션은 [Batch API](/azure/batch/batch-apis-tools?WT.mc_id=gridbanksg-docs-dastarr), Azure Portal을 통해 바로 또는 [Batch용 Azure CLI 명령](/azure/batch/cli-samples?WT.mc_id=gridbanksg-docs-dastarr)을 통해 업로드할 수 있습니다.

![Azure Batch 그리드 컴퓨팅](./assets/risk-grid-compute-assets/05-batch-grid-computing.png)

**그림 1:** Azure Batch 그리드 컴퓨팅

Batch 실행은 여러 논리 요소로 구성되어 있습니다. 그림 2에서는 일괄 작업의 논리 모델을 보여줍니다. 풀은 Batch 실행에 관여하는 VM용 컨테이너로서, 컴퓨팅 노드 VM을 프로비전합니다. 또한 풀은 컴퓨팅 노드에 설치된 애플리케이션용 컨테이너이기도 합니다. 작업은 풀 내부에 생성되어 실행됩니다. 태스크는 작업에서 실행됩니다. 태스크는 작업자 애플리케이션의 실행이며 명령줄 지침에 의해 호출됩니다.

작업자 애플리케이션은 생성되면 컴퓨팅 노드에 설치됩니다.

![풀, 작업 및 태스크](./assets/risk-grid-compute-assets/06-pool-job-logical-model.png)

**그림 2:** 논리 Batch 개념 모델

작업이 실행되면 풀은 필요한 작업자 VM을 프로비전하고 작업자 애플리케이션을 설치합니다. 작업은 이러한 컴퓨팅 노드로 태스크를 할당하고, 일반적으로 설치된 애플리케이션 또는 스크립트를 호출하는 명령줄 지침을 실행합니다.
Batch 사용 시 아래 설명된 대로 정형화된 패턴을 따릅니다.

1. Batch 자산을 포함할 리소스 그룹을 만듭니다.
2. 리소스 그룹 내에 Batch 계정을 만듭니다.
3. 연결된 스토리지 계정을 만듭니다.
4. 작업자 VM을 프로비전하는 풀을 만듭니다.
5. 풀에 컴퓨팅 노드 애플리케이션 또는 스크립트를 업로드합니다.
6. 풀의 VM에 태스크를 할당할 작업을 만듭니다.
7. 풀에 작업을 추가합니다.
8. Batch 실행을 시작합니다.
9. 작업은 컴퓨팅 노드에서 실행할 태스크를 대기시킵니다.
10. VM이 사용 가능해지면 컴퓨팅 노드가 태스크를 실행합니다.

이 프로세스의 예시는 그림 3에 나와 있습니다.

![Batch 실행 프로세스](./assets/risk-grid-compute-assets/07-batch-run-process.png)

**그림 3:** 논리 Batch 개념 모델

태스크가 완료되면 사용하지 않는 동안 비용이 발생하지 않도록 컴퓨팅 노드를 제거하는 것이 유용할 수 있습니다. 코드 또는 포털을 통해 삭제하려면 포함 풀을 삭제하세요. 그러면 작업자 VM이 제거됩니다.

Batch 시작 방법에 대해 더 자세한 연습을 하려면 여러 언어로 프로세스 또는 Azure Portal을 안내하는 [5분 빠른 시작](/azure/batch/?WT.mc_id=gridbanksg-docs-dastarr)을 참조하세요.

## <a name="batch-process-scheduling"></a>Batch 프로세스 일정 예약

Azure Batch에는 스케줄러가 기본 제공되므로 포털 또는 API를 통해 각 실행 일정 예약을 정의할 수 있습니다. Batch 작업 스케줄러는 여러 작업을 실행하도록 여러 일정을 정의할 수 있습니다. 각 작업마다 작업 시작 및 종료 시 수행할 작업 등, 자체 속성이 있습니다. 작업 일정은 되풀이 간격 또는 1회 실행으로 설정할 수 있습니다.

많은 은행 그리드 컴퓨팅 시스템에는 이미 자체 일정 예약 서비스가 있습니다. 내 스케줄러를 Azure로 바로 전환할 필요는 없습니다. Azure Batch가 수동으로, 또는 SDK를 통해 호출되어 일정 예약이 온-프레미스에서 발생하고 워크로드를 Azure에서 처리할 수 있으므로 원활하게 진행할 수 있습니다.

Batch 처리는 미리 정해진 일정 또는 요청 시 실행할 수 있지만, 어느 쪽이든 사용하지 않을 때는 컴퓨팅 노드 VM을 활성 상태로 유지할 필요가 없습니다.  수천 개가 아닌 수백 개의 VM 컴퓨팅 노드를 사용할 경우 대기 중인 태스크를 실행할 때 프로비전한 서버를 해제함으로써 상당한 비용 절감을 실현할 수 있습니다.

## <a name="compute-node-applications"></a>컴퓨팅 노드 애플리케이션

컴퓨팅 노드는 태스크가 호출되면 실행할 애플리케이션이 필요합니다. 이러한 애플리케이션은 비즈니스에서 작업자에 설치 시 처리 작업을 수행하기 위해 작성됩니다. 은행 시나리오에 대한 위험 그리드 컴퓨팅에서 이 애플리케이션은 데이터를, 특히 다운스트림 분석 또는 기타 처리에 적합한 형식으로 전환하는 작업에 사용됩니다.

컴퓨팅 노드로 배포를 위해 애플리케이션을 풀로 제공하면 애플리케이션 패키지로 업로드됩니다. 애플리케이션 패키지는 이전에 업로드한 애플리케이션 패키지의 또 다른 버전일 수 있습니다. 둘 이상의 애플리케이션 패키지를 하나의 컴퓨팅 노드에 설치할 수 있습니다. 작업은 작업자 머신으로 로드할 애플리케이션 패키지를 포함합니다.

애플리케이션 패키지 배포는 버전별로 관리할 수도 있습니다. 여러 버전의 애플리케이션 패키지가 풀에 로드되었다면 Batch 실행에 사용할 특정 버전을 지정할 수 있습니다(그림 4 참조). 이는 감사 환경 또는 비즈니스가 이전 실행을 재현하려는 경우 필요할 수 있으며, 작업자 애플리케이션에 버그가 유입된 경우 롤백 목적으로 사용할 수도 있습니다.

![Batch 실행 프로세스](./assets/risk-grid-compute-assets/08-versioning-worker-applications.png)

**그림 4:** 컴퓨팅 노드 태스크 애플리케이션 버전 관리

애플리케이션 패키지는 태스크가 애플리케이션을 실행하는 데 필요한 애플리케이션 이진 파일 및 지원 파일이 포함된 .zip 파일로 풀에 업로드됩니다. 애플리케이션 패키지에는 두 가지 범위가 있습니다. 풀의 범위 또는 태스크의 범위에서 애플리케이션 패키지를 지정할 수 있습니다.

## <a name="pool-application-packages"></a>풀 애플리케이션 패키지

이러한 패키지는 풀의 모든 컴퓨팅 노드에 배포됩니다. 컴퓨팅 노드 VM이 프로비전, 재부팅 또는 이미지로 다시 설치될 때 업데이트된 애플리케이션이 있다면 새로운 사본의 풀 애플리케이션 패키지가 설치됩니다. 하나 이상의 애플리케이션 패키지를 풀로 할당할 수 있습니다. 즉, 컴퓨팅 노드에 모든 패키지가 지정됩니다.

## <a name="task-application-packages"></a>태스크 애플리케이션 패키지

태스크 수준을 대상으로 하는 애플리케이션 패키지는 태스크를 실행하도록 예약된 컴퓨팅 노드에만 설치됩니다. 태스크 애플리케이션 패키지는 둘 이상의 작업이 하나의 풀에서 실행될 때 사용하기 위한 것입니다.

태스크 애플리케이션은 위험 그리드 컴퓨팅 시나리오에서 관련성이 있을 수 있는 풀 수준 작업에서 생성된 데이터 집계 시 유용합니다. 예를 들어 작업 애플리케이션은 위험 계산 워크플로에서 나중에 사용할 데이터를 생성하는 위험 계산 집합을 실행할 수 있습니다.

## <a name="scaling-batch-jobs"></a>일괄 작업 크기 조정

은행은 계산 리소스의 활용률이 적은 주말 또는 야간에 위험 분석 일괄 실행을 수행하는 경우가 많습니다. 일부 작업에는 이 모델이 작용하지만, 규모가 빠르게 커져서 더 많은 작업자 머신을 그리드에 추가하려면 더 많은 자본이 필요할 수 있습니다.

Batch 작업이 너무 오래 걸려 실행할 수 없거나 Batch 실행에서 더 나은 컴퓨팅 성능을 원하는 경우 Azure는 여러 옵션을 제공합니다.

1. 더 많은 컴퓨팅 노드 머신을 할당하여 규모를 확장합니다.
2. 더 강력한 컴퓨팅 노드 머신을 할당하여 성능을 높입니다. Azure 머신은 코어와 메모리, 심지어는 GPU 컴퓨팅 성능에 대한 높은 성능 요구 사항을 충족하도록 프로비전할 수 있습니다.

> 참고: Microsoft HPC 팩(Batch 포함)을 사용하는 모델은 더 복잡하며 이 문서에서는 설명되어 있지 않습니다.

Batch 처리 클러스터에는 두 개의 처리 VM처럼 적거나, 수만 개의 코어가 포함된 수천 개의 VM 컴퓨팅 노드에서 수천 개의 동시 작업이 실행될 수 있습니다. 각 VM은 한번에 하나의 작업을 실행합니다. 로드가 증가하거나 감소하면 구성에 따라 수동 또는 자동으로 풀 하나에 포함되는 VM의 개수를 조정할 수 있습니다.

### <a name="burst-to-cloud"></a>클라우드로 버스트

온-프레미스 그리드의 컴퓨팅 리소스가 대규모 분석 작업으로 인해 느리게 실행되는 경우 “클라우드로 버스트”는 Azure에서 컴퓨팅 노드를 더 추가하여 이러한 리소스를 보강할 수 있는 방법을 제공합니다. 클라우드로 버스트는 온-프레미스 리소스에 대한 수요가 높으면 프라이빗 클라우드 또는 인프라가 자체 워크로드를 분산시키는 모델입니다.

이러한 컴퓨팅 노드는 Azure의 IaaS 플랫폼에서 프로비전될 Linux 또는 Windows 가상 머신으로 사전 구성할 수 있습니다. 또한 서버를 프로비전하고 Tibco Gridserver 및 IBM Symphony와 같은 기존 투자로 실행하도록 자동 구성할 수 있습니다.

### <a name="automatic-scaling-formulas"></a>자동 크기 조정 수식

이러한 탄력성은 [자동 크기 조정 공식](/azure/batch/batch-automatic-scaling?WT.mc_id=gridbanksg-docs-dastarr)을 사용하거나 Azure Portal에서 구성할 수 있습니다. 자동 크기 조정 수식은 Batch 처리를 세부 조정하기 위해 Batch 처리 스케줄러로 업로드된 스크립트입니다. 컴퓨팅 노드 풀에 대한 자동 크기 조정은 자동 크기 조정 수식과 노드를 연결하여 실행됩니다.

다음 예는 하나의 VM으로 시작한 후 필요에 따라 VM을 최대 50개까지 확장하도록 자동으로 크기를 조정하는 자동 크기 조정 수식입니다. 태스크가 완료되면 VM은 하나씩 비워지고 자동 크기 조정 수식은 풀을 축소합니다.

```Formula
startingNumberOfVMs = 1;
maxNumberofVMs = 50;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicatedNodes=min(maxNumberofVMs, pendingTaskSamples);
```

### <a name="other-scaling-techniques"></a>기타 크기 조정 방법

또한 자동 크기 조정은 Enable-AzureBatchAutoScale PowerShell cmdlet을 통해 사용하도록 설정할 수도 있습니다. Enable-AzureBatchAutoScale cmdlet을 사용하면 지정된 풀을 자동으로 크기 조정할 수 있습니다. 예제는 다음과 같습니다.

1. 첫 번째 명령은 수식을 정의한 다음, `$Formula` 변수에 저장합니다.
2. 두 번째 명령은 `$Formula`의 수식을 사용하여 이름이 `RiskGridPool`인 풀에서 자동 크기 조정을 사용하도록 설정합니다.

```console
C:\> $Formula = ‘startingNumberOfVMs = 1;
maxNumberofVMs = 50;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second?WT.mc_id=gridbanksg-docs-dastarr);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicatedNodes=min(maxNumberofVMs, pendingTaskSamples);’;

C:\> Enable-AzureBatchAutoScale -Id "RiskGridPool" -AutoScaleFormula $Formula -BatchContext $Context
```

자동 크기 조정은 `az batch pool resize` 명령으로 Azure CLI, 그리고 Azure Portal을 사용하여 수행할 수도 있습니다.

## <a name="data-storage-and-retention"></a>데이터 스토리지 및 처리

컴퓨팅 노드에서 데이터가 수집되어 처리되면 결과 출력 데이터를 데이터베이스에 저장하여 추가 처리 및 분석에 사용하거나, 저장하기 전에 수집 시 변환하여 다운스트림 처리에 적절한 형식을 보장할 수 있습니다. Microsoft Azure는 여러 스토리지 옵션을 제공합니다. [사용할 데이터 저장소 기술](/azure/architecture/data-guide/?WT.mc_id=gridbanksg-docs-dastarr)의 선택은 주로 다운스트림 프로세스에서의 분석 및/또는 보고 요구 사항에 따라 크게 좌우됩니다.

하이브리드 네트워크 사용 시 데이터 스토리지 대상은 온-프레미스일 수 있습니다. 하이브리드 네트워크를 통해 Batch를 사용할 경우 컴퓨팅 노드는 Azure 기반 스토리지 위치를 사용하지 않고 온-프레미스 데이터 스토어로 데이터를 다시 쓸 수 있습니다. 또는 작업자는 온-프레미스 파일과 호환되는 어떠한 프로세스를 통해서도 쉽게 액세스할 수 있도록 온-프레미스 머신의 디스크로 탑재할 수 있는 Azure File 스토리지에도 쓸 수 있습니다.

## <a name="monitoring-and-logging"></a>모니터링 및 로깅

이후의 Batch 작업 실행을 최적화하려면 최적화 영역을 식별할 수 있도록 데이터를 기록해야 합니다. 예를 들어 작업자가 CPU 최대 가용량에 가깝게 실행 중인 경우 컴퓨팅 노드에 코어를 추가하면 CPU에 구애받지 않고 작업을 더 빨리 마칠 수 있습니다. Batch 작업에서 실행하는 각 애플리케이션마다 고유한 특징이 있으며 Batch 실행에서 VM에 적용되는 최적화가 다를 수 있습니다. 메모리 집약적 태스크의 경우 다음 실행 시 머신을 다르게 구성하여 더 많은 메모리를 할당할 수 있습니다.

로깅은 컴퓨팅 노드 및 그리드 헤드 애플리케이션 또는 [Batch 진단 로깅](/azure/batch/batch-diagnostics?WT.mc_id=gridbanksg-docs-dastarr)을 사용하여 작업별로 수행할 수 있습니다. Batch 실행의 성능에 대한 로깅 정보는 더 나은 성능과 더 빠른 태스크 완료를 위해 개선할 영역을 식별하는 데 도움이 되도록 구성할 수 있습니다.

### <a name="custom-batch-monitoring-and-logging"></a>사용자 지정 Batch 모니터링 및 로깅

제어 애플리케이션 및 컴퓨팅 노드 애플리케이션은 이 데이터를 생성하고 추가 분석을 위해 저장할 수 있습니다. Batch 작업 최적화에 유용한 것으로 나타난 데이터는 다음과 같습니다.

- 각 태스크의 시작 및 종료 시간
- 각 컴퓨팅 노드의 활성화 및 태스크 실행 시간
- 각 컴퓨팅 노드의 활성화 및 태스크 비실행 시간
- Batch 작업 전체 실행 시간

### <a name="batch-diagnostic-logging"></a>Batch 진단 로깅

계층 데이터를 생성하기 위해 컨트롤러와 컴퓨팅 노드 애플리케이션 사용에 대한 대안이 있습니다. [Batch 진단 로깅](/azure/batch/batch-diagnostics?WT.mc_id=gridbanksg-docs-dastarr)은 많은 실행 데이터를 캡처할 수 있습니다. Batch 진단 로깅은 기본적으로 사용 설정되어 있지 않으며 Batch 계정에 대해 사용 설정해야 합니다.

Batch 진단 로깅은 문제 해결을 지원하고 Batch 실행을 최적화하는 상당한 양의 데이터를 제공합니다. 작업 및 태스크, 코어 수, 총 노드 수 및 많은 기타 메트릭에 대한 시작 및 종료 시간입니다.

Batch 로깅은 생성된 풀 생성, 작업 실행, 태스크 실행 등, Batch 실행으로 생성된 이벤트를 저장하고 내보낸 로그를 위한 스토리지 대상이 필요합니다. Azure Storage 계정에 진단 로그를 저장하는 것 외에도 [Azure Event Hub](/azure/event-hubs/event-hubs-what-is-event-hubs?WT.mc_id=gridbanksg-docs-dastarr)에 Batch 서비스 로그 이벤트를 스트림하고 [Azure Log Analytics](/azure/log-analytics/log-analytics-overview?WT.mc_id=gridbanksg-docs-dastarr)로 보낼 수 있습니다.

이러한 데이터를 사용하여 코어 계산 및 헤드 노드 애플리케이션을 최적화할 수 있습니다. 이를 통해 작업자 VM이 더 이상 필요하지 않을 때 Batch 실행이 완료될 때까지 기다리지 않고 더 빨리 해제하는 등, 비용을 절감할 수 있습니다.

### <a name="batch-management-tools"></a>Batch 관리 도구

Azure Portal은 작업이 실행 중일 때 Batch에 대한 정보는 물론, 할당량 사용 현황을 보여주는 Batch 모니터링 대시보드를 제공합니다. 많은 Batch 작업 애플리케이션에 충분합니다.

Azure Portal에서 사용 가능한 Batch 관리 및 시각화 도구뿐만 아니라, Batch를 관리용 무료 오픈 소스 도구인 [BatchLabs](https://github.com/Azure/BatchLabs)가 있습니다. BatchLabs는 Azure Batch 애플리케이션을 만들고, 디버그하고, 모니터링할 수 있도록 하는 무료의 풍부한 기능을 가진 독립 실행형 클라이언트 도구입니다. Mac, Linux 또는 Windows의 경우 설치 패키지를 다운로드합니다.

## <a name="network-models"></a>네트워크 모델

위험 분석의 경우 수천 개가 아니라면 수백 개의 문서를 위험 그리드 컴퓨팅 프로세스에 수집해야 하는 경우가 많습니다. 이러한 파일은 대개 파일 저장소, 네트워크 공유 또는 기타 레포지토리에 온-프레미스로 위치하고 있습니다. Azure 기반 VM을 사용하여 이러한 파일에 액세스하고 처리하는 경우 온-프레미스 네트워크가 Azure 네트워크에 원활하게 연결되는 것이 유용합니다. 그래야 파일 액세스가 간단하고 빠릅니다. 이러한 접근 방식은 컴퓨팅 노드에서 처리를 수행하는 코드를 변경할 필요가 없다는 것을 의미할 수도 있습니다.

Azure는 현재 온-프레미스 시스템을 Azure, [Microsoft Azure ExpressRoute](/azure/expressroute/expressroute-introduction?WT.mc_id=gridbanksg-docs-dastarr) 및 [VPN Gateway](/azure/vpn-gateway/?WT.mc_id=gridbanksg-docs-dastarr)에 안전하고 안정적으로 연결하기 위한 두 가지 모델을 제공합니다. 구현, 성능, 비용 및 [기타 특성](/azure/networking/networking-overview?WT.mc_id=gridbanksg-docs-dastarr)에서 차이가 있긴 하지만 둘 다 신뢰할 수 있는 연결 성능을 제공합니다.

또는 리스크 그리드 컴퓨팅 헤드 노드는 온-프레미스에서 활성화되고 .NET 및 기타 언어의 REST API 또는 SDK를 통해 Batch 작업을 수행할 수 있습니다.

하이브리드 네트워크 솔루션 없이 Azure와 온-프레미스 리소스 간 차이를 보완해 줄 다른 방법이 있습니다. 자세한 내용은 아래에 나와 있습니다.

### <a name="expressroute"></a>ExpressRoute

ExpressRoute는 현재 인터넷 서비스 공급자(ISP?WT.mc_id=gridbanksg-docs-dastarr) 등, 연결 파트너가 활성화한 프라이빗 연결을 통해 Azure에 온-프레미스 또는 데이터 센터 네트워크를 연결합니다. 이를 통해 두 네트워크는 동일한 네트워크 인터페이스처럼 서로 볼 수 있어 네트워크 간에 원활한 액세스를 제공합니다. 기존 온-프레미스 시스템을 Azure 네트워크와 통합하려는 경우 네트워크 통합은 중요하며, ExpressRoute는 가능한 가장 빠른 연결 속도를 제공합니다.

Azure ExpressRoute에 대한 추가 가격 정보는 [여기를 참조](https://azure.microsoft.com/pricing/details/expressroute/?WT.mc_id=gridbanksg-docs-dastarr)하세요.

### <a name="vpn-gateway"></a>VPN Gateway

VPN Gateway는 네트워크를 Azure에 연결하는 또 다른 방법입니다. 이 모델의 단점은 인터넷을 통한 트래픽 흐름이라는 것입니다. 그결과 연결 복원력은 떨어지고 네트워크 속도는 ExpressRoute의 속도에 도달할 수 없지만, 데이터 파일을 읽는 것이 일반적으로 빠른 작업이므로 위험 그리드 컴퓨팅 시나리오에 대한 장벽이 될 수는 없습니다.

VPN Gateway에 대한 추가 가격 정보는 [여기를 참조](https://azure.microsoft.com/pricing/details/expressroute/?WT.mc_id=gridbanksg-docs-dastarr)하세요.

### <a name="choices-for-connectivity-details"></a>연결 세부 정보 선택

기본적으로 Azure로 네트워크를 확장할 수 있는 두 개 모델이 있습니다(그림 5 참조).

1. 가상 게이트웨이 - 사이트 간
2. ExpressRoute – Exchange 또는 ISP 공급자

![사이트 간 및 ExpressRoute](./assets/risk-grid-compute-assets/10-s2s-expressroute.png)

**그림 5:** 사이트 간 및 ExpressRoute

#### <a name="virtual-gateway-site-to-site-integration"></a>가상 게이트웨이 사이트 간 통합

[사이트 간 VPN Gateway](/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal?WT.mc_id=gridbanksg-docs-dastarr)는 온-프레미스 네트워크를 Azure VNet에 연결합니다. 이 방법은 리소스, 서버 및 아티팩트에 대한 양방향 액세스를 통해 기본적으로 두 네트워크의 부분으로 만들어 네트워크 간 차이를 보완합니다. 이를 통해 위험 그리드 컴퓨팅 일괄 작업을 실행하는 Azure 작업자 VM에서 데이터 파일에 바로 액세스할 수 있습니다.

#### <a name="expressroute-integration"></a>ExpressRoute 통합

Azure 파트너 네트워크 공급자가 활성화한 ExpressRoute 연결은 사이트 간 연결과 같은 이점을 실현하지만, 속도오 안정성은 더 높습니다.

[ExpressRoute 연결 모델]에 대해 자세히 알아보세요(/azure/expressroute/expressroute-connectivity-models?WT.mc_id=gridbanksg-docs-dastarr).

### <a name="batch-processing-without-an-azure-hybrid-network"></a>Azure 하이브리드 네트워크 없이 Batch 처리

또 다른 Batch 시나리오는 Azure 기반 컴퓨팅 머신에서 나중에 처리할 수 있도록 Azure Storage로 모든 데이터 파일을 업로드하는 것입니다. File Storage 및 Blob Storage는 위험 그리드 컴퓨팅 데이터 저장을 위한 유력한 후보입니다.

이 시나리오에서 작업 컨트롤러와 모든 컴퓨팅 노드는 Azure에 상주합니다(그림 6 참조). 처리된 데이터의 유력한 대상은 Azure Machine Learning 솔루션 또는 다른 시스템에서의 추가 처리에 대비하여 Azure 데이터 저장소입니다. 이러한 추가 처리는 이 문서의 범위를 벗어난 것입니다.

![사이트 간 및 ExpressRoute](./assets/risk-grid-compute-assets/09-batch-upload-process.png)

**그림 6:** 실행 수명 주기로 일괄 업로드

### <a name="hybrid-network-connectivity-resources"></a>하이브리드 네트워크 연결 리소스

상황에 따라 다양한 구성을 적용할 수 있습니다. 네트워크를 Azure에 연결하는 방법과 관련된 의사 결정 및 아키텍처 지침을 보려면 패턴 및 사례 그룹별로 _[Azure에 온-프레미스 네트워크 연결](/azure/architecture/reference-architectures/hybrid-networking/?WT.mc_id=gridbanksg-docs-dastarr)_ 문서를 참조하세요.

- VPN Gateway 구성 대안은 [이 문서를 참조](/azure/vpn-gateway/vpn-gateway-about-vpngateways?WT.mc_id=gridbanksg-docs-dastarr)하세요.
- [ExpressRoute 연결 모델](/azure/expressroute/expressroute-connectivity-models?WT.mc_id=gridbanksg-docs-dastarr)에 대해 알아봅니다.
- [ExpressRoute 가격](https://azure.microsoft.com/pricing/details/expressroute/?WT.mc_id=gridbanksg-docs-dastarr)을 계산합니다.
- [VPN Gateway 가격](https://azure.microsoft.com/pricing/details/vpn-gateway/?WT.mc_id=gridbanksg-docs-dastarr)을 계산합니다.

## <a name="security-considerations"></a>보안 고려 사항

Azure [Virtual Network(VNet)](/azure/virtual-network/virtual-networks-overview?WT.mc_id=gridbanksg-docs-dastarr)를 생성하면 풀의 컴퓨팅 노드가 내부에 생성됩니다. 이로써 Batch 실행에 추가적인 수준의 격리를 제공하고 [AAD(Azure Active Directory)](/azure/active-directory/active-directory-whatis?WT.mc_id=gridbanksg-docs-dastarr)를 사용한 인증이 가능합니다. 자세한 내용은 [풀 네트워크 구성](/azure/batch/batch-api-basics#pool-network-configuration?WT.mc_id=gridbanksg-docs-dastarr)을 참조하세요.

AAD를 사용하여 Batch 애플리케이션을 인증하는 두 가지 방법이 있습니다.

1. **통합 인증**. AAD 계정을 사용하는 일괄 처리 애플리케이션은 계정을 사용하여 데이터 저장소 및 기타 리소스에 대한 리소스를 획득할 수 있습니다.

2. **서비스 주체**. AAD 서비스 주체는 사용자 및 애플리케이션에 대한 액세스 정책 및 사용 권한을 정의합니다. 서비스 주체는 해당 애플리케이션에 연결된 비밀 키를 사용하여 사용자에 대한 인증을 제공합니다. 따라서 비밀 키로 무인 애플리케이션을 인증할 수 있습니다. 서비스 주체는 런타임에 리소스에 액세스할 때 애플리케이션을 나타내기 위해 애플리케이션에 대한 정책과 권한을 정의합니다. [여기를 참조하세요](/azure/active-directory/develop/active-directory-application-objects?WT.mc_id=gridbanksg-docs-dastarr).

AAD를 사용한 일괄 처리에서의 보안에 대한 자세한 내용은 [이 문서를 참조](/azure/batch/batch-aad-auth?WT.mc_id=gridbanksg-docs-dastarr)하세요.

또한 Batch 서비스는 공유 키를 사용해서 인증할 수도 있습니다. 인증 서비스에서는 두 개의 헤더 값을 HTTP 요청, 데이터 및 권한 부여에 추가해야 합니다. 공유 키 인증에 대한 자세한 내용은 [여기를 참조](/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service?WT.mc_id=gridbanksg-docs-dastarr)하세요.

## <a name="cost-considerations"></a>비용 고려 사항

Azure Batch 사용에 대한 요금은 없습니다. 가상 머신 가동 시간, 스토리지 및 네트워킹과 같은 기본 사용 리소스에 대해서만 비용을 지불합니다. 그러나 컴퓨팅 노드 VM은 유휴 상태가 되어도 비용을 지불하므로 더 이상 필요가 없을 때는 머신의 프로비전을 해제하는 것이 좋습니다. 종종 VM이 포함된 풀을 삭제하여 수행하기도 합니다.
>
풀을 만들 때 각각에 대해 원하는 컴퓨팅 노드 유형 및 개수를 지정할 수 있습니다. 컴퓨팅 노드의 두 가지 유형은 다음과 같습니다.
>
>**전용 컴퓨팅 노드**는 사용자 워크로드용으로 예약되어 있습니다. 우선 순위가 낮은 노드보다 비용은 많이 들지만 절대로 선취되지 않습니다.
>
>**우선 순위가 낮은 컴퓨팅 노드**는 Azure의 나머지 용량을 활용하여 Batch 워크로드를 실행합니다. 우선 순위가 낮은 노드는 전용 노드보다 시간당 비용이 더 적게 들며, 많은 컴퓨팅 능력이 필요한 워크로드를 사용할 수 있습니다. 자세한 내용은 [Batch에서 우선 순위가 낮은 VM 사용](/azure/batch/batch-low-pri-vms?WT.mc_id=gridbanksg-docs-dastarr)을 참조하세요.
>
>전용 및 순위가 낮은 노드가 같은 풀에 있을 수 있습니다.
>
>우선 순위가 낮은 컴퓨팅 노드 및 전용 컴퓨팅 노드에 대한 가격 정보는 [Batch 가격 책정](https://azure.microsoft.com/pricing/details/batch/?WT.mc_id=gridbanksg-docs-dastarr)을 참조하세요.

Batch 진단 로깅 서비스를 사용하는 경우 Azure 스토리지로 내보낸 데이터는 비용이 발생합니다. 다른 데이터와 같은 스토리지 데이터이며, 가격은 보유한 진단 데이터의 양에 따라 결정됩니다.

## <a name="getting-started"></a>시작

위험 그리드 컴퓨팅용 Batch 컴퓨팅과 같은 복잡한 영역을 시작할 수 있는 많은 곳이 있지만, 여기에서는 Batch 기술을 더 잘 이해할 수 있도록 몇 가지 논리적 시작점을 소개합니다.

코딩 및 Azure Portal 예를 사용하여 Batch를 [시작하는 것이 좋습니다](/azure/batch/?WT.mc_id=gridbanksg-docs-dastarr). 또한 Azure Batch 샘플 애플리케이션도 [GitHub](https://github.com/Azure/azure-batch-samples)에서 자유롭게 사용할 수 있습니다.

다음은 Batch 컴퓨팅 작업을 만들고 실행하기 위해 간단한 애플리케이션을 빌드하는 데 도움이 되는 몇 가지 빠른 자습서입니다. 애플리케이션 빌드 옵션은 다음과 같습니다.

- [Batch .NET API](/azure/batch/batch-dotnet-get-started?WT.mc_id=gridbanksg-docs-dastarr)
- [Python용 Batch SDK](/azure/batch/batch-python-tutorial?WT.mc_id=gridbanksg-docs-dastarr)
- [Node.js용 Batch SDK](/azure/batch/batch-nodejs-get-started?WT.mc_id=gridbanksg-docs-dastarr)
- [PowerShell 지원 Batch 관리](/azure/batch/batch-powershell-cmdlets-get-started?WT.mc_id=gridbanksg-docs-dastarr)
- [Azure CLI 지원 Batch 관리](/azure/batch/batch-cli-get-started?WT.mc_id=gridbanksg-docs-dastarr)

개념 증명 이니셔티브를 실행하는 것도 고려하세요. Azure로 데이터 수집에 사용할 접근 방식은 무엇인가요? SDK 또는 REST 인터페이스를 통해 하이브리드 네트워크 또는 업로드 데이터를 사용할 예정인가요? 하이브리드 네트워크를 고려하고 있다면 파일럿을 실행하여 설치해 보세요.

Batch 컴퓨팅 작업의 크기를 평가하고 적합한 크기 조정 솔루션을 선택하세요. [자동 크기 조정 수식](/azure/batch/batch-automatic-scaling?WT.mc_id=gridbanksg-docs-dastarr)은 복잡한 일정 예약 시나리오를 실행하는 데 활용하고, 더 단순한 시나리오는 Azure Portal을 사용하여 달성 가능합니다.

## <a name="technologies-presented"></a>제공되는 기술

[Azure Batch](/azure/batch/?WT.mc_id=gridbanksg-docs-dastarr)는 클라우드에서 대규모 병렬 처리 작업을 실행할 수 있는 기능을 제공합니다.

[Azure Active Directory](/azure/active-directory/active-directory-whatis?WT.mc_id=gridbanksg-docs-dastarr)는 Microsoft의 다중 테넌트, 클라우드 기반 디렉터리 및 ID 관리 서비스로, 핵심 디렉터리 서비스, 애플리케이션 액세스 관리 및 ID 보호를 단일 솔루션에 결합합니다.

[자동 크기 조정 수식](/azure/batch/batch-automatic-scaling?WT.mc_id=gridbanksg-docs-dastarr)은 Batch 크기 조정 작업을 세부 조정하기 위해 Batch 처리 스케줄러로 업로드된 스크립트입니다.

[Batch 진단 로깅](/azure/batch/batch-diagnostics?WT.mc_id=gridbanksg-docs-dastarr)은 Batch 실행과 생성된 이벤트로부터 상세 로그를 생성하는 Azure Batch의 기능입니다. 로그는 Azure Storage에 저장됩니다.

[BatchLabs](https://github.com/Azure/BatchLabs)는 Windows, MacOS 및 Linux에서 사용 가능한 Batch 모니터링 및 관리용 독립 실행 애플리케이션입니다.

[ExpressRoute](/azure/expressroute/expressroute-introduction?WT.mc_id=gridbanksg-docs-dastarr)는 온-프레미스 및 Azure 네트워크를 결합하기 위한 속도가 빠르고 안정적인 하이브리드 네트워크 솔루션입니다.

[VPN Gateway](/azure/vpn-gateway/?WT.mc_id=gridbanksg-docs-dastarr)는 온-프레미스 및 Azure 네트워크를 결합하기 위해 인터넷을 사용하는 하이브리드 네트워크 솔루션입니다.

## <a name="conclusion"></a>결론

이 문서는 금융용 위험 그리드 컴퓨팅에 Azure Batch 사용 시 고려 사항 및 기술 솔루션에 대한 개요를 제공합니다. 이 문서에서는 Azure Batch의 정의에서 [네트워킹 옵션](/azure/architecture/reference-architectures/hybrid-networking/?WT.mc_id=gridbanksg-docs-dastarr)은 물론, 비용 고려 사항까지 많은 사실을 다뤘습니다.

위험 그리드 컴퓨팅용으로 Azure Batch를 더 심도 있게 평가하고 싶다면 [이 페이지](/azure/batch/?WT.mc_id=gridbanksg-docs-dastarr)는 시작할 수 있는 좋은 리소스입니다. 위험 그리드 컴퓨팅에 기본적인 병렬 파일 처리를 샘플을 통해 안내하는 자습서를 제공하며, 자습서는 Azure Portal, Azure CLI, .NET 및 Python을 사용하여 제공됩니다.
