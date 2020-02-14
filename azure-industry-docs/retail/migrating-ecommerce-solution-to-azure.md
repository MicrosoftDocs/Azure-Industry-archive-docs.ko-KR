---
title: Azure로 전자상거래 솔루션 마이그레이션 개요
author: scseely
ms.author: scseely
ms.date: 11/20/2019
ms.topic: article
ms.service: industry
description: 이 문서에서는 전자상거래 인프라를 온-프레미스에서 Azure로 마이그레이션하는 단계에 대해 설명합니다.
ms.openlocfilehash: e918f1157dc2bc42a6c4d0decfef95a8daa7ccf0
ms.sourcegitcommit: 3b175d73a82160c4cacec1ce00c6d804a93c765d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77054187"
---
# <a name="migrating-your-e-commerce-solution-to-azure-overview"></a>Azure로 전자상거래 솔루션 마이그레이션 개요

## <a name="introduction"></a>소개

기존의 전자상거래 솔루션을 클라우드로 이동하면 엔터프라이즈에 많은 혜택이 있습니다. 확장성을 사용할 수 있고, 고객에게 연중무휴 접근성이 제공되며, 클라우드 서비스를 보다 쉽게 통합할 수 있습니다. 그러나 전자상거래 솔루션을 클라우드로 이동하는 것은 의사 결정자가 이해해야 하는 비용이 있는 방대한 작업입니다. 이 문서에서는 옵션 정보 제공을 목표로, Azure 마이그레이션의 범위에 대해 설명합니다. 첫 번째 단계는 IT 전문가가 구성 요소를 클라우드로 이동하는 것으로 시작됩니다. Azure로 이동되고 나면, 전자상거래 팀이 ROI를 높이고 클라우드를 활용하기 위해 수행할 수 있는 단계를 설명합니다.

**교차로에서**

전 세계 전자상거래는 총 소매 판매의 일부에 불과하지만, 이 채널은 매년 꾸준히 성장하고 있습니다. 2017년에는 총 소매 판매의 10.2%로, 2016년의 8.6%에서 증가했습니다([출처](https://www.emarketer.com/Report/Worldwide-Retail-Ecommerce-Sales-eMarketers-Updated-Forecast-New-Mcommerce-Estimates-20162021/2002182)). 클라우드 컴퓨팅의 도입과 함께 전자상거래가 성숙함에 따라 소매점은 교차로에 서 있습니다.  이제 어느 길로 갈 것인지 선택해야 합니다. 기술이 발전하면서 사용 가능한 새로운 기능으로 비즈니스 모델을 구상할 수 있으며, 현재 기능 공간을 감안하여 현대화를 계획할 수 있습니다.

**고객 경험 개선**

주로 고객 경험에 중점을 두는 전자상거래에는 서로 다른 여러 특성이 있습니다. 이러한 특성은 검색, 평가, 구매 및 구매 후의 네 가지 주요 영역으로 그룹화할 수 있습니다.

고객 행동은 데이터로 캡처됩니다. 또한 쇼핑 유입 경로는 제품 데이터, 트랜잭션, 재고, 배송, 주문 이행, 고객 프로필, 쇼핑 카트, 제품 권장 사항 등을 보는 데 사용되는 애플리케이션에 대한 연결점 컬렉션입니다.

일반적인 소매 비즈니스는 고객 애플리케이션에서 스택을 통해 기본 애플리케이션까지 이르는 대규모 소프트웨어 솔루션 컬렉션을 이용합니다.  다음 그림은 일반적인 소매 비즈니스에 있는 기능을 보여 줍니다.

 ![](./assets/migrating-ecommerce-solution-to-azure/ecommerce-system-sketch.png)

클라우드는 조직이 기술을 확보, 사용 및 관리하는 방법을 전환할 수 있는 기회를 제공합니다.  기타 혜택에는 데이터 센터 유지 관리 비용 절감, 안정성 및 성능 향상, 다른 서비스를 추가할 수 있는 유연성 등이 있습니다. 이 사용 사례에서는 소매업체가 기존 인프라를 Azure로 마이그레이션하는 데사용할 수 있는 경로를 살펴보겠습니다. 또한 다시 호스트, 리팩터링, 다시 빌드의 단계별 접근 방법을 사용하여 새로운 환경을 활용합니다. 이러한 일련의 현대화 경로를 따르는 조직도 많지만, 대부분의 경우 조직이 모든 단계를 시작점으로 선택할 수 있습니다.  조직이 Azure에서 현재 애플리케이션을 다시 호스트하지 않고 리팩터링 또는 다시 빌드로 바로 이동할 수 있습니다.  이 결정은 현대화 요구를 가장 잘 충족하기 위한 애플리케이션 및 조직의 고유 결정입니다.

## <a name="rehost"></a>다시 호스트

&quot;리프트 앤 시프트&quot;라고도 하는 이 단계는 물리적 서버와 VM을 있는 그대로 클라우드로 마이그레이션합니다. 현재 서버 환경을 IaaS로 바로 이동하기만 해도 비용 절감, 보안 및 안정성 향상의 혜택을 얻을 수 있습니다. 비용 절감은 적절한 크기의 VM에서 워크로드를 실행하는 것과 같은 기술에서 발생합니다. 현재 온-프레미스 VM 및 물리적 머신의 기능은 소매점의 일상적인 요구를 초과하는 경우가 많습니다. VM은 1년에 몇 번에 불과한 비즈니스 성수기를 처리할 수 있어야 합니다. 이 때문에 사용량이 적은 시즌에는 사용하지 않는 기능을 위해 요금을 지불합니다. Azure에서는 현재 비즈니스 주기의 수요를 기반으로 하여 올바른 크기의 VM을 선택합니다.

Azure에서 다시 호스트하려면 다음 세 단계를 거쳐야 합니다.

- **분석**: 애플리케이션, 워크로드, 네트워킹, 보안 등의 온-프레미스 리소스를 확인하고 인벤토리를 작성합니다. 이 단계가 끝나면 기존 시스템의 완벽한 문서가 작성됩니다.
- **마이그레이션**: 각 하위 시스템을 온-프레미스에서 Azure로 이동합니다. 이 단계에서는 Azure를 데이터 센터의 확장으로 사용하고 애플리케이션이 계속 통신합니다.
- **최적화**: 시스템이 Azure로 이동함에 따라 크기가 제대로 조정되었는지 확인합니다. 환경에서 일부 VM에 너무 많은 리소스가 할당되었다고 표시되는 경우 VM 유형을 CPU, 메모리 및 로컬 스토리지가 더 적절하게 조합된 VM 유형으로 변경합니다.

### <a name="analyze"></a>분석

다음을 수행합니다.

1. 온-프레미스 서버 및 애플리케이션을 나열합니다. 이렇게 하려면 에이전트 또는 관리 도구를 사용하여 서버, 서버에서 실행되는 애플리케이션, 현재 서버 사용량 및 서버와 해당 애플리케이션의 구성 방법에 대한 메타데이터를 수집해야 합니다. 이 결과는 환경에 있는 모든 서버 및 애플리케이션에 대한 보고서입니다.
1. 종속성을 파악합니다. 도구를 사용하여 서로 대화하는 서버와 서로 통신하는 애플리케이션을 파악할 수 있습니다. 그 결과는 모든 애플리케이션 및 워크로드의 맵입니다. 이러한 맵은 마이그레이션 계획에 피드됩니다.
1. 구성을 분석합니다. 분석 목표는 Azure에서 실행한 다음 필요한 VM 유형을 알기 위한 것입니다. 그 결과는 Azure로 이동할 수 있는 모든 애플리케이션에 대한 보고서입니다. 다음과 같이 추가로 분류할 수 있습니다.
      1. 수정 사항 없음
      1. 이름 변경과 같은 기본 수정 사항
      1. 약간의 코드 변경과 같은 사소한 수정 사항
      1. 이동을 위해 추가 노력이 필요한 호환되지 않는 워크로드
1. 예산을 만듭니다. 이제 각 CPU, 메모리 등과 각 애플리케이션의 요구 사항을 열거하는 목록이 있습니다. 해당 워크로드를 적절한 크기의 VM에 배치합니다. 클라우드 플랫폼은 사용량을 기준으로 비용을 청구합니다. 사용자 요구를 적절한 크기의 Azure VM에 매핑하기 위한 도구가 있습니다. Windows VM 또는 SQL Server를 마이그레이션하는 경우 Azure에서 사용자 비용을 줄이는 [Azure 하이브리드 혜택](https://azure.microsoft.com/pricing/hybrid-benefit/?WT.mc_id=retailecomm-docs-scseely)도 살펴봐야 합니다.

Microsoft는 시스템을 분석하고 카탈로그를 작성하기 위한 몇 가지 도구를 제공합니다. VMWare를 실행하는 경우 [Azure Migrate](/azure/migrate/migrate-overview?WT.mc_id=retailecomm-docs-scseely)를 사용하여 검색 및 평가를 지원할 수 있습니다. 이 도구는 Azure로 이동할 수 있는 머신을 확인하고, 실행할 VM 유형을 권장하며, 워크로드 비용을 예측합니다. Hyper-V 환경에서는 [AAzure Site Recovery Deployment Planner](/azure/site-recovery/hyper-v-deployment-planner-overview?WT.mc_id=retailecomm-docs-scseely)를 사용합니다. 수백 개 이상의 VM을 이동해야 하는 대규모 마이그레이션의 경우 [Azure 마이그레이션 파트너](https://azure.microsoft.com/migration/partners/?WT.mc_id=retailecomm-docs-scseely)와 함께 작업하는 것이 좋습니다. 이러한 파트너는 워크로드를 이동하기 위한 전문 지식과 경험을 보유하고 있습니다.

### <a name="migrate"></a>마이그레이션

클라우드로 이동할 서비스와 이동 순서에 대한 계획을 시작합니다. 이 단계에서는 워크로드를 이동해야 하므로 다음 순서를 따릅니다.

1. 네트워크를 빌드합니다.
2. ID 시스템(Azure Active Directory)을 통합합니다.
3. Azure에서 스토리지 영역을 프로비전합니다.

마이그레이션 중에 Azure 환경은 온-프레미스 네트워크의 확장입니다. 논리 네트워크를 [Azure Virtual Network](/azure/virtual-network/virtual-networks-overview?WT.mc_id=retailecomm-docs-scseely)에 연결할 수 있습니다. [Azure ExpressRoute](/azure/expressroute/?WT.mc_id=retailecomm-docs-scseely)를 사용하여 사용자 네트워크와 Azure 간의 통신을 인터넷에 연결되지 않는 프라이빗 연결에 유지하도록 선택할 수 있습니다. Azure VPN Gateway가 온-프레미스 VPN 디바이스에 대화하고, Azure와 네트워크 간에 암호화된 통신을 사용하여 모든 트래픽이 안전하게 전송되는 사이트 간 VPN을 사용할 수도 있습니다. 하이브리드 네트워크를 설정하는 방법을 자세히 설명하는 참조 아키텍처가 [여기](/azure/architecture/reference-architectures/hybrid-networking/vpn?WT.mc_id=retailecomm-docs-scseely)에 게시되었습니다.

네트워크가 구성되고 나면 비즈니스 연속성을 계획합니다. 실시간 복제를 사용하여 온-프레미스 데이터를 클라우드로 이동하고 클라우드와 기존 데이터가 동일한지 확인하는 것이 좋습니다. 전자상거래 상점은 연중무휴입니다. 중복은 고객에게 미치는 영향을 최소화하여 온-프레미스에서 Azure로 전환하는 기능을 제공합니다.

데이터, 애플리케이션 및 관련 서버를 Azure로 이동하기 시작합니다. 대부분의 회사는 [Azure Site Recovery](/azure/site-recovery/site-recovery-overview?WT.mc_id=retailecomm-docs-scseely) 서비스를 사용하여 Azure로 마이그레이션합니다. 이 서비스는 BCDR(비즈니스 연속성 및 재해 복구)을 목표로 합니다. 온-프레미스에서 Azure로의 마이그레이션하는 데 이상적입니다. 구현 팀은 온-프레미스 VM 및 물리적 서버를 Azure로 마이그레이션하는 방법에 대한 세부 정보를 [여기](/azure/site-recovery/migrate-tutorial-on-premises-azure?WT.mc_id=retailecomm-docs-scseely)에서 확인할 수 있습니다.

하위 시스템이 Azure로 이동되고 나면 모든 것이 예상대로 작동하는지 테스트하여 확인합니다. 모든 문제가 닫히고 나면 워크로드를 Azure로 이동합니다.

### <a name="optimize"></a>최적화

이 시점에는 환경을 계속 모니터링하고, 환경이 변경됨에 따라 기본 컴퓨팅 옵션을 워크로드에 맞게 변경합니다. 환경 상태를 모니터링하는 사람은 사용되는 각 리소스의 양을 감시해야 합니다. 목표는 대부분의 VM에서 사용률을 75-90%로 유지하는 것이어야 합니다. 사용률이 매우 낮은 VM에서는 더 많은 애플리케이션을 채우거나, Azure에서 적절한 수준의 성능을 유지하는 최저 비용 VM으로 마이그레이션하는 것이 좋습니다.

Azure는 환경을 최적화하는 도구도 제공합니다. [Azure Advisor](/azure/advisor/advisor-overview?WT.mc_id=retailecomm-docs-scseely)는 환경 구성 요소를 모니터링하고 모범 사례를 기반으로 하여 개인 설정된 권장 사항을 제공합니다. 권장 사항은 애플리케이션에서 사용되는 리소스의 성능, 보안 및 가용성을 개선하는 데 도움이 됩니다. Azure Portal에도 애플리케이션 상태에 대한 정보가 표시됩니다. VM은 [Linux 및 Windows용 Azure 가상 머신 확장](/azure/virtual-machines/extensions/overview?WT.mc_id=retailecomm-docs-scseely)을 활용해야 합니다. 이러한 확장은 배포 후 구성, 바이러스 백신, 앱 모니터링 등을 제공합니다. [Network Watcher](/azure/network-watcher/network-watcher-monitoring-overview?WT.mc_id=retailecomm-docs-scseely), [서비스 맵](/azure/monitoring/monitoring-walkthrough-servicemap?WT.mc_id=retailecomm-docs-scseely), [Application Insights](/azure/application-insights/app-insights-overview?WT.mc_id=retailecomm-docs-scseely) 및 [Log Analytics](/azure/log-analytics/log-analytics-overview?WT.mc_id=retailecomm-docs-scseely)와 같은 서비스를 통해 다양한 Azure 서비스를 네트워크 진단, 서비스 사용 및 경고에 활용할 수도 있습니다.

조직의 일부가 현재 Azure에서 시스템을 최적화하는 동안 개발 팀은 마이그레이션 후 단계인 리팩터링으로 이동하기 시작할 수 있습니다.

## <a name="refactor"></a>리팩터링

마이그레이션이 완료되면 전자상거래 애플리케이션이 Azure의 새로운 홈을 활용할 수 있습니다. 리팩터링 단계는 전체 환경이 이동될 때까지 대기할 필요가 없습니다. CMS 팀은 마이그레이션되었지만 ERP 팀이 마이그레이션되지 않았다고 해도 문제가 되지 않습니다. CMS 팀은 리팩터링 작업을 시작할 수 있습니다. 이 단계에서는 추가 Azure 서비스를 사용해 애플리케이션을 리팩터링하여 비용, 안정성 및 성능을 최적화합니다. 리프트 앤 시프트에서는 공급자가 관리하는 하드웨어 및 OS만 활용했지만, 이 모델에서는 비용 절감을 위해 클라우드 서비스도 활용합니다. 애플리케이션 코드 또는 구성만 약간 변경하여 현재 애플리케이션을 있는 그대로 계속 이용하고, 컨테이너, 데이터베이스, ID 관리 시스템 등의 새 인프라 서비스에 애플리케이션을 연결합니다.

리팩터링 작업은 코드 및 구성을 약간만 변경합니다. 이 단계에서 채택된 기술은 스크립팅을 사용하여 리소스를 빌드하고 배포하기 때문에 자동화에 더 많은 시간을 집중합니다. 배포 지침이 스크립트입니다.

많은 Azure 서비스를 사용할 수 있지만 리팩터링 단계에서 사용되는 가장 일반적인 서비스(컨테이너, 앱 서비스 및 데이터베이스 서비스)에 중점을 둡니다. 리팩터링을 고려하는 이유는 무엇인가요? 리팩터링은 코드 부채를 타당한 이유 이내로 유지하여 장기 비용을 낮추는 강력한 코드 기반을 제공합니다.

컨테이너는 애플리케이션을 번들하는 방법을 제공합니다. 컨테이너가 운영 체제를 가상화하는 방법 때문에 여러 컨테이너를 단일 VM으로 압축할 수 있습니다. 코드를 전혀 또는 거의 변경하지 않고 애플리케이션을 컨테이너로 이동할 수 있습니다. 구성은 변경해야 할 수도 있습니다. 또한 이러한 노력은 애플리케이션을 컨테이너에 번들하는 스크립트 작성까지 이어집니다. 개발 팀은 이러한 스크립트를 작성하고 테스트하는 데 리팩터링 시간을 할애합니다. Azure는 AKS([Azure Kubernetes Service](/azure/aks/?WT.mc_id=retailecomm-docs-scseely)) 및 컨테이너 이미지 관리에 사용할 수 있는 관련 [AAzure Container Registry](https://azure.microsoft.com/services/container-registry/?WT.mc_id=retailecomm-docs-scseely)를 통해 컨테이너 처리를 지원합니다.

앱 서비스의 경우 다양한 Azure 서비스를 활용할 수 있습니다. 예를 들어 기존 인프라는 메시지를 [RabbitMQ](https://www.rabbitmq.com/)와 같은 큐에 배치하여 고객 주문을 처리할 수 있습니다. 예를 들어 고객에 게 요금을 부과 하는 한 가지 메시지는 주문을 배송 하는 것입니다. 재호스팅 때 RabbitMQ를 별도의 VM에 배치 합니다. 리팩터링 중에 [Service Bus](/azure/service-bus-messaging/service-bus-queues-topics-subscriptions?WT.mc_id=retailecomm-docs-scseely) 큐 또는 토픽을 솔루션에 추가하고 RabbitMQ 코드를 다시 작성한 다음, 큐 기능을 제공한 VM 사용을 중지합니다. 이렇게 변경하면 VM 집합이 상시 메시지 큐 서비스로 대체되어 비용이 절감됩니다. 기타 앱 서비스는 Azure Portal에서 확인할 수 있습니다.

데이터베이스의 경우 VM에서 서비스로 데이터베이스를 이동할 수 있습니다. Azure는 [Azure SQL Database](/azure/sql-database/sql-database-cloud-migrate?WT.mc_id=retailecomm-docs-scseely) 및 [Azure SQL Database Managed Instance](/azure/sql-database/sql-database-managed-instance?WT.mc_id=retailecomm-docs-scseely)를 사용하여 SQL Server 워크로드를 지원합니다. [데이터 마이그레이션 서비스](https://azure.microsoft.com/services/database-migration/?WT.mc_id=retailecomm-docs-scseely)는 데이터베이스를 평가하고 마이그레이션 전에 수행되어야 하는 작업을 알려 준 다음, 데이터베이스를 VM에서 서비스로 이동합니다. Azure는 [MySQL](https://azure.microsoft.com/services/mysql/?WT.mc_id=retailecomm-docs-scseely), [PostgreSQL](https://azure.microsoft.com/services/postgresql/?WT.mc_id=retailecomm-docs-scseely) 및 [기타 데이터베이스](https://azure.microsoft.com/services/#databases?WT.mc_id=retailecomm-docs-scseely) 엔진 서비스도 지원합니다.

## <a name="rebuild"></a>다시 작성

지금까지는 전자상거래 시스템에 대한 변경을 최소화하려고 했습니다. 작동하는 시스템을 그대로 두었습니다. 이제 실제로 클라우드를 활용하는 방법을 알아보겠습니다. 이 단계에서는 PaaS 또는 SaaS 서비스와 아키텍처를 적극적으로 채택하여 기존 애플리케이션을 수정합니다. 이 프로세스에는 새 기능을 추가하거나 클라우드용으로 애플리케이션 아키텍처를 변경하기 위한 대규모 수정 작업이 수반됩니다.  _관리되는 API_는 클라우드 시스템을 활용하는 새로운 개념입니다. 서비스 간 통신을 위한 API를 만들어 시스템을 보다 쉽게 업데이트할 수 있습니다.  두 번째 혜택은 보유한 데이터에 대한 인사이트를 얻을 수 있는 기능입니다. 이렇게 하려면 _마이크로 서비스 + API_ 아키텍처로 이동하고 기계 학습 및 기타 도구를 사용하여 데이터를 분석합니다.

### <a name="microservices--apis"></a>마이크로 서비스 + API

마이크로 서비스는 외부에 연결되는 API를 통해 통신합니다. 각 서비스는 자체 포함되며 단일 비즈니스 기능(예: 고객에게 항목 추천, 쇼핑 카트 유지 관리 등)을 구현해야 합니다. 애플리케이션을 마이크로 서비스로 분해하려면 시간과 계획이 필요합니다. 마이크로 서비스를 정의하기 위한 하드 규칙은 없지만, 일반적으로 배포 가능한 단위를 거의 항상 함께 변경되는 구성 요소 집합까지 축소해야 합니다. 마이크로 서비스를 사용하면 전체 애플리케이션을 테스트하는 부담을 줄이는 동시에 필요에 따라 변경 내용을 자주 배포할 수 있습니다. 일부 서비스는 매우 작을 수 있습니다. 이러한 서비스의 경우 [Azure Functions](/azure/azure-functions/functions-overview?WT.mc_id=retailecomm-docs-scseely)를 사용하여 서버리스로 전환하면 사용하지 않을 때는 리소스를 이용하지 않는 동시에 필요한 만큼 호출자 수를 확장하는 데 효율적입니다. 기타 서비스는 제품 관리, 고객 주문 캡처 등 비즈니스 기능을 중심으로 분석됩니다.

서버리스 메커니즘에는 결함이 있습니다. 부하가 적을 때 클라우드의 일부 서버가 코드를 구성하고 실행하는 데 몇 초 정도 걸려 응답 속도가 느릴 수 있습니다. 환경에서 고객이 많이 사용하는 부분의 경우 빠르고 쉽게 제품을 찾고, 주문을 넣고, 반품을 요청할 수 있도록 하는 것이 좋습니다. 성능이 저하될 때마다, 쇼핑 유입 경로에서 고객을 잃게 될 위험이 있습니다. 빠르게 응답해야 하는 기능이 있는 경우 [Azure Kubernetes Service](/azure/aks/?WT.mc_id=retailecomm-docs-scseely)에서 해당 기능을 개별적으로 배포 가능한 단위로 다시 빌드합니다.  많은 메모리, 여러 CPU, 많은 로컬 스토리지의 일부 조합이 필요한 서비스와 같은 경우에는 마이크로 서비스를 자체 VM에서 호스트하는 것이 타당할 수 있습니다.

각 서비스는 상호 작용을 위해 API를 사용합니다. 마이크로 서비스는 API에 직접 액세스할 수 있지만, 이 경우 서비스와 통신하는 모든 사용자가 애플리케이션 토폴로지를 알고 있어야 합니다. [API Management](/azure/api-management/?WT.mc_id=retailecomm-docs-scseely)와 같은 서비스는 중앙에서 API를 게시하는 방법을 제공합니다. 모든 애플리케이션은 API Management 서비스에 연결하기만 하면 됩니다. 개발자가 사용 가능한 API를 검색할 수 있습니다. API Management 서비스는 소매 환경의 성능을 높이는 기능도 제공합니다. 서비스는 애플리케이션 부분별로 API 액세스를 제한하고(병목 현상 방지), 느리게 변경되는 값에 대한 응답을 캐시하며, JSON에서 XML로 변환할 수 있습니다. 정책의 전체 목록은 [여기](/azure/api-management/api-management-policies?WT.mc_id=retailecomm-docs-scseely)에서 확인할 수 있습니다.

### <a name="make-use-of-your-data-and-the-azure-marketplace"></a>데이터 및 Azure Marketplace 사용

모든 데이터와 시스템이 Azure에 있으므로 다른 SaaS 솔루션을 비즈니스에 쉽게 통합할 수 있습니다. 일부 작업은 즉시 수행할 수 있습니다. 예를 들어 [Power BI](https://powerbi.microsoft.com/?WT.mc_id=retailecomm-docs-scseely)를 사용해 다양한 데이터 원본을 연결하여 시각화 및 보고서를 만들고 인사이트를 얻습니다.

그런 다음, [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?WT.mc_id=retailecomm-docs-scseely)에서 제품을 살펴보면 인벤토리 최적화, 고객 특성을 기준으로 캠페인 관리, 기본 설정 및 기록을 기준으로 각 고객에게 올바른 항목 제공 등의 작업을 수행하는 데 도움이 됩니다. Marketplace 제품에서 작동하도록 데이터를 구성하는 데 시간이 어느 정도 소요됩니다.

## <a name="next-steps"></a>다음 단계

많은 개발 팀은 기술 부채를 처리하고 용량을 보다 효율적으로 이용하기 위해 다시 호스트와 리팩터링을 동시에 수행하려고 합니다. 다음 단계로 이동하기 전에 다시 호스트할 경우의 혜택이 있습니다.  새 환경에 배포 시의 문제를 더 쉽게 진단하고 해결할 수 있습니다. 따라서 개발 및 지원 팀이 Azure를 새 환경으로 사용하여 확장할 수 있는 시간이 제공됩니다. 시스템을 리팩터링하고 다시 빌드하기 시작할 때는 안정적으로 작동하는 애플리케이션에서 빌드하게 됩니다. 이 경우 대상이 지정된 더 작은 변경과 보다 빈번한 업데이트가 가능합니다.

클라우드로 마이그레이션하는 방법에 대한 보다 일반적인 백서인 [Cloud Migration Essentials](https://azure.microsoft.com/resources/cloud-migration-essentials-e-book/?_lrsc=9618a836-9f81-4087-901f-51058783c3a8&WT.mc_id=retailecomm-docs-scseely)(클라우드 마이그레이션 필수 사항)가 게시되었습니다. 마이그레이션을 계획할 때 이 백서를 읽어보는 것이 좋습니다.

## <a name="technologies-presented"></a>제공되는 기술

다시 호스트 중에 사용되는 기술:

- [Azure Advisor](/azure/advisor/advisor-overview?WT.mc_id=retailecomm-docs-scseely)는 Azure 배포를 최적화하기 위한 모범 사례를 따르는 데 도움이 되는 개인 설정된 클라우드 컨설턴트입니다.
- [Azure Migrate](/azure/migrate/migrate-overview?WT.mc_id=retailecomm-docs-scseely) 서비스는 Azure로 마이그레이션하기 위해 온-프레미스 워크로드를 평가합니다.
- [Azure Site Recovery](/azure/site-recovery/site-recovery-overview?WT.mc_id=retailecomm-docs-scseely)는 Azure VM, 온-프레미스 VM 및 물리적 서버에 대한 재해 복구를 오케스트레이션하고 관리합니다.
- [Azure Virtual Network](/azure/virtual-network/virtual-networks-overview?WT.mc_id=retailecomm-docs-scseely)를 사용하면 Azure VM(Virtual Machines)과 같은 다양한 형식의 Azure 리소스가 서로, 인터넷 및 특정 온-프레미스 네트워크와 안전하게 통신할 수 있습니다.
- [Azure ExpressRoute](/azure/expressroute/?WT.mc_id=retailecomm-docs-scseely)를 사용하면 연결 공급자가 지원하는 프라이빗 연결을 통해 온-프레미스 네트워크를 Microsoft 클라우드로 확장할 수 있습니다.

리팩터링 중에 사용되는 기술:

- [Azure Kubernetes Service](/azure/aks/?WT.mc_id=retailecomm-docs-scseely)는 호스트된 Kubernetes 환경을 관리하여, 컨테이너 오케스트레이션에 대한 전문 지식이 없어도 컨테이너화된 애플리케이션을 빠르고 쉽게 배포하고 관리할 수 있게 합니다.
- [Azure SQL Database](/azure/sql-database/sql-database-technical-overview?WT.mc_id=retailecomm-docs-scseely)는 Microsoft Azure의 범용 관계형 데이터베이스 관리 서비스입니다. 관계형 데이터, JSON, 공간 및 XML과 같은 구조를 지원합니다. SQL Database는 관리되는 단일 SQL 데이터베이스, 탄력적 풀의 관리되는 SQL 데이터베이스 및 SQL Managed Instances를 제공합니다.

다시 빌드 중에 사용되는 기술:

- Azure [API Management](/azure/api-management/?WT.mc_id=retailecomm-docs-scseely)를 통해 조직은 외부, 파트너 및 내부 개발자에게 API를 게시하여 데이터 및 서비스의 잠재성을 활용할 수 있습니다.
- [Azure Functions](/azure/azure-functions/functions-overview?WT.mc_id=retailecomm-docs-scseely)는 클라우드에서 작은 코드 또는 &quot;함수&quot;를 쉽게 실행하기 위한 솔루션입니다.
- [Power BI](https://powerbi.microsoft.com/?WT.mc_id=retailecomm-docs-scseely)는 조직 전체에 인사이트를 전달하는 비즈니스 분석 도구 모음입니다.

## <a name="conclusion"></a>결론

전자상거래 시스템을 Azure로 이동하는 경우 분석, 계획 및 정의된 접근 방식이 필요합니다. 다시 호스트, 리팩터링 및 다시 빌드의 세 단계 접근 방식을 살펴보았습니다. 이 경우 조직이 각 단계의 변경량을 최소화하면서 한 작업 상태에서 다른 작업 상태로 이동할 수 있습니다. 소매점은 다시 호스트를 완전히 건너뛰고 구성 요소를 리팩터링하거나 다시 빌드하도록 선택할 수도 있습니다. 대부분의 경우 명확한 현대화 경로가 제공되며, 가능할 때 진행하면 됩니다. Azure에서 실행 경험이 축적되면 새로운 기능을 추가하고, 비용을 절감하며, 전반적인 시스템을 개선할 수 있는 기회가 더 많아집니다.


_이 문서는 Scott Seely 및 Mariya Zorotovich에 의해 작성되었습니다._