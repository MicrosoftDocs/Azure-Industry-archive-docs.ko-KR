---
title: 예측 유지 관리 솔루션
author: ercenk
ms.author: ercenk
ms.date: 05/03/2018
ms.topic: article
ms.service: industry
description: Azure에서 제조 산업 고객을 위한 예측 유지 관리를 개발하는 방법에 대한 솔루션을 설명합니다.
ms.openlocfilehash: 1c7b95e2da21df46465ccaf21827ae97597206a2
ms.sourcegitcommit: 76f2862adbec59311b5888e043a120f89dc862af
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "51654320"
---
# <a name="predictive-maintenance-in-manufacturing-solution-guide"></a>제조 솔루션의 예측 유지 관리 가이드


## <a name="introduction"></a>소개

예측 유지 관리는 센서, 인공 지능 및 데이터 과학을 결합하여 장비 유지 관리를 최적화합니다. 장비 유지 관리를 통해 유지 관리 비용을 최소화하고 가동 시간을 최대화해야 한다고 요구하는 것은 제조업체에 상당한 가치를 제공합니다.

데이터는 솔루션의 핵심입니다. 데이터에는 적절한 오류 표시기와 컨텍스트를 설명하는 다른 측면이 있어야 합니다. 센서, 머신 로그 및 제조 애플리케이션 로그와 같은 여러 원본에서 가져올 수 있습니다.

이 문서에서는 예측 유지 관리 솔루션을 구축하기 위한 옵션을 제시합니다. 제시된 다양한 관점과 기존 자료를 사용하여 시작할 수 있습니다. 예측 유지 관리 솔루션에 대한 요구 사항은 장비, 환경, 프로세스 및 조직에 따라 다릅니다. 요구 사항에 맞는 솔루션을 제시하는 과정에서 안내할 수 있는 대체 방법과 기술을 제공하려고 합니다.

먼저 예측 유지 관리 솔루션의 대략적인 구성 요소를 사용하여 시작해 보겠습니다.


![고급 솔루션](./assets/pdm-assets/highlevelsolution.png)

이 분석에서 수행되는 대략적인 작업은 다음과 같습니다.

1. 오류 데이터를 포함하여 학습 데이터를 수집합니다.

2. 이 학습 데이터를 사용하여 일단의 조건에 따라 미래 자산 오류를 예측하는 ML(기계 학습) 모델을 학습합니다.

3. 지속적으로 데이터를 계속 수집합니다.

4. 수집된 데이터를 ML 모델에 입력합니다. 이 모델은 일반적으로 특정 수준의 신뢰도로 오류를 예측합니다(예: "다음 24시간 이내 머신에서 오류가 발생할 확률이 85%입니다").

5. 예측된 오류 사례를 노출합니다.

6. 데이터에 표시되는 인사이트를 계획하고 수행합니다.

## <a name="training-the-ml-model"></a>ML 모델 학습

ML 모델을 작성하려면 충분하고, 정확하며, 완전한 데이터가 필요합니다. 또한 예측 유지 관리에는 고유한 문제가 있으며, 주요 문제는 오류 데이터의 가용성입니다. 오류는 비교적 드문 이벤트이며, 특히 CNC 머신 또는 정유소 구성품과 같은 고자본 장비에서 발생합니다. 이로 인해 장기간에 걸쳐 센서 데이터를 수집했더라도 오류 데이터가 충분하지 않을 수 있습니다. 다음과 같이 "오류"가 정의되는 방법을 고려해 보세요. 정확히 어떤 것이 오류를 구성합니까? 디바이스에서 예기치 않게 작동을 중지하는 경우입니까?
디바이스가 원하는 수준에서 더 이상 작동하지 않는 수준으로 저하되는 경우입니까? 오류 발생 시 금속 피로에 따른 부품 오류 또는 재해가 발생하기 전에 오류를 나타내는 다른 표시기로 인해 절삭기가 파손되고 있는 오류 사례입니까?

## <a name="considering-the-data-needed-for-ml"></a>ML에 필요한 데이터에 대한 고려

이러한 오류를 제대로 기록하는 데 충분한 데이터를 캡처하고 있는지도 고려합니다. 대부분의 경우 센서 데이터만으로는 오류를 식별하기에 충분하지 않을 수 있습니다. 경우에 따라 머신의 상태를 오류 상태 "플래그로 지정"하기 위해 외부 데이터가 필요하거나 다른 시스템을 통해 오류 사례를 캡처하는 운영자와 같은 보조 정보원이 필요할 수도 있습니다. 이 데이터는 ERP, MES(제조 실행 시스템), 이력 장치(historian) 등과 같은 외부 시스템에 있을 수 있으며, 제조 회사에서 널리 퍼져 있는 악명 높은 IT/OT 경계를 넘나들어 필요한 데이터의 보안을 더욱 어렵게 만들 수 있습니다.

기본적으로 예측 유지 관리는 동적 문제이므로 연결된 기계 학습 모델을 지속적으로 새로 고쳐야(또는 다시 학습) 합니다. 제대로 작동하는 경우 예측 유지 관리는 오류 인스턴스를 줄여야 합니다. 이는 좋은 일이지만 오류 데이터가 적습니다. 또한 오류에 영향을 미치는 기능으로 인해 이전의 기계 학습 모델을 무효화하는 것을 바꿀 수 있습니다. 모델을 변경된 오류 조건으로 정기적으로 학습하는 것이 좋습니다.

또한 "새로운" 데이터는 이전에 모델을 학습하는 데 사용된 조건과 다른 새 조건이 모델에 도입되었음을 의미합니다. 다시 말해, 오류를 _x<sub>1</sub>,x<sub>2</sub>,⋯,x<sub>n</sub>, f(x<sub>1</sub>,x<sub>2</sub>,⋯,x<sub>n</sub>)_ 변수의 함수로 모델링할 수 있지만, 결국 _x<sub>(n+1)</sub>,⋯,x<sub>(m+n)</sub>_ 변수에서 오류에 영향을 미치고 있다는 것을 발견할 수 있으므로 _f(x<sub>1</sub>,x<sub>2</sub>,⋯,x<sub>(m+n)</sub>)_ 에 대한 모델 학습을 수정해야 할 수도 있습니다. 모델은 오류를 감지하는 데 잘 수행되지 않을 수 있으며, 다음 반복을 위해 머신의 MES 로그 데이터 요소를 포함하여 새 모델이 작성될 수 있습니다.

클라우드로 데이터를 스트림하는 최신 IoT 환경이 없더라도 기계 학습 모델을 학습하는 데 필요한 데이터는 이미 MES, 이력 장치 또는 기타 프로덕션 시스템에 있을 수 있습니다. 데이터를 준비하는 것만으로도 기계 학습 모델을 학습하는 데 사용할 수 있습니다.

다음 그림에서는 ML 모델을 학습하는 일반적인 워크플로를 보여 줍니다. "반복"이라는 레이블이 지정된 화살표는 이 워크플로가 반복적 과정임을 의미합니다. 즉 새 데이터가 들어오고 조건이 변경됨에 모델을 지속적으로 다시 학습합니다. 이 루프를 반복해야 하는 시기와 빈도는 구현의 특정 조건에 따라 달라지며, 모델이 "노화" 또는 "저하"되는 경우를 감지하기 위해 오류를 예측할 때 이전에 작성된 모델의 성능을 신중하게 모니터링해야 합니다.

새 모델을 지속적으로 학습하고 배포하면 모델 관리에도 어려움이 따릅니다. Microsoft는 모델의 CI/CD(지속적인 통합 및 지속적인 업데이트)를 위한 Azure Machine Learning 모델 관리 서비스를 제공합니다.

![ML 모델 빌드 단계](assets/pdm-assets/mlmodelbuildingstages.png)

Microsoft는 데이터를 준비하고 기계 학습 모델을 학습하는 방법에 대한 [자세한 가이드](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance?WT.mc_id=pdmsolution-docs-ercenk)를 게시했습니다. 다음 세 가지 일반적인 유지 관리 질문과 관련 기계 학습 알고리즘이 있습니다.

- _자산의 경우 다음 X시간 내에 오류가 발생할 확률은 얼마입니까?_ 대답: 0-100%
  - **이진 분류:** 데이터를 사용하여 항목 또는 데이터 행의 범주, 형식 또는 클래스를 두 클래스 중 하나의 멤버로 결정하는 기계 학습 방법입니다. 여러 유형의 분류 알고리즘이 있으며, Microsoft는 [Machine Learning Studio 모듈](https://docs.microsoft.com/en-us/azure/machine-learning/studio-module-reference/machine-learning-initialize-model-classification?WT.mc_id=pdmsolution-docs-ercenk)로 사용할 수 있는 알고리즘 세트를 게시했습니다.
- _자산의 잔여 수명은 얼마입니까?_ 대답: X시간
  - **회귀:** 다른 변수 세트로 지정된 변수의 값을 예측하는 기계 학습 알고리즘의 클래스입니다. Machine Learning Studio에는 회귀 알고리즘 세트가 [모듈](https://docs.microsoft.com/en-us/azure/machine-learning/studio-module-reference/machine-learning-initialize-model-regression?WT.mc_id=pdmsolution-docs-ercenk)로 포함되어 있습니다.
    - **LSTM(Long Short Term Memory):** [LSTM](http://colah.github.io/posts/2015-08-Understanding-LSTMs/?WT.mc_id=pdmsolution-docs-ercenk) 네트워크는 DNN(딥 신경망)의 한 유형입니다. DNN의 영감은 뇌의 개별 뉴런에 대한 동작을 모델링하는 것에서 비롯됩니다. Microsoft는 예측 유지 관리에 LSTM을 사용하는 방법을 설명하는 [단계별 가이드](https://docs.microsoft.com/en-us/azure/machine-learning/desktop-workbench/scenario-deep-learning-for-predictive-maintenance?WT.mc_id=pdmsolution-docs-ercenk)를 게시했습니다.
- _어떤 자산에 가장 긴급하게 서비스를 제공해야 합니까?_ 대답: X 자산
  - **다중 클래스 분류:** 데이터를 사용하여 항목 또는 데이터 행의 범주, 형식 또는 클래스를 셋 이상의 클래스의 멤버로 결정하는 기계 학습 방법입니다.

다시 말해, 데이터를 가져오는 것은 여러 채널을 활용하고, 대량으로 초기화한 다음, 스트리밍 데이터를 계속 받아서 오류를 예측하고, 모델의 후속 작성에도 사용할 수 있다는 것을 의미할 수 있습니다.

## <a name="bringing-data-to-azure"></a>Azure로 데이터 가져오기

Microsoft Azure는 데이터 수집 및 저장을 위한 다양한 서비스를 제공합니다. 데이터가 Azure에 아직 없는 경우 데이터를 Azure로 전송하는 일괄 처리 방법을 사용하는 것이 좋습니다. 데이터를 csv, json, xml 등과 같이 잘 알려진 형식으로 내보낼 수 있는 경우 이러한 방법은 좋은 옵션입니다. 또한 업로드하기 전에 압축하고 클라우드 쪽에서 추가로 처리하도록 선택할 수도 있습니다.

- [AzCopy](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy?WT.mc_id=pdmsolution-docs-ercenk)를 사용하여 스토리지 Blob에 업로드(Windows 및 Linux 모두)

- Linux에서 파일 시스템으로 [Blob 스토리지 탑재](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-how-to-mount-container-linux?WT.mc_id=pdmsolution-docs-ercenk)

- [가져오기/내보내기 서비스](https://docs.microsoft.com/en-us/azure/storage/common/storage-import-export-service?WT.mc_id=pdmsolution-docs-ercenk) 사용 - 데이터 크기가 크고 업로드하는 데 너무 많은 시간이 걸리는 경우

- Windows, Linux 및 MacOS에서 [Azure 파일 공유 탑재](https://docs.microsoft.com/en-us/azure/storage/files/storage-how-to-use-files-windows?WT.mc_id=pdmsolution-docs-ercenk)

데이터가 SQL Server 데이터베이스에 있는 경우 [Data Migration Assistant](https://docs.microsoft.com/en-us/sql/dma/dma-overview?WT.mc_id=pdmsolution-docs-ercenk)를 사용하여 데이터를 Azure SQL Database에 업로드할 수도 있습니다.

Azure 플랫폼에는 ETL(추출, 변환 및 로드) 작업을 위한 다양한 도구와 서비스가 있습니다. 가장 중요한 서비스는 데이터를 조작하기 위한 모든 기능 세트를 제공하는 [Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/?WT.mc_id=pdmsolution-docs-ercenk)입니다. 데이터를 조작하기 위한 다른 옵션은 오픈 소스 라이브러리를 통해 Azure에서 사용할 수 있는 많은 ML 서비스에 있습니다.

ML 모드를 학습하는 경우 Microsoft Azure에서 다양한 옵션을 제공하며, 모두 서로 다른 조합으로 사용할 수 있습니다.

- [Azure Machine Learning Services](https://docs.microsoft.com/en-us/azure/machine-learning/preview/?WT.mc_id=pdmsolution-docs-ercenk)

- [Azure Machine Learning Studio](https://docs.microsoft.com/en-us/azure/machine-learning/studio/?WT.mc_id=pdmsolution-docs-ercenk)

- [Data Science Virtual Machine](https://docs.microsoft.com/en-us/azure/machine-learning/data-science-virtual-machine/?WT.mc_id=pdmsolution-docs-ercenk)

- [HDInsight의 Spark MLLib](https://docs.microsoft.com/en-us/azure/hdinsight/spark/apache-spark-machine-learning-mllib-ipython?WT.mc_id=pdmsolution-docs-ercenk)

- [Batch AI 학습 서비스](https://docs.microsoft.com/en-us/azure/batch-ai/?WT.mc_id=pdmsolution-docs-ercenk)

사용할 도구를 결정하는 것은 작업의 복잡성, 팀의 경험 및 데이터의 크기에 따라 달라집니다.

클라우드 솔루션의 비용 수식에는 추가 엔지니어링, 관리, 데이터 전송 등과 같은 클라우드 서비스 비용 외에도 많은 변수가 포함됩니다. 비용을 평가할 때는 이러한 변수를 사용하고, 합리적으로 결정합니다. 서비스는 총 비용 수식만 구성하지 않습니다.

데이터를 분석하고 모델을 게시하는 프로세스를 설계하는 것은 세부적인 주제이며 사용되는 기술에 따라 달라집니다. 이러한 주제는 이 문서의 범위를 벗어납니다. 모델 생성에 사용할 수 있는 프로세스 및 Azure 서비스를 설명하는 일련의 문서를 이용할 수 있습니다. 또한 Microsoft는 데이터 과학자 팀에서 데이터 수명 주기 동안 효과적으로 공동 작업할 수 있는 솔루션을 구축하기 위한 체계적인 방법을 제공합니다.

Microsoft의 [Machine Learning 설명서](https://docs.microsoft.com/en-us/azure/machine-learning?WT.mc_id=pdmsolution-docs-ercenk)는 클라우드에 ML 및 AI 모델을 작성, 배포 및 관리하는 옵션을 탐색하기 위한 좋은 시작 지점입니다.

Microsoft Azure 플랫폼은 대규모 데이터 처리 및 ML 모델 작성을 위한 다양한 선택 항목을 제공합니다. 클라우드 플랫폼에서 거의 무한하게 확장 가능한 컴퓨팅 및 스토리지 기능의 가용성을 통해 ML 및 AI 모델을 작성할 수 있습니다. 따라서 모델 작성에 Azure 서비스를 사용하는 것이 이 데이터 흐름을 구현하는 가장 논리적인 옵션입니다.

## <a name="using-the-model"></a>모델 사용

ML 모델이 갖춰지면 장비의 유지 관리에 대한 요구 사항을 예측하기 위해 이 모델을 소비(또는 "사용")하는 메커니즘이 필요합니다. 장비로부터 데이터를 받은 후 처리 계층을 통해 이동하여 향후의 오류 사례를 예측하고 유지 관리 팀에서 수행할 수 있는 다양한 수단을 제시합니다.

![모델 사용](assets/pdm-assets/usingthemodel.png)

데이터는 라이브 센서 데이터를 솔루션으로 스트림하여 온라인으로 수집하거나 센서 데이터를 솔루션으로 정기적으로 가져와서 오프라인으로 수집할 수 있습니다.

Microsoft Azure 플랫폼은 데이터의 수집, 처리 및 저장을 위해 다음과 같은 다양한 서비스를 제공합니다.

- [Azure Event Hubs](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-what-is-event-hubs?WT.mc_id=pdmsolution-docs-ercenk)

- [Azure Service Bus](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messaging-overview?WT.mc_id=pdmsolution-docs-ercenk)

- [Azure IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-what-is-iot-hub?WT.mc_id=pdmsolution-docs-ercenk)

- [HDInsight용 Apache Kafka](https://docs.microsoft.com/en-us/azure/hdinsight/kafka/apache-kafka-introduction?WT.mc_id=pdmsolution-docs-ercenk)

ML 모델을 작성하는 프로세스와 달리, 모델 사용에는 많은 계산 리소스가 필요하지 않습니다. 모델은 필요에 따라 클라우드의 서비스에 배포하거나 공장 현장에 로컬로 배포할 수 있습니다.

ML 모델이 실행되는 위치에 대해 로컬로 또는 클라우드에서만 실행되는 두 가지 주요 대안이 있습니다.

## <a name="local-execution"></a>로컬 실행

ML 모델은 로컬에서 사용되지만, 데이터는 수집, 저장 및 추가 처리를 위해 클라우드로 전송됩니다. 이 옵션은 초기 검색이 중요한 시나리오에 매우 적합합니다.

![로컬 실행](assets/pdm-assets/localandcloud.png)

## <a name="cloud-execution"></a>클라우드 실행

ML 모델의 수집, 처리, 저장 및 실행은 모두 Azure 클라우드에서 수행할 수 있습니다. 이 옵션은 다중 테넌트 또는 지역 간에 ML 모델의 실행 결과를 공유하는 경우에 더 적합할 수 있습니다(대기 시간은 중요하지 않음). 종종 "에지 게이트웨이"라고 하는 선택적 구성 요소는 ["특사(Ambassador)" 패턴](https://docs.microsoft.com/en-us/azure/architecture/patterns/ambassador?WT.mc_id=pdmsolution-docs-ercenk)으로 알려진 패턴에 따라 데이터 집계 및 프로젝션, 스트림 분석 등과 같은 일부 작업을 수행하기 위해 로컬로 추가할 수 있습니다.

Azure에서 모델을 사용하는 방법은 여러 가지가 있습니다. [Azure Machine Learning 웹 서비스](https://docs.microsoft.com/en-us/azure/machine-learning/studio/consume-web-services?WT.mc_id=pdmsolution-docs-ercenk)가 가장 간단하며, [Azure Machine Learning Studio](https://docs.microsoft.com/en-us/azure/machine-learning/studio/what-is-ml-studio?WT.mc_id=pdmsolution-docs-ercenk)는 모델을 만들기 위한 옵션으로 사용합니다. 또한 모델 관리를 위한 포괄적인 서비스 세트를 제공하는 [Azure Machine Learning 모델 관리](https://docs.microsoft.com/en-us/azure/machine-learning/preview/model-management-overview?WT.mc_id=pdmsolution-docs-ercenk)를 선택할 수 있으며, 이는 인증, 부하 분산, 자동 확장 및 암호화 기능이 포함된 REST API 엔드포인트를 제공합니다. 모델은 단일 머신(예: Data Science Virtual Machine, IoT 디바이스, 로컬 PC) 또는 [Azure Container Service](https://docs.microsoft.com/en-us/azure/aks/intro-kubernetes?WT.mc_id=pdmsolution-docs-ercenk)에 배포할 수 있습니다. 모델이 REST API를 통해 공개되면 사용자 지정 애플리케이션에서 엔터프라이즈 솔루션 통합에 이르기까지 무한하게 사용할 수 있습니다.

![클라우드 전용](assets/pdm-assets/cloudonly.png)

클라우드 전용 배포가 반드시 데이터를 라이브로 스트림한다는 것을 의미하지는 않습니다. 잠재적인 전략은 로컬 시스템(예: 이력 장치 또는 MES)에서 데이터를 정기적으로 내보내고 클라우드 시스템으로 가져와서 결과를 제시하는 것입니다. 이 옵션은 디바이스에서 데이터를 시스템에 직접 푸시할 수 없거나, 기존 시스템에서 이미 데이터를 수집하고 있거나, 실시간에 가까운 데이터 처리가 필요하지 않은 경우에 실행 가능한 방법이 될 수 있습니다. 이러한 경우 에지 게이트웨이를 고려할 필요가 없습니다.

## <a name="predictive-maintenance-in-the-iot-context"></a>IoT 컨텍스트의 예측 유지 관리

많은 IoT 솔루션은 해당 기능 세트의 일부로 데이터를 수집 및 저장합니다. 또한 예측 유지 관리 솔루션은 IoT 데이터를 사용하는 경우가 많으므로 IoT 솔루션에 자연스럽게 추가할 수 있습니다. 이 컨텍스트에서 강조해야 할 중요한 점은 기존 데이터에 오류를 기록하여 해당 오류에 대한 예측 모델을 학습하는 것이 중요하다는 것입니다.

일부 사용 사례에는 실시간에 가까운 데이터 처리가 필요합니다. 이러한 경우 높은 데이터 수집 속도 기능을 갖춘 확장성 있는 IoT 솔루션이 필요합니다. Microsoft Azure 플랫폼은 확장성이 뛰어난 IoT 요구 사항을 충족하는 솔루션을 사용하도록 설정할 수 있는 다양한 서비스를 제공합니다. Azure 플랫폼의 [Microsoft IoT 솔루션 아키텍처](https://docs.microsoft.com/en-us/azure/iot-suite/iot-suite-what-is-azure-iot?WT.mc_id=pdmsolution-docs-ercenk)에는 다음 3단계의 논리적 구성 요소가 있습니다.

- 장치 연결

- 데이터 처리 및 분석

- 프레젠테이션

![IoT 솔루션 아키텍처](assets/pdm-assets/iot.png)

Azure IoT 솔루션 아키텍처에 대한 자세한 내용은 [온라인으로 제공](http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf?WT.mc_id=pdmsolution-docs-ercenk)됩니다.
그러나 잠재적으로 상당한 수의 디바이스가 백 엔드 서비스에 연결되어 발생할 수 있는 고유한 문제가 있습니다.

## <a name="data-ingestion-and-stream-processing"></a>데이터 수집 및 스트림 처리

디바이스에서 데이터를 가져오는 것은 두 개의 개별 서비스, 즉 데이터를 생성하는 시스템(디바이스)과 이 데이터를 처리하는 시스템(즉, ML 모델을 학습하고, 학습된 모델에 들어오는 데이터 요소를 실행하여 잔여 수명을 예측함) 간의 통신 문제입니다.

정의에 따라 분산 시스템은 기본적으로 서로 통신해야 하는 고유한 구성 요소로 구성됩니다. 통신을 사용 하도록 설정하는 한 가지 옵션은 관련 구성 요소가 서로 직접 통신하는 것일 수 있습니다. 이 경우 유지 관리 및 크기 조정이 어려운 시스템이 만들어집니다. 구성 요소의 수가 증가하면 통신 링크에 대한 _O(n<sup>2</sup>)_ 복잡성이 발생합니다. 더 나은 방법은 데이터를 게시하고 공통 허브에서 읽거나 읽어들이는 것입니다.

![하위 구성 요소 통신](./assets/pdm-assets/subcomponentcommunication.png)

데이터 수집을 위해 새 구성 요소를 삽입하면 통신의 확장성이 향상됩니다. 이 구성 요소는 데이터 수집 프로세스를 지리적으로 분할하는 옵션을 사용하여 확장 가능하고, 안전하며, 글로벌로 액세스할 수 있어야 합니다. 

예측 유지 관리는 IoT 솔루션의 기능입니다. 데이터는 게이트웨이를 통해 스트림되므로 예측 유지 관리 기능과 관련된 서비스로 라우팅해야 합니다.
고려해야 할 또 다른 패턴은 [게이트웨이 라우팅](https://docs.microsoft.com/en-us/azure/architecture/patterns/gateway-routing?WT.mc_id=pdmsolution-docs-ercenk)입니다.

두 패턴은 모두 Azure 서비스인 [IoT Hub](https://azure.microsoft.com/en-us/services/iot-hub/?WT.mc_id=pdmsolution-docs-ercenk)와 [Azure Stream Analytics](https://azure.microsoft.com/en-us/services/stream-analytics/?WT.mc_id=pdmsolution-docs-ercenk)를 사용하여 구현할 수 있습니다.

## <a name="edge-and-cloud-processing-cooperation"></a>공동 작업을 처리하는 에지 및 클라우드

모든 디바이스와 장비에서 인터넷에 일관되게 직접 액세스할 수 있는 것은 아닙니다.
경우에 따라 공용 게이트웨이에서 데이터를 끌어와야 합니다. 예를 들어 [MTConnect](http://www.mtconnect.org/) 에이전트는 데이터를 끌어오는 REST 인터페이스만 제공합니다.

대기 시간, 디바이스 데이터를 클라우드로 보내기 전에 로컬로 스크럽해야 하는 요구 사항(다중 테넌트의 경우), 디바이스 데이터에 대한 프로젝션 또는 집계를 수행해야 하는 요구 사항과 같은 다른 고려 사항이 있을 수 있습니다. [특사 패턴](https://docs.microsoft.com/en-us/azure/architecture/patterns/ambassador?WT.mc_id=pdmsolution-docs-ercenk)은 이러한 요구 사항을 해결하는 좋은 방법입니다. [Microsoft Azure IoT Edge](https://docs.microsoft.com/en-us/azure/iot-edge/how-iot-edge-works?WT.mc_id=pdmsolution-docs-ercenk)는 [Microsoft Azure IoT Hub](https://azure.microsoft.com/en-us/services/iot-hub/?WT.mc_id=pdmsolution-docs-ercenk)의 프록시로 작동할 수 있을 뿐 아니라 원격 관리를 통해 로컬 처리 기능을 제공할 수도 있는 구현입니다.

일반적인 배포에서는 실시간에 가까운 작업 현장 경고가 포함되는 한편, 보관, 모델 학습 및 비시간 결정적 보고를 위해 여전히 데이터를 스크럽하고 클라우드의 다중 테넌트 솔루션에 게시할 수 있습니다. Azure IoT Edge 및 IoT Hub의 기능을 통해 고객은 에지 디바이스에서 데이터 필터링 옵션을 제어할 수 있을 뿐만 아니라 다른 현장 시스템과 상호 작용하여 경고를 전달할 수도 있습니다.

![다중 테넌트](assets/pdm-assets/multitenant.png)

## <a name="multitenant-perspective"></a>다중 테넌트 관점

앞에서 설명한 대로 일부 제조업체 또는 제3자는 고객에게 예측 유지 관리 서비스를 제공하려고 할 수도 있습니다. 이러한 서비스는 자체적인 일단의 과제를 제시하는 다중 테넌트 클라우드 배포에서 제공될 가능성이 높습니다.

### <a name="data-security-and-isolation"></a>데이터 보안 및 격리

서비스를 제공하는 당사자는 고객의 기밀 정보를 식별하고, 적절하게 보호하거나 스크럽해야 합니다. Microsoft Azure는 사용되는 스토리지 서비스에 따라 데이터를 암호화하는 기능을 제공합니다.

또한 디바이스에서 데이터를 생성하고 제출하는 방법은 디바이스별 인증서, 디바이스별 사용/사용 안 함, TLS 보안, X.509 지원, IP 허용/차단 목록 및 공유 액세스 정책과 같이 잘 알려진 방법을 사용하여 보호해야 합니다. 서비스를 제공하는 당사자는 고객의 기밀 정보를 식별하고, 적절하게 보호하거나 스크럽해야 합니다. [Azure Data Lake Store](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-encryption?WT.mc_id=pdmsolution-docs-ercenk), [Azure Storage](https://docs.microsoft.com/en-us/azure/storage/common/storage-service-encryption?WT.mc_id=pdmsolution-docs-ercenk), [Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/database-encryption-at-rest?WT.mc_id=pdmsolution-docs-ercenk) 및 [Azure SQL Database](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql?WT.mc_id=pdmsolution-docs-ercenk)는 저장 데이터를 암호화하는 데 사용할 수 있는 서비스의 예입니다. 솔루션 공급자는 동일한 리소스(예: 데이터베이스) 내 또는 여러 리소스 내에서 데이터를 분할하는 방법도 고려해야 합니다. 

### <a name="geographical-considerations"></a>지리적 고려 사항

데이터를 생성하는 디바이스는 지리적으로 분산되어 있을 가능성이 높습니다. 솔루션은 데이터 원본에 가장 가까운 수집 지점을 제공해야 합니다. 또한 지속적인 연결이 문제가 되고, 데이터를 대량으로 수집해야 하거나 로컬 저장 및 전달 메커니즘이 있어야 하는 경우도 있습니다.

### <a name="scalability"></a>확장성

ML 모델을 작성하려면 탄력적으로 크기를 조정할 수 있는 컴퓨팅 리소스가 필요합니다.
솔루션 공급자는 컴퓨팅 리소스를 효과적으로 사용하고 필요에 따라 솔루션 크기를 조정하는 프로세스를 고안해야 합니다.

### <a name="provisioning-tenants-and-secure-access"></a>테넌트 및 보안 액세스 프로비전

서비스 공급자는 새 테넌트를 효과적으로 온보딩하고 스스로 계정을 관리할 수 있는 수단을 제공하는 방법을 고안해야 합니다. 또한 전용 또는 공유 리소스에 대한 배포를 결정하는 시점이기도 합니다.


## <a name="pillars-of-software-quality-review"></a>소프트웨어 품질 핵심 요소에 대한 검토 

복잡한 시스템은 기능적 요구 사항을 충족하는 것 이외의 추가적인 정밀 조사가 필요합니다. 성공적인 클라우드 솔루션은 확장성, 가용성, 복원력, 관리 및 보안의 5가지 핵심 요소에 집중합니다. 또한 5가지 핵심 요소 외에도 솔루션의 비용 효율성도 제공하려고 합니다.

자세한 내용은 [소프트웨어 품질 핵심 요소](https://docs.microsoft.com/en-us/azure/architecture/guide/pillars?WT.mc_id=pdmsolution-docs-ercenk) 문서를 참조하세요.

| 핵심 요소                      |                                                                                                                                                                                                                                                                                                                                                                                                 |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 확장성                 | 대부분의 Azure 서비스는 수직 및 수평 크기 조정 옵션을 지원합니다. Azure 플랫폼에서 주문형 리소스 배포를 활용하고 자동화된 서비스를 통해 리소스의 크기 조정(인스턴스의 크기와 수)을 제어합니다.                                                                                                                                                                   |
| 가용성 및 복원력 | 컴퓨팅 및 스토리지 리소스는 많은 Azure 서비스를 사용하여 필요에 따라 탄력적으로 프로비전할 수 있습니다. 모든 Azure 서비스에서 다양한 수준의 SLA를 제공하지만, 솔루션은 적절한 설계 원칙을 사용하여 SLA 수준을 적절하게 고려하여 활용해야 합니다.                                                                                                                    |
| 관리                  | Azure 리소스는 ARM 템플릿, 명령줄 도구, PowerShell cmdlet 및 Azure 관리 API와 같은 다양한 방법으로 배포하고 관리할 수 있습니다. Azure 리소스를 관리하는 경우 UI가 있는 도구를 사용하는 대신 자동화된 솔루션을 구축하는 것이 좋습니다.                                                                                                                                |
| 보안                    | Azure IoT Hub는 TLS를 통한 대칭 및 비대칭 키(X509 인증서 및 TPM)를 지원합니다. 데이터 저장소는 IAM(ID 및 액세스 관리) 설정을 사용하여 보호되며 저장 데이터 암호화도 지원합니다. 높은 수준의 일반 보안 검사 목록인 권한 부여, 인증, 전송 및 저장 데이터 암호화, 감사 메커니즘을 검토하는 것이 좋습니다. |
| 비용 효율성              | 리소스는 필요할 때 프로비전하고, 자동화에서 사용하지 않을 때는 버리는 것이 좋습니다.                                                                                                                                                                                                                                                                                                  |

## <a name="conclusion"></a>결론

예측 유지 관리는 오랫동안 토론의 주제였습니다. 최근의 클라우드 플랫폼(예: Microsoft Azure) 개발을 통해 예측 유지 관리의 구현 담당자는 데이터를 처리할 때 과거에는 장애물이었던 많은 문제를 해결할 수 있습니다. 컴퓨팅 및 스토리지 용량의 크기를 탄력적으로 조정함으로써 클라우드 플랫폼은 새로운 수익 창출 기회와 함께 예측 유지 관리를 구현할 수 있는 새로운 기회를 제공합니다. Microsoft의 Azure 플랫폼은 예측 유지 관리 솔루션의 비즈니스 목표를 달성할 수 있도록 다양한 기능의 다양한 서비스를 제공합니다.

이 문서에서는 학습된 모델을 활용하여 이전 섹션에서 예측한 결과에 대한 작업을 수행하는 것과 함께, 데이터를 수집하고 데이터 모델을 학습하는 방법에 대한 비전을 제공했습니다.

## <a name="further-reading"></a>추가 참고 자료

1. [미래 지향: 과거의 생각을 멈추고 IoT를 통해 예기치 않은 상황을 미리 극복](https://blogs.microsoft.com/iot/2017/02/28/future-focused-stop-thinking-in-the-past-and-get-ahead-of-the-unexpected-with-iot-2/?WT.mc_id=pdmsolution-docs-ercenk)

2. [IoT 사용 예측 유지 관리를 통한 장비 안정성 향상](https://www.microsoft.com/en-us/internet-of-things/predictive-maintenance?WT.mc_id=pdmsolution-docs-ercenk)

3. [사물 인터넷에서 가치 획득: 예측 유지 관리 프로젝트에 대한 접근 방식](http://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF?WT.mc_id=pdmsolution-docs-ercenk)

4. [파트너 관점: 현장의 예측 유지 관리](https://blogs.microsoft.com/iot/2017/03/21/partner-perspectives-predictive-maintenance-on-the-frontlines/?WT.mc_id=pdmsolution-docs-ercenk)

5. [상품화에서 서비스화까지: 현장 서비스의 새로운 시대에서 경쟁할 수 있도록 IoT를 사용하여 비즈니스 전환](https://blogs.microsoft.com/iot/2016/11/07/from-commodization-to-servitization-transforming-your-business-to-compete-in-the-new-age-of-field-service-with-iot/?WT.mc_id=pdmsolution-docs-ercenk)
