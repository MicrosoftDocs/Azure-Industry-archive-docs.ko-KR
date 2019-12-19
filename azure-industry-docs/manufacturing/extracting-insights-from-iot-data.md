---
title: 제조용 IoT에서 인사이트를 추출하는 아키텍처
description: Azure 서비스를 사용하여 IoT 데이터에서 인사이트를 추출합니다.
author: ercenk
ms.author: ercenk
manager: gmarchet
ms.service: industry
ms.topic: article
ms.date: 11/28/2019
ms.openlocfilehash: c08e6bbb1da47084122dae1ed6a9e1cea0b59473
ms.sourcegitcommit: db3bee67c1467884af223a48a895715afba8e08c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2019
ms.locfileid: "75005312"
---
# <a name="extracting-actionable-insights-from-iot-data"></a>IoT 데이터에서 작업 가능한 인사이트 추출

공장 현장에서 기계를 담당한다면 프로세스와 결과를 개선하기 위한 다음 단계가 IoT(사물 인터넷)라는 사실을 이미 인지하고 있을 것입니다. 기계나 공장 현장의 센서가 그 첫 단계입니다. 그 다음이 데이터의 사용으로, 바로 이 문서의 목적입니다. 이 가이드에서는 IoT 데이터 분석에서 조치 가능한 인사이트를 추출하는 데 필요한 구성 요소를 기술적으로 살펴봅니다.

IoT 분석 솔루션은 여러 디바이스의 IoT 데이터를 분석에 더 적합한 형태로 변환하는 것과 관련이 있습니다. 데이터가 분석 가능한 형태가 되면 실제 분석을 수행할 수 있습니다. 분석의 몇 가지 예는 다음과 같습니다.

- 원격 분석 데이터의 간단한 시각화. 예: 시간 경과에 따른 온도를 보여 주는 막대형 차트
- KPI 계산. 예: OEE(전체 디바이스 효과)
- 기계 학습 모델이 제공하는 예측 분석

다시 이 분석은 작업을 알려 주는 인사이트를 제공합니다. 작업은 기계에 간단한 명령을 보내는 것에서 작업 매개 변수를 조정하여 다른 소프트웨어 시스템에서 작업을 수행하고 회사 전체의 운영 향상 프로그램을 구현하는 것에 이르기까지 광범위할 수 있습니다.

아래 그림에서는 공장 기계와 최종 결과 사이에 발생할 수 있는 작업 체인을 보여 줍니다. 즉 그래프로 표시되는 “사용률”의 대시보드 표현과 레이블 “87.5%”입니다.

![공장에서 대시보드까지](assets/extracting-insights-from-iot/factory-to-dashboard.png)

이 그림에서는 기계 사용률이라는 간단한 KPI 계산을 사용했습니다. *기계 사용률*은 기계가 실제 부품을 *생산*한 시간의 백분율입니다. 예를 들어, 한 작업 조가 8시간이고 기계가 이 중 7시간 동안 부품을 생산했다면 이 기계에서 해당 조의 사용률은 87.5%(7/8 x 100)가 됩니다.

## <a name="approach"></a>접근 방식

IoT 애플리케이션에는 비즈니스 또는 프로세스 향상에 도움이 되는 **작업**을 생성하는 데 사용되는 **인사이트**와, 인사이트 생성에 사용되는 데이터 또는 이벤트를 보내는 **사물**(또는 디바이스) 등, 3가지 구성 요소가 있습니다.

제조 공장의 장비(사물)은 작동하면서 다양한 형식의 데이터를 보냅니다. 예를 들어, 밀링 기계는 공급 속도와 온도 데이터를 보냅니다. 이 데이터로 해당 기계의 작동 여부를 평가하는데 이것이 인사이트입니다. 인사이트는 공장 최적화, 즉 작업에 사용됩니다. 데이터를 추출하여 대시보드에 가상화하고 새 인사이트를 추출하여 추가 작업을 수행하는 단계를 살펴보겠습니다.
![사물, 인사이트, 작업](assets/extracting-insights-from-iot/things-insights-actions.png)

Microsoft는 다양한 하위 시스템을 안내하는 IoT 애플리케이션에 대한 상위 수준 참조 아키텍처와, 해당 하위 시스템에 권장되는 접근 방법을 게시했습니다.
IoT 애플리케이션은 다음 하위 시스템으로 구성됩니다.

1. 디바이스 또는 온-프레미스 에지 게이트웨이. 메시지 원본(디바이스)을 클라우드에 안전하게 등록할 수 있는 특정 종류의 디바이스입니다. 에지 게이트웨이도 네이티브 프로토콜의 메시지를 다른 형식(예: JSON)으로 변환할 수 있습니다.
2. 클라우드 게이트웨이 서비스 또는 허브(예: [Azure IoT Hub](/azure/iot-hub/?WT.mc_id=iotinsightssoln-docs-ercenk) 또는 [Azure Event Hubs](/azure/event-hubs/event-hubs-about?WT.mc_id=iotinsightssoln-docs-ercenk)). 데이터를 안전하게 수집하고 디바이스 관리 기능을 제공합니다. 
3. 스트리밍 데이터를 사용하는 스트림 프로세서. 이 프로세서는 비즈니스 프로세스와 상호 작용하고 데이터를 스토리지에 배치할 수도 있습니다.
4. 대시보드 형태의 사용자 인터페이스. IoT 데이터를 시각화하고 디바이스 관리를 용이하게 합니다.

![솔루션 아키텍처.](assets/extracting-insights-from-iot/architecture.png)

이 문서에서는 인사이트 추출 프로세스에 초점을 맞춥니다. 주요 단계는 다음과 같습니다.

1. 데이터에 액세스하고 데이터 스트림으로 처리합니다.
2. 데이터를 처리 및 저장합니다.
3. 데이터를 시각화하거나 표시합니다. 

아래 그림은 데이터 원본에서 변환, 수집 및 처리 및 저장, 표시 및 최종 작업에 이르기까지 데이터의 흐름을 보여 주는 다이어그램입니다.

![데이터 흐름: 원본에서 수집, 표시, 작업까지.](assets/extracting-insights-from-iot/data-flow.png)

## <a name="converting-the-data-to-a-stream"></a>데이터를 스트림으로 변환

IoT 데이터는 시계열 데이터로, 시간 경과에 따라 더 유의미할 수 있는 “사물”의 값입니다. 공장 현장의 장비는 시간에 걸쳐 작동하며 이 시간 동안 이벤트가 발생합니다. 공장 현장의 데이터가 데이터 수집 서비스(예: [Azure IoT Hub](/azure/iot-hub/?WT.mc_id=iotinsightssoln-docs-ercenk))로 보내지지 않는 경우 저장소(예: MES(Manufacturing Execution System) 또는 HTTP 엔드포인트)에서 주기적으로 데이터를 폴링하여 수집 서비스로 보내야 합니다.  
데이터를 스트림으로 변환하려면 보통 다음과 같이 합니다.

1. 데이터 원본에 액세스합니다.
2. 데이터를 변환 및 보강합니다.
3. 스트리밍 데이터를 수집할 수 있는 엔드포인트에 데이터를 게시합니다.

액세스는 데이터 상주 위치에 따라 다르며 변형이 매우 많으므로 여기서는 데이터 원본에 대한 액세스는 다루지 않습니다.

## <a name="transforming-and-enriching-the-data"></a>데이터 변환 및 보강

원시 데이터는 보통 표준화와, 수집 준비를 위한 변환 작업을 거칩니다.  특정 변환은 사용된 분석 유형에 따라 달라집니다.  데이터 변환의 일반적인 예에는 측정 값이 누락되어 입력이 필요하거나, 여러 기계에서의 시간 단위를 합리화해야 하는 시계열 데이터를 들 수 있습니다.  타임스탬프가 적용되고 이름-값 쌍 형식인 데이터 레코드(원본이 포함할 경우)를 갖고자 합니다. 일반적으로 데이터는 계층 구조 형식으로 들어오며 플랫 구조로 변환해야 합니다.

아래 그림은 가변 프로필이 있고 표준화된 열 및 행 형식(블록)으로 변환된 계층 구조적 데이터 구조를 보여 줍니다.

![데이터 셰이프 - 계층에서 플랫으로.](assets/extracting-insights-from-iot/hierarchy-to-flat.png)

일반적으로 데이터는 인터넷에서 액세스할 수 없습니다. 일반적인 패턴은 공장 현장의 데이터를 수집 지점으로 푸시하는 에지 게이트웨이를 사용하는 것입니다. [Azure IoT Edge](/azure/iot-edge?WT.mc_id=iotinsightssoln-docs-ercenk)는 IoT Hub를 기반으로 빌드되는 서비스입니다. IoT Edge 디바이스는 투명, 프로토콜 변환 및 ID 변환 등, 3개 [패턴](/azure/iot-edge/iot-edge-as-gateway?WT.mc_id=iotinsightssoln-docs-ercenk)을 다루는 게이트웨이 역할을 할 수 있습니다.

데이터를 외부에서 사용할 수 있고 인터넷에서 액세스할 수 있다면 몇 가지 Azure 서비스를 사용하여 데이터에 액세스하고 데이터를 변환 및 보강할 수 있습니다. 이러한 옵션에는 다음이 포함됩니다.

- [App Service](/azure/app-service/?WT.mc_id=iotinsightssoln-docs-ercenk), [AKS(Azure Kubernetes Service)](/azure/aks/?WT.mc_id=iotinsightssoln-docs-ercenk), [Container Instances](/azure/container-instances/?WT.mc_id=iotinsightssoln-docs-ercenk) 또는 [Service Fabric](/azure/service-fabric/service-fabric-overview?WT.mc_id=iotinsightssoln-docs-ercenk) 등 다양한 Azure 컴퓨팅 서비스에 배포된 사용자 지정 코드.
- [Azure Logic Apps](/azure/logic-apps/?WT.mc_id=iotinsightssoln-docs-ercenk)
- [Azure Data Factory의 파이프라인 및 작업](/azure/data-factory/copy-activity-overview/?WT.mc_id=iotinsightssoln-docs-ercenk)
- [Azure Functions](/azure/azure-functions/functions-overview)
- [BizTalk Services](https://azure.microsoft.com/services/biztalk-services/)

위의 서비스는 각각 시나리오에 따라 고유의 장단점이 있습니다. 예를 들어 Logic Apps는 [XML 문서 변환](/azure/logic-apps/logic-apps-enterprise-integration-transform?WT.mc_id=iotinsightssoln-docs-ercenk)을 위한 방법을 제공합니다. 그러나 데이터가 과하게 복잡한 XML 문서가 될 수 있으므로 데이터 변환을 위해 대형 XSLT 스크립트를 개발하는 것은 실용적이지 못할 수 있습니다. 이 경우 다른 Azure 서비스에서 여러 마이크로 서비스를 사용하여 하이브리드 솔루션을 개발할 수 있습니다. 예를 들어 Azure Logic Apps로 구현된 마이크로 서비스는 HTTP 엔드포인트를 폴링하고, 원시 결과를 임시 저장하며 다른 마이크로 서비스에게 알릴 수 있습니다. 메시지를 변환하는 다른 마이크로 서비스는 [Azure Functions 호스트](https://github.com/Azure/azure-functions-host)에서 호스팅되는 사용자 지정 코드가 될 수 있습니다.  

![Https는 데이터에 대해 폴링되어 Functions에서 처리됩니다.](assets/extracting-insights-from-iot/poll-logic-process.png)

또는 일련의 작업이 변환을 수행하는 Azure Data Factory가 조정하는 워크플로를 선택할 수 있습니다. 사용 가능한 작업 형식에 대한 자세한 내용은 [Azure Data Factory의 파이프라인 및 작업](/azure/data-factory/concepts-pipelines-activities)을 참조하세요.
메시지는 수신 시점에 타임스탬프가 지정되거나, 측정된 여러 값의 시계열을 재구성할 수 있는 타임스탬프를 포함할 수 있습니다. 따라서 시기에 맞는 최종 응답과 정보 무결성을 보장하기 위해서는 무시할 수 있는 수집 대기 시간과 높은 처리량이 기본입니다. 대기 시간을 최소화하기 위해 타임스탬프를 가능한 공장에 가깝게 정규화합니다.

## <a name="ingesting-the-data-stream"></a>데이터 스트림 수집

데이터를 스트림으로 분석하기 위해 기간을 기준으로 데이터에 대한 쿼리를 수행하여 패턴과 관계를 식별할 수 있습니다. Azure 플랫폼에는 높은 처리량으로 데이터를 수집할 수 있는 여러 서비스가 있습니다.
디바이스 관리, 프로토콜 지원, 확장 가능성, 팀이 선호하는 프로그래밍 모델 등, 프로젝트의 요구 사항에 따라 아래 서비스 중에 선택합니다. 예를 들어, 팀은 경험이 있는 Kafka를 선호하거나, 솔루션에 대한 Kafka 브로커를 필요로 할 수 있습니다. 또는 다른 경우 프로젝트가 [IoT Hub Device Provisioning Service의 TPM 키 증명](/azure/iot-dps/?WT.mc_id=iotinsightssoln-docs-ercenk)을 사용하여 디바이스의 수집 지점 액세스를 보호하기 위해 데이터 수집 시스템이 필요할 수 있습니다.

- [Azure IoT Hub](/azure/iot-hub/?WT.mc_id=iotinsightssoln-docs-ercenk)는 IoT 애플리케이션과 디바이스 간의 양방향 통신 허브입니다. 디바이스를 제어 및 구성할 수 있게 보안 통신, 메시지 전달, 다른 Azure 서비스와의 상호 작용과 관리 기능을 제공하여 완벽한 IoT 솔루션을 구현하는 확장 가능한 서비스입니다.

- [Azure Event Hubs](/azure/event-hubs/event-hubs-about?WT.mc_id=iotinsightssoln-docs-ercenk)는 확장성 높은 수집 전용 서비스로, 매우 빠른 처리 속도로 동시 원본에서 원격 측정 데이터를 수집합니다.
- [HDInsight의 Apache Kafka](/azure/hdinsight/kafka/apache-kafka-introduction?WT.mc_id=iotinsightssoln-docs-ercenk)는 [Apache Kafka](https://kafka.apache.org/)를 호스트하는 관리 서비스입니다. Apache Kafka는 오픈 소스 분산형 스트리밍 플랫폼으로, 메시지 브로커 기능도 제공합니다. 호스트되는 서비스는 Kafka 가동 시간에서 99.9% SLA(Service Level Agreement)를 제공합니다.

## <a name="processing-and-storing-the-data"></a>데이터 처리 및 데이터 저장

IoT 애플리케이션에는 이벤트 중심 시스템이면서 기록 데이터를 기반으로 작동을 유지해야 하는 과제가 있습니다. 들어오는 데이터는 데이터의 추가 형식이므로 급증할 수 있습니다. 보관, 일괄 처리 분석과 ML(기계 학습) 모델 빌드를 위해 데이터를 더 오래 보관해야 합니다. 한 편으로 변칙을 탐지하고, 적용 기간의 패턴을 인식하거나 값이 임계값 초과 또는 미만이면 경고를 트리거하기 위해 거의 실시간에 가까운 분석을 수행하려면 이벤트 스트림이 중요합니다.
Microsoft Azure IoT 참조 아키텍처는 람다 아키텍처를 사용하여 IoT 솔루션에서 클라우드 메시지 및 이벤트에 디바이스에 대한 권장 데이터 흐름을 제시합니다. 람다 아키텍처는 거의 실시간에 가까운 스트리밍 데이터와 보관된 데이터를 모두 분석할 수 있으므로 들어오는 데이터의 처리를 위한 최적의 옵션입니다.

## <a name="lambda-architecture"></a>람다 아키텍처

람다 아키텍처는 데이터 흐름에 대해 두 개의 경로를 만들어 이 문제를 해결합니다. 시스템으로 들어오는 모든 데이터는 다음 두 경로를 거칩니다.

- 일괄 처리 계층(실행 부하 미달 경로)은 들어오는 모든 데이터를 원시 형식으로 저장하고 해당 데이터에 대해 일괄 처리를 수행합니다. 이러한 처리의 결과는 일괄 처리 보기로 저장됩니다. 여러 원본과 더 긴 기간(몇 시간, 며칠 또는 그 이상)에서 데이터를 결합하는 등, 복잡한 분석을 실행하고 보고서, 기계 학습 모델 등의 새 정보를 생성하는 느린 처리 파이프라인입니다.
- 빠른 레이어(웜 경로)는 데이터를 실시간으로 분석합니다. 이 계층은 정확도는 떨어지지만 짧은 대기 시간을 제공하도록 디자인되었습니다. 들어오는 메시지를 보관 및 표시하고 이 레코드를 분석하여 경보 같은 단기 중요 정보 및 작업을 생성하는 더 빠른 처리 파이프라인입니다.
- 일괄 처리 계층은 쿼리에 응답하는 “서비스 계층”에 공급됩니다. 일괄 처리 계층은 효율적인 쿼리를 위해 일괄 처리 보기를 인덱싱합니다. 빠른 계층은 가장 최근 데이터를 기준으로 하는 증분 업데이트로 서비스 계층을 업데이트합니다.

다음 이미지는 변환 단계를 나타내는 5개의 블록을 보여 줍니다. 첫 번째 블록은 속도 계층 및 일괄 처리 계층에 모두 병렬로 공급하는 데이터 스트림입니다. 두 계층 모두 서비스 계층에 공급하며 빠른 계층과 서비스 계층 모두 분석 클라이언트에 공급합니다.
![람다 아키텍처](assets/extracting-insights-from-iot/lambda-schematic.png)

 Azure 플랫폼은 아키텍처 구현에 사용할 수 있는 여러 서비스를 제공합니다. 다음 다이어그램은 이러한 서비스를 매핑하여 구현하는 방법을 보여 줍니다. 이 그림은 변환의 5단계를 보여 주며 각 단계는 관련 Azure 기술을 포함하고 있습니다. 더 어두운 색의 상자가 해당 작업 수행을 위한 여러 옵션의 가용성을 나타냅니다.

![람다 아키텍처](assets/extracting-insights-from-iot/lambda-architecture-all.png)

속도 계층의 데이터 수집 서비스 옵션은 이전 섹션 “데이터 스트림 수집”에서 다루었습니다.

[HDInsight의 Apache Kafka](/azure/hdinsight/kafka/apache-kafka-introduction?WT.mc_id=iotinsightssoln-docs-ercenk)는 데이터 수집 서비스와 스트림 처리 모두를 위해 데이터 스트림을 구현하는 서비스 옵션이 될 수 있습니다.

데이터 수집 서비스에 Event Hubs를 사용할 경우 [ASA(Azure Stream Analytics)](/azure/stream-analytics?WT.mc_id=iotinsightssoln-docs-ercenk)를 사용합니다. Azure Stream Analytics는 디바이스에서 대용량의 데이터 스트리밍을 검사할 수 있도록 하는 이벤트 처리 엔진입니다. 들어오는 데이터는 디바이스, 센서, 웹 사이트, 소셜 미디어 피드, 애플리케이션 등에서 기인할 수 있습니다. 또한 데이터 스트림의 정보 압축, 패턴 및 관계 식별을 지원합니다.

Stream Analytics 쿼리는 Azure Event Hub, Azure IoT Hub로 수집된 스트리밍 데이터의 원본 또는 Azure Blob Storage와 같은 데이터 저장소에서 시작합니다. 스트림을 검사하려면 데이터를 스트리밍하는 입력 원본을 지정하는 Stream Analytics 작업을 만듭니다. 작업은 또한 데이터, 패턴 또는 관계를 검색하는 방법을 지정하는 변환 쿼리를 지정합니다. 변환 쿼리는 기간에 따라 스트리밍 데이터를 필터링, 정렬, 집계 및 조인하는 데 사용되는 SQL과 유사한 쿼리 언어를 활용합니다.

## <a name="warm-path"></a>웜 경로

이 문서의 예제 시나리오는 기계 사용률 KPI(가이드 앞부분에서 소개)입니다. 데이터의 원시 해석을 선택했고, 기계가 데이터를 보내면 이를 사용한다고 가정해 보겠습니다. 그러나 기계는 유휴 또는 유지 보수 상황 등, 실제 아무 것도 생산하지 않는 동안에도 데이터를 보낼 수 있습니다. 여기서 IoT 데이터에서 인사이트를 추출할 때 가장 일반적인 과제가 드러납니다. 즉 확보한 데이터에서 찾고자 하는 데이터를 제공하지 않는 것입니다. 따라서 이 예제에서는 기계가 생산 중인지 여부에 대해서 명확하게 알려 주는 데이터를 가져오지 않게 됩니다.  이 때문에 확보한 데이터를 다른 데이터 원본과 결합하고 기계가 생산 중인지 여부를 판단하기 위한 규칙을 적용하여 사용률을 추론해야 합니다. 또한 회사마다 “생산”에 대한 해석이 다르기 때문에 이러한 규칙도 달라질 수 있습니다. 웜 경로는 데이터가 시스템을 통해 흘러가는 상황을 분석하는 것입니다. 이 스트림을 거의 실시간으로 처리하고 웜 스토리지에 저장하여 분석 클라이언트에 푸시합니다.

Azure Event Hubs는 초당 수백만 개의 이벤트를 수신하여 처리할 수 있는 빅 데이터 스트리밍 플랫폼이자 이벤트 수집 서비스입니다. Event Hubs는 분산된 소프트웨어와 디바이스에서 생성된 이벤트, 데이터 또는 원격 분석을 처리하고 저장할 수 있습니다. Event Hub로 전송된 데이터는 실시간 분석 공급자 또는 일괄 처리/스토리지 어댑터를 사용하여 변환하고 저장할 수 있습니다. Event Hubs는 데이터 흐름에 대한 웜 경로의 첫 단계에 아주 잘 맞습니다.

아래 그림은 빠른 계층 단계를 보여 줍니다. 이벤트 허브, Stream Analytics 인스턴스 및 웜 스토리지에 대한 데이터 저장소로 구성되어 있습니다.

![람다 아키텍처: 빠른 계층 강조 표시](assets/extracting-insights-from-iot/lambda-2.png)
  
Azure 플랫폼은 이벤트 허브에서 이벤트를 처리하는 여러 옵션을 제공하나 Stream Analytics가 권장됩니다. Stream Analytics는 스트리밍된 데이터를 시각화하기 위해 Power BI 서비스에 데이터를 푸시할 수도 있습니다.

Stream Analytics는 대규모의 복잡한 분석을 실행할 수 있습니다. 범위 연속/슬라이드/도약, 스트림 집계, 외부 데이터 원본 조인 등을 예로 들 수 있습니다. 더 복잡한 처리를 위해 다음 그림에서처럼 Event Hubs, Stream Analytics 작업 및 Azure Functions의 여러 인스턴스를 연결하여 성능을 증대할 수 있습니다.

![Event Hubs - Power BI 분석](assets/extracting-insights-from-iot/event-hubs-to-power-bi.png)
  
Azure SQL Database와 같은 Azure 플랫폼에서 다양한 서비스를 사용하여 웜 스토리지를 구현할 수 있습니다. [Azure Cosmos DB](/azure/cosmos-db/introduction?WT.mc_id=iotinsightssoln-docs-ercenk)를 권장합니다. 전 세계에 배포된 Microsoft의 멀티모델 데이터베이스입니다. 유연하며 스키마 독립적이고 자동 인덱싱이 적용되며 쿼리가 풍성한 인터페이스를 사용할 수 있어 데이터 세트에 가장 적합합니다. Cosmos DB에서는 다중 지역, 읽기/쓰기가 가능하며 자동 장애 조치(Failover)뿐 아니라 수동 장애 조치도 지원합니다. 또한 Cosmos DB를 사용하여 데이터에 TTL(Time to Live)을 설정하여 구 데이터가 자동으로 만료되게 할 수 있습니다. 레코드가 데이터베이스에 머무는 시간을 제어하여 결과적으로 데이터베이스 크기를 관리할 수 있도록 이 기능을 사용하는 것이 좋습니다.

Cosmos DB 가격 책정은 사용된 스토리지와 프로비전된 [요청 단위](/azure/cosmos-db/request-units)를 기준으로 합니다. Cosmos DB는 대규모 데이터 세트에서의 집계가 관련된 쿼리가 필요하지 않은 상황에 가장 적합합니다. 이러한 쿼리는 디바이스의 최근 이벤트 등, 기본 쿼리보다 더 많은 요청 단위를 요구하기 때문입니다.  

[Microsoft Power BI](/power-bi/power-bi-overview?WT.mc_id=iotinsightssoln-docs-ercenk)는 소프트웨어 서비스, 앱 및 커넥터의 컬렉션으로, 모두 함께 작용하여 무관한 데이터 원본을 일관되며 시각적으로 몰입도 높은 대화형 인사이트로 변환합니다. Power BI를 사용하면 중요한 정보를 최신으로 유지할 수 있습니다. [Power BI에서 실시간 스트리밍](/power-bi/service-real-time-streaming?WT.mc_id=iotinsightssoln-docs-ercenk)을 사용하여 데이터를 서비스에 푸시할 수 있습니다. 이 실시간 스트림은 Power BI 대시보드에서 다양한 시각적 개체에 대한 실시간 스트리밍 데이터 원본 역할을 할 수 있습니다.

## <a name="cold-path"></a>콜드 경로

웜 경로는 시간 경과에 따른 패턴을 찾아내기 위해 스트림 처리가 발생하는 위치입니다. 그러나 기계, 라인, 공장, 생산된 부품 등, 다른 피벗과 집계를 통해 과거 일정 기간 동안의 사용률도 계산하고자 합니다. 이 결과를 웜 경로의 결과와 결합하여 사용자에게 통합 보기를 제시하려 합니다. 콜드 경로에는 일괄 처리 계층과 서비스 계층이 포함됩니다. 이 조합은 시스템의 장기적 보기를 제공합니다.

콜드 경로는 솔루션에 대한 장기 데이터 저장소를 포함합니다. 장기간 동안 빠른 쿼리 응답을 제공하기 위해 미리 계산된 집계 보기를 만드는 일괄 처리 계층도 포함합니다. Azure 플랫폼에서 이 계층에 사용할 수 있는 기술 옵션은 매우 다양합니다.

![람다 아키텍처: 일괄 처리 계층 강조 표시](assets/extracting-insights-from-iot/lambda-3.png)
  
[Azure TSI(Time Series Insights)](/azure/time-series-insights/?WT.mc_id=iotinsightssoln-docs-ercenk)는 시계열 데이터에 대한 분석, 스토리지, 시각화 서비스입니다. SQL과 유사한 필터링 및 집계를 제공하므로 사용자 정의 함수의 필요성을 낮춥니다. TSI는 Event Hubs, IoT Hub 또는 Azure Blob Storage에서 데이터를 받을 수 있습니다. TSI의 모든 데이터는 메모리 안과 SSD에 저장되므로 대화형 분석에 항상 데이터를 사용할 수 있습니다. 예를 들어, 수십 만 개의 이벤트에 대한 일반적인 집계에서는 밀리초 순서가 반환됩니다. 또한 다른 시계열, 대시보드 비교, 액세스할 수 있는 테이블 형식 뷰 및 히트 맵 등과 같은 시각화를 제공합니다. TSI의 주요 기능은 다음과 같습니다.

- 데이터에 대해 즉시 보고하지 않아도 되는 솔루션에 대한 기본 제공 시각화 서비스. TSI의 데이터 레코드 쿼리 대기 시간은 약 30~60초입니다. 
- 대규모 데이터 세트를 쿼리하는 기능.
- 많은 사용자는 추가 비용 없이 쿼리를 무제한 수행할 수 있습니다.

TSI의 최대 보존 기간은 400일이며 최대 스토리지 용량은 3TB입니다. 더 많은 보존 기간이나 용량이 필요한 경우 콜드 스토리지 데이터베이스(필요에 따라 쿼리하기 위해 데이터를 TSI로 교환)를 사용합니다.

IoT 애플리케이션에 대한 콜드 스토리지는 시간 경과에 따라 확실히 증대됩니다. 여기서 데이터가 장기간 저장되며 분석을 위한 일괄 처리 보기에서 집계됩니다. ML 모델에 대한 데이터도 여기에 저장됩니다. 콜드 스토리지에는 [Azure Storage](/azure/storage/?WT.mc_id=iotinsightssoln-docs-ercenk)가 권장됩니다. 이것은 가용성, 보안, 내구성, 확장성 및 중복성이 높은 클라우드 스토리지를 제공하는 Microsoft 관리 클라우드 서비스입니다. Azure Storage에는 Azure Blob(개체), Azure Data Lake Storage Gen2, Azure File, Azure Queue 및 Azure Table이 포함됩니다. 콜드 스토리지는 Blobs, Data Lake Storage Gen2, Azure Tables 또는 그 조합이 될 수 있습니다.

[Azure Table Storage](/azure/cosmos-db/table-storage-overview?WT.mc_id=iotinsightssoln-docs-ercenk)는 클라우드에 구조화된 NoSQL 데이터를 저장하는 서비스로, 스키마 없이 디자인된 키/특성 저장소를 제공합니다. Table Storage는 스키마가 없기 때문에 애플리케이션의 요구 사항이 변화함에 따라 데이터를 쉽게 적응시킬 수 있습니다. Table Storage 데이터에 대한 액세스는 많은 애플리케이션 유형에 대해 빠르고 비용 효율적이며 비슷한 양의 데이터일 때 일반적으로 전통적인 SQL에 비해 비용이 매우 낮습니다. 샘플에 한 테이블, 데이터 스트림에서 받은 이벤트에 한 테이블을 사용합니다. 파티션 키 디자인은 특별히 중요한 개념으로, 두 테이블 모두 이벤트나 샘플에 타임스탬프의 시간을 사용합니다. 자세한 내용은 [테이블 서비스 데이터 모델 이해](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model?WT.mc_id=iotinsightssoln-docs-ercenk)를 참조하세요.

IoT 애플리케이션이 받은 처리되지 않은 데이터를 포함하는 JSON 또는 XML 문서처럼 대규모의 비정형 데이터를 저장하려면 [Blob Storage](/azure/storage/blobs/storage-blobs-introduction?WT.mc_id=iotinsightssoln-docs-ercenk), [Azure Files](/azure/storage/files/storage-files-introduction?WT.mc_id=iotinsightssoln-docs-ercenk), 또는 [Azure Data Lake Storage Gen2](/azure/storage/data-lake-storage/introduction?WT.mc_id=iotinsightssoln-docs-ercenk)가 가장 적합한 옵션입니다.

Azure Blob Storage는 HTTP 또는 HTTPS를 통해 전 세계 어디에서든 안전하게 액세스할 수 있습니다. Blob Storage에 액세스하려면 서비스에서 사용되는 [권한 부여 메커니즘](/azure/storage/common/storage-auth?toc=%2fazure%2fstorage%2fblobs%2ftoc.json?WT.mc_id=iotinsightssoln-docs-ercenk) 중 하나를 통해 승인을 받아야 합니다. 이 서비스는 로컬 중복, 영역 중복, 지역 중복 및 읽기 액세스 지역 중복 등, 여러 복제 [옵션](/azure/storage/common/storage-redundancy?toc=%2fazure%2fstorage%2fblobs%2ftoc.json?WT.mc_id=iotinsightssoln-docs-ercenk)을 제공합니다. 가장 경제적인 솔루션을 제시하는 3가지 [액세스 계층](/azure/storage/blobs/storage-blob-storage-tiers?WT.mc_id=iotinsightssoln-docs-ercenk)도 있습니다.

콜드 스토리지에 데이터가 있게 되면 람다 아키텍처의 서비스 계층에 대한 일괄 처리 보기를 만들어야 합니다. [Azure Data Factory](/azure/data-factory/introduction?WT.mc_id=iotinsightssoln-docs-ercenk)는 서비스 계층에서 일괄 처리 보기를 만들기 위한 좋은 솔루션입니다. 데이터 이동 및 데이터 변환을 오케스트레이션하고 자동화하기 위해 클라우드에서 데이터 기반 워크플로를 만들 수 있는 클라우드 기반 관리 데이터 통합 서비스입니다. Azure Data Factory를 사용하여 서로 다른 데이터 저장소의 데이터를 수집할 수 있는 [데이터 기반 워크플로](/azure/data-factory/concepts-pipelines-activities?WT.mc_id=iotinsightssoln-docs-ercenk)(파이프라인이라고 함)를 만들고 예약할 수 있습니다. [Azure HDInsight Hadoop](/azure/hdinsight/?WT.mc_id=iotinsightssoln-docs-ercenk), [Spark](/azure/hdinsight/?WT.mc_id=iotinsightssoln-docs-ercenk) 및 [Azure Databricks](/azure/azure-databricks/?WT.mc_id=iotinsightssoln-docs-ercenk)와 같은 서비스를 사용하여 데이터를 처리하고 변환할 수 있습니다. 이렇게 하면 기계 학습 모델을 빌드하여 분석 클라이언트에 활용할 수 있습니다.

예를 들어, 다음 그림에서처럼 데이터 팩터리 파이프라인은 마스터 데이터 저장소에서 데이터를 읽습니다. 한 파이프라인이 데이터를 요약 및 집계하여 Azure Data Warehouse 인스턴스를 입력합니다. Data Factory 파이프라인도 ML 모델 빌드에 사용되는 [Azure Databricks 노트북 작업](/azure/data-factory/transform-data-using-databricks-notebook?WT.mc_id=iotinsightssoln-docs-ercenk)을 포함합니다.

![람다 아키텍처: 일괄 처리 계층 강조 표시](assets/extracting-insights-from-iot/master-data-to-ml-analytics.png)
  
[Azure SQL Database](/azure/sql-database/?WT.mc_id=iotinsightssoln-docs-ercenk) 또는 [Azure SQL Data Warehouse](/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is?WT.mc_id=iotinsightssoln-docs-ercenk)는 최적의 일괄 처리 보기 호스트 옵션입니다. 이러한 서비스는 마스터 데이터에 대해 미리 계산 및 집계된 보기를 서비스할 수 있습니다. 

Azure SQL DB(SQL Database)는 최신 Microsoft SQL Server 데이터베이스 엔진 버전을 기반으로 하는 관계형 DaaS(Database as-a-Service)입니다. SQL DB는 믿을 수 있고 안전한 고성능 데이터베이스로, 데이터 기반 애플리케이션과 웹 사이트를 빌드하는 데 사용할 수 있습니다. Azure 서비스로서, 인프라를 관리할 필요가 없습니다. 데이터 볼륨이 증가하면 솔루션은 기술을 통해 데이터를 집계 및 저장하여 쿼리 속도를 증대할 수 있습니다. 집계를 미리 계산하는 것은 특히 추가 전용 데이터에는 잘 알려진 방법입니다. 비용 관리에도 유용합니다.

Azure SQL Data Warehouse는 몇 가지 시나리오에서 유용할 수 있는 많은 추가 기능을 제공합니다. 대규모 병렬 처리를 활용하는 클라우드 기반 엔터프라이즈 데이터 웨어하우스이며 페타바이트 데이터에서 복잡한 쿼리를 신속하게 실행합니다. 페타바이트의 데이터를 유지하고 쿼리를 신속히 실행하려면 SQL Data Warehouse를 사용하는 것이 좋습니다.

## <a name="visualizing-the-data"></a>데이터 시각화

이 계층에서는 두 데이터 파이프라인(웜 및 콜드 경로)을 결합하여 일관된 데이터 보기를 제시하려 합니다. 이 예제에서는 여러 메트릭을 사용하여 웜 및 콜드 경로 모두에서 기계의 사용률을 추론했습니다. 분석 단계에서는 이 경로의 데이터를 결합하는 시각화를 제공합니다.

![람다 아키텍처: 분석 클라이언트 계층 강조 표시](assets/extracting-insights-from-iot/lambda-4.png)

[Microsoft Power BI](/power-bi/?WT.mc_id=iotinsightssoln-docs-ercenk) 및 [Azure Time Series Insights](/azure/time-series-insights/?WT.mc_id=iotinsightssoln-docs-ercenk)는 바로 사용할 수 있는 데이터 시각화를 제공합니다. Power BI는 데이터를 시각화하고 조직 전체에서 결과를 공유하거나 앱 또는 웹 사이트에 포함할 수 있는 비즈니스 분석 솔루션입니다. [Power BI Desktop](https://powerbi.microsoft.com/desktop/?WT.mc_id=iotinsightssoln-docs-ercenk)은 보고서 및 기본 데이터 원본의 모델링을 위한 강력한 무료 도구입니다.  Power BI 시각화를 포함하는 애플리케이션은 데스크톱 도구에서 작성하여 Power BI Service에서 호스트되는 보고서를 사용합니다.

Time Series Insights에는 REST 쿼리 API뿐만 아니라 데이터를 시각화하고 쿼리할 수 있는 데이터 탐색기가 있습니다. 또한 사용자 지정 애플리케이션에 TSI 제공 차트를 포함할 수 있는 JavaScript 컨트롤 라이브러리를 노출합니다. 다음은 관측된 샘플 수만을 통해 매장의 기계 사용률을 대략적으로 추정하는 들어오는 데이터에 대한 기본 히트맵 보기입니다.

![람다 아키텍처: 일괄 처리 계층 강조 표시](assets/extracting-insights-from-iot/client-screen.png)

여러 원본에서 데이터를 집계하는 브라우저 기반 사용자 인터페이스가 필요한 경우 TSI와 Power BI 서비스 모두 시각화 컨트롤 포함이 가능합니다. 둘 다 광범위한 사용자 지정을 허용하는 REST API([Power BI Rest API](/rest/api/power-bi/?WT.mc_id=iotinsightssoln-docs-ercenk), [TSI REST API](/rest/api/time-series-insights/time-series-insights-reference-queryapi?WT.mc_id=iotinsightssoln-docs-ercenk)) 및 JavaScript SDK([Power BI JavaScript SDK](https://github.com/Microsoft/PowerBI-JavaScript?WT.mc_id=iotinsightssoln-docs-ercenk), [TSI JavaScript SDK](/azure/time-series-insights/tutorial-explore-js-client-lib?WT.mc_id=iotinsightssoln-docs-ercenk))를 제공합니다.

## <a name="next-steps"></a>다음 단계

많은 개념을 다루었으며, 독자들이 더 많은 내용을 알아보고 자체 요구 사항에 기법을 적용할 수 있는 여러 출발점을 제시하고자 합니다. 이를 위해 유용할 수 있는 몇 가지 자습서는 다음과 같습니다.

- 데이터를 스트림으로 변환
  - [일정에 따라 실행되는 Logic App 만들기](/azure/logic-apps/tutorial-build-schedule-recurring-logic-app-workflow?WT.mc_id=iotinsightssoln-docs-ercenk)
  - [Logic Apps의 데이터 작업에 대한 코드 예제](/azure/logic-apps/logic-apps-data-operations-code-samples?WT.mc_id=iotinsightssoln-docs-ercenk)
  - Azure Function을 호스트하는 [컨테이너에서 Azure Functions 실행](/azure/azure-functions/functions-create-function-linux-custom-image?WT.mc_id=iotinsightssoln-docs-ercenk)은 여러 문서에서 다루고 있습니다. 모든 플랫폼에서 함수를 실행하는 사용자 지정 이미지 및 Azure Functions Runtime용 Docker 이미지를 사용하여 Linux에서 함수 만들기
  - [Azure Functions에서 다양한 바인딩 사용](/azure/azure-functions/functions-triggers-bindings?WT.mc_id=iotinsightssoln-docs-ercenk)

- 핫 경로
  - Event Hubs, Azure Stream Analytics 및 Power BI의 사용을 보여 주는 엔드투엔드 자습서입니다. 단계별 지침은 [자습서: Azure Event Hubs로 전송되는 실시간 이벤트에서 데이터 변칙 시각화](/azure/event-hubs/event-hubs-tutorial-visualize-anomalies?WT.mc_id=iotinsightssoln-docs-ercenk)와 [전화 통화 데이터를 분석하기 위한 Stream Analytics 작업 만들기](/azure/stream-analytics/stream-analytics-manage-job?WT.mc_id=iotinsightssoln-docs-ercenk) 및 Power BI 대시보드에서 결과 시각화를 참조하세요.
  -[.NET에 Azure Cosmos DB 사용](/azure/cosmos-db/sql-api-get-started?WT.mc_id=iotinsightssoln-docs-ercenk)
- 콜드 경로
  - [Azure Data Factory에서 Spark 작업을 사용하여 클라우드의 데이터 변환](/azure/data-factory/tutorial-transform-data-spark-portal?WT.mc_id=iotinsightssoln-docs-ercenk)
  - [자습서: Azure Time Series Insights 환경 만들기](/azure/time-series-insights/tutorial-create-populate-tsi-environment?WT.mc_id=iotinsightssoln-docs-ercenk)
- 분석 클라이언트
  - [Power BI 살펴보기](/power-bi/guided-learning/?WT.mc_id=iotinsightssoln-docs-ercenk)
  - [Time Series Insights SPA 만들기](/azure/time-series-insights/tutorial-create-tsi-sample-spa?WT.mc_id=iotinsightssoln-docs-ercenk)
  - [Time Series Insights Java Script 클라이언트 라이브러리 살펴보기](/azure/time-series-insights/tutorial-explore-js-client-lib?WT.mc_id=iotinsightssoln-docs-ercenk)
  - [TSI 데모](https://insights.timeseries.azure.com/demo) 및 [Power BI 데모](https://microsoft.github.io/PowerBI-JavaScript/demo/v2-demo/index.html)를 참조하세요.

## <a name="appendix-pillars-of-software-quality-posq"></a>부록: 소프트웨어 품질 핵심 요소(PoSQ)

성공적인 클라우드 애플리케이션은 다음과 같은 [소프트웨어 품질 핵심 요소](/azure/architecture/guide/pillars?WT.mc_id=iotinsightssoln-docs-ercenk)를 기반으로 빌드합니다. 확장성, 가용성, 복원력, 관리 및 보안 이 섹션에서는 각 구성 요소에 대해 이러한 기본 요소를 필요에 따라 간략히 설명합니다. 구현 수준에서 대부분 다룬 가용성, 복원력, 관리 및 DevOps에 대해서는 다루지 않습니다. Azure 플랫폼은 API, 도구, 진단 및 로깅을 통해 이를 달성할 수 있는 광범위한 방법을 제공한다는 점을 강조하고자 합니다. 여기서 언급한 기본 요소 외에도 비용 효율성에 대해 설명합니다.

이러한 기본 요소를 간략히 살펴보겠습니다.

- **확장성**은 증가된 부하를 처리하는 시스템의 기능입니다. 애플리케이션은 다음 두 가지 주요 방법으로 확장될 수 있습니다. 예를 들어, 수직 확장(강화)이란 더 큰 VM 크기를 사용하여 리소스의 용량을 늘리는 것입니다. 수평 확장(확장)이란 VM 또는 데이터베이스 복제본과 같은 리소스의 새 인스턴스를 추가하는 것입니다. 확장성 원칙에는 성능 및 로드 처리 기능도 포함됩니다.
- **가용성**은 시스템이 기능하고 작동하는 시간의 비율입니다. 일반적으로 가동 시간의 백분율로 측정됩니다. 애플리케이션 오류, 인프라 문제 및 시스템 부하는 모두 가용성을 저하시킬 수 있습니다. Microsoft Azure 서비스에 대한 서비스 수준 약정은 [Service Level Agreement(서비스 수준 약정)](https://azure.microsoft.com/support/legal/sla/?WT.mc_id=iotinsightssoln-docs-ercenk)에서 게시 및 제공됩니다. 가용성은 시스템 수준에서만 유의미한 메트릭입니다. 별개의 구성 요소는 시스템의 전반적인 가용성에 영향을 줍니다.
- **복원력**은 오류를 복구하여 계속 작동하는 시스템 기능입니다. 복원력의 목표는 오류가 발생한 후에 애플리케이션을 완전히 작동하는 상태로 되돌리기 위한 것입니다. 복원력은 가용성과 밀접한 관련이 있습니다.
- **관리 및 DevOps**. 이 핵심 요소에서는 프로덕션에서 애플리케이션을 계속 실행하는 작업 프로세스를 다룹니다. 배포는 안정적이고 예측이 가능해야 합니다. 사용자 오류의 발생 가능성을 줄이기 위해 자동화되어야 합니다. 새로운 기능 또는 버그 수정의 릴리스 속도를 저하하지 않도록 빠르고 일상적인 프로세스여야 합니다. 또한 업데이트에 문제가 있는 경우 신속하게 롤백 또는 롤포워드할 수 있어야 합니다.
- **보안**에는 디자인 및 구현에서 배포 및 작업까지 솔루션의 전체 수명 주기에서 가장 주안점이 맞춰져야 합니다. ID 관리, 인프라 보호, 애플리케이션 보안, 인증, 데이터 주권 및 암호화, 감사는 모두 해결이 필요한 광범위 영역입니다.

## <a name="posq-converting-the-data-to-a-stream"></a>PoSQ: 데이터를 스트림으로 변환

**확장성**: 두 가지 관점에서 확장성에 접근할 수 있습니다. 먼저 구성 요소의 관점에서 보는 것이고 두 번째는 원본 데이터를 제공하는 시스템의 관점에서 보는 것입니다.

각각의 Azure 서비스는 수직적 및 수평적 확장을 위한 옵션을 제공합니다. 솔루션을 설계하는 동안 확장성 요구를 고려하는 것이 좋습니다.

원본 데이터를 제공하는 시스템의 경우 지나치게 잦은 쿼리로 시스템에 과부하를 초래하여 결과적으로 DoS(서비스 거부) 공격을 야기하지 않기 위해 주의가 필요합니다. 시스템을 폴링할 경우 폴링 간격 조정에는 두 가지 결과가 따른다는 점에 유의합니다. 즉 데이터 세분성(쿼리가 잦을수록 실시간에 근접)과 원격 시스템에 초래되는 로드입니다.

**보안**: 대칭 또는 비대칭 키로 원격 시스템에 액세스할 경우 비밀을 [Azure Key Vault](/azure/key-vault/?WT.mc_id=iotinsightssoln-docs-ercenk)에 보관하는 것이 좋습니다.

## <a name="posq-warm-path"></a>PoSQ: 웜 경로

**확장성**: Azure Event Hubs가 수집 하위 시스템에 사용될 경우 기본 확장성 메커니즘은 [처리량 단위](/azure/event-hubs/event-hubs-features#throughput-units?WT.mc_id=iotinsightssoln-docs-ercenk)입니다. Event Hubs는 처리량 단위를 정적으로 설정하거나 [자동 팽창 기능](/azure/event-hubs/event-hubs-auto-inflate?WT.mc_id=iotinsightssoln-docs-ercenk)을 통해 설정하는 기능을 제공합니다.

Stream Analytics의 [SU(스트리밍 단위)](/azure/stream-analytics/stream-analytics-streaming-unit-consumption?WT.mc_id=iotinsightssoln-docs-ercenk)는 작업을 실행하도록 할당된 컴퓨팅 리소스를 나타냅니다. SU 수가 클수록 작업에 더 많은 CPU 및 메모리 리소스가 할당됩니다. 이러한 용량을 통해 쿼리 논리에 중점을 두고 Stream Analytics 작업을 적시에 실행하도록 하드웨어를 관리해야 할 필요성을 요약할 수 있습니다. SU 외에도 [쿼리의 적절한 병렬 처리](/azure/stream-analytics/stream-analytics-scale-jobs?WT.mc_id=iotinsightssoln-docs-ercenk)를 통해 사용하는 것도 중요합니다.

Azure Cosmos DB 구현은 올바른 처리량 매개 변수와 적합한 분할 디자인을 통해 프로비전되어야 합니다. 처리량 프로비전은 컨테이너 또는 데이터베이스 수준에서 가능하며 [RU(요청 단위)](/azure/cosmos-db/request-units?WT.mc_id=iotinsightssoln-docs-ercenk)로 표현됩니다. Cosmos DB는 RU 추정을 위한 도구를 제공합니다. 처리량 프로비전 외에도 [효율적으로 데이터베이스를 분할](/azure/cosmos-db/partition-data?WT.mc_id=iotinsightssoln-docs-ercenk)하는 것이 핵심입니다.

**보안**: 클라이언트에 의한 Azure Event Hubs 액세스는 클라이언트 인증에 대한 SAS(공유 액세스 시그니처) 토큰과 이벤트 게시자 조합을 통해 이루어집니다. 백엔드 애플리케이션에 대한 보안은 서비스 버스 토픽과 동일한 개념을 따릅니다. Event Hubs 보안 모델에 대한 상세 설명은 [Event Hubs 인증 및 보안 모델 개요](/azure/event-hubs/event-hubs-authentication-and-security-model-overview?WT.mc_id=iotinsightssoln-docs-ercenk)에서 제공합니다.

Cosmos DB 데이터베이스 보안은 데이터에 대한 제어 액세스와 저장 데이터 암호화를 제공합니다. 자세한 내용은 [Azure Cosmos DB 데이터베이스 보안](/azure/cosmos-db/database-security?WT.mc_id=iotinsightssoln-docs-ercenk)을 참조하세요.

**비용 효율성**: Event Hubs의 가격 책정은 SKU(표준 또는 프리미엄), 수신된 수백 만 개의 이벤트, 처리량 단위의 함수입니다. 들어오는 메시지에서 지시하는 데이터 수집 속도를 살펴 최적의 조합을 이룰 수 있습니다.

Cosmos DB를 사용할 경우 RU 사용을 통해 저장소의 최적 사용을 관찰하는 것이 좋습니다. Cosmos DB에는 앞서 설명한 것처럼 데이터 보존을 제어하기 위한 기능도 있습니다. 이 기능을 통해 레코드가 데이터베이스에 머무는 기간을 제어하여 데이터베이스 크기를 관리하는 것이 좋습니다.

## <a name="posq-cold-path"></a>PoSQ: 콜드 경로

**확장성**: Azure TSI(Time Series Insights)는 수신 속도, 스토리지 용량 및 SKU 관련 비용에 적용되는 승수인 이름이 “capacity”인 메트릭을 통해 확장됩니다. 

Azure Time Series Insights는 수직 규모 조정에도 직접적인 영향이 있는 여러 SKU를 갖습니다. 확장에 대한 자세한 내용은 [Azure Time Series Insights 환경 계획](/azure/time-series-insights/time-series-insights-environment-planning?WT.mc_id=iotinsightssoln-docs-ercenk) 문서를 참조하세요. 다른 많은 Azure 서비스처럼 TSI도 “시끄러운 이웃” 문제를 방지하기 위해 제한의 대상이 될 수 있습니다. 노이지 네이버(Noisy Neighbor)는 공유 환경(/azure/sql-database/sql-database-service-tiers-vcore?WT.mc_id=iotinsightssoln-docs-ercenk)에 있는 애플리케이션으로서, 다른 사용자의 리소스까지 독점합니다. 제한 관리는 [TSI 설명서](/azure/time-series-insights/time-series-insights-environment-mitigate-latency?WT.mc_id=iotinsightssoln-docs-ercenk)를 참조하세요. 

스토리지 계정의 확장성 목표는 [Azure Storage 확장성 및 성능 목표](/azure/storage/common/storage-scalability-targets?WT.mc_id=iotinsightssoln-docs-ercenk)에서 설명합니다. 단일 스토리지 계정의 용량보다 많은 데이터를 저장하기 위한 일반적인 방법은 여러 스토리지 계정에 걸쳐 분할하는 것입니다.

Azure SQL Database에는 구매 모델([DTU 기반](/azure/sql-database/sql-database-service-tiers-dtu?WT.mc_id=iotinsightssoln-docs-ercenk) 및 vCore 기반)에 따라 수직 및 수평 확장성을 관리하는 여러 옵션이 있습니다. 토픽에 관한 [SQL Database 설명서](/azure/sql-database/sql-database-scale-resources?WT.mc_id=iotinsightssoln-docs-ercenk)를 통해 다른 솔루션에 대한 최적 옵션을 더 연구해 보는 것이 좋습니다.

**보안**: TSI 환경은 서로 독립적인 관리 액세스 및 데이터 액세스를 위한 [액세스 정책](/azure/time-series-insights/time-series-insights-data-access?WT.mc_id=iotinsightssoln-docs-ercenk)을 제공합니다. 정의된 데이터 원본 이외의 TSI 환경에 데이터를 추가하는 직접적인 방법은 없습니다. 관리 액세스 정책은 환경 구성과 관련한 권한을 부여합니다. 데이터 액세스 정책은 데이터 쿼리를 실행하고 환경에서 참조 데이터를 조작하며 환경과 관련된 저장된 쿼리 및 관심 사항을 공유 할 수 있는 권한을 부여합니다.

Azure Data Factory 서비스는 관리 저장소 또는 Azure Key Vault에서 데이터 저장소 자격 증명을 보호하기 위한 여러 방법을 제공합니다. 전송 중인 데이터 암호화는 데이터 저장소의 전송(예: HTTPS 또는 TLS)에 따라 달라집니다. 저장 데이터 암호화도 데이터 저장소에 따라 달라집니다. 자세한 내용은 [Azure Data Factory의 데이터 이동에 대한 보안 고려 사항](/azure/data-factory/data-movement-security-considerations?WT.mc_id=iotinsightssoln-docs-ercenk)을 참조하세요.

SQL Database는 데이터 액세스, 모니터링 및 감사를 위한 광범위한 보안 기능과 저장 데이터 암호화를 제공합니다. 자세한 내용은 [SQL Server 데이터베이스 엔진 및 Azure SQL Database 보안 센터](/sql/relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database?WT.mc_id=iotinsightssoln-docs-ercenk)를 참조하세요.

**비용 효율성**: 모든 분석 솔루션의 핵심은 스토리지입니다. 분석 엔진은 합당한 시간에 일정 규모의 데이터를 처리하기 위해 속도, 효율성, 보안 및 처리량이 필요합니다. 데이터를 집계 및 요약하고 효율적으로 폴리글롯 저장소를 사용하여 기본 플랫폼을 최대한 활용하기 위한 메커니즘의 고안은 효율적인 비용 관리를 위해 필요한 수단입니다. Azure는 클라우드 플랫폼이므로 프로그램 방식으로 리소스 서비스 해제, 서비스 재승인 및 크기 조정을 위한 방법을 제시합니다. 예를 들어 [만들기 또는 업데이트 작업](/rest/api/sql/databases/createorupdate?WT.mc_id=iotinsightssoln-docs-ercenk)은 Azure SQL Database의 데이터베이스 크기를 변경하는 방법을 제공합니다.