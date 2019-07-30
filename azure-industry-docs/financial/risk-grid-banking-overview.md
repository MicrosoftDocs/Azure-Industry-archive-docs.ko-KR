---
title: 금융의 위험 그리드 컴퓨팅 개요
author: dstarr
ms.author: dastarr
ms.date: 04/12/2018
ms.topic: article
ms.service: industry
description: Azure에서 금융의 위험 그리드 컴퓨팅을 구현하는 경우의 비즈니스 고려 사항을 제공합니다.
ms.openlocfilehash: 49d3d5223bee85689043d84eb5236cca4f53fc03
ms.sourcegitcommit: 76f2862adbec59311b5888e043a120f89dc862af
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "51654180"
---
# <a name="risk-grid-computing-in-banking-overview"></a>금융의 위험 그리드 컴퓨팅 개요

## <a name="introduction"></a>소개

기업의 재무 및 투자 금융에서 가장 중요한 작업 중 하나는 위험 분석입니다.

투자 포트폴리오와 관련된 위험을 포괄적으로 파악하기 위해 재무 위험 분석가는 조사를 검토하고, 경제 및 사회 상황을 모니터링하며, 규제를 예측하고, 투자 환경의 컴퓨터 모델을 만듭니다.

포트폴리오에 영향을 주는 많은 벡터에 대한 위험 분석은 매우 복잡하므로 컴퓨터 모델링이 필요합니다. 대부분의 분석가는 컴퓨터 모델 작업에 많은 시간을 할애하여 재무 상황이 어떻게 변화하는지를 시뮬레이션하고 예측합니다. 투자 위험(시장 위험, 신용 위험 및 운영 위험)을 평가할 때 데이터의 볼륨 및 다양성으로 인해 예측 모델을 처리하는 계산 부하가 상당히 클 수 있습니다.

클라우드 컴퓨팅은 분석가가 자본 비용을 발생시키거나 인프라를 관리하지 않고도 요청 시 방대한 계산 리소스에 액세스할 수 있으므로 위험 그리드 컴퓨팅 또는 위험 모델링에 상당한 혜택을 제공합니다. 이 문서에서는 Microsoft Azure를 활용하여 현재 위험 그리드 컴퓨팅 리소스를 보강하고 위험 그리드 컴퓨팅 워크로드의 비용과 속도를 최적화하는 방법을 살펴보겠습니다. 안전하고 신뢰할 수 있는 연결, 일괄 처리, 온-프레미스 서버의 용량에 대한 수요에 따라 컴퓨팅 리소스 증대 등의 주제가 다루어집니다.

## <a name="grid-computing-services"></a>그리드 컴퓨팅 서비스

분석가는 데이터 수집을 시작하고 데이터 처리를 통해 분석으로 진행하여 결과 데이터에서 인사이트를 얻을 수 있는 일괄 처리 파이프라인에 모델을 제공하는 간단하고 신뢰할 수 있는 방법이 필요합니다.

위험 모델 입력 데이터는 여러 형태로 제공되며, 가장 일반적인 형태는 Excel 파일 또는 .csv 파일입니다. 이러한 파일은 위험 컴퓨팅 파이프라인의 이후 단계에서 위험 모델을 처리하는 데 보다 적합한 형식으로 재구성되는 경우가 많습니다. 해당 파일을 구문 분석하고 처리하는 일반적인 기술은 일괄 처리로, VM(가상 머신?WT.mc_id=gridbank-docs-dastarr) 그리드가 공통 목표에 도달하기 위해 함께 작동합니다.

[Azure Batch](/azure/batch/?WT.mc_id=gridbank-docs-dastarr)는 아래와 같이 여러 작업자 VM이 병렬로 실행될 수 있도록 하는 Azure 서비스입니다. 데이터 파일을 처리하고 결과를 기계 학습 시스템 또는 데이터 저장소에 제출하는 것이 작업자 노드의 일반적인 작업입니다. 작업자 노드에서 실행하는 애플리케이션 코드는 고객이 만들기 때문에 일괄 작업에서 거의 모든 작업을 수행할 수 있습니다.

![온-프레미스 일괄 처리](./assets/risk-grid-compute-assets/01-on-prem.png)

Azure는 Azure Batch를 사용하여 위험 그리드 컴퓨팅을 위한 유연한 솔루션을 제공합니다. 고객은 Azure Batch를 사용하여 기존 위험 컴퓨팅 그리드를 확장하거나 온-프레미스 리소스를 완전한 클라우드 기반 솔루션으로 대체할 수 있습니다.

Azure 클라우드에 직접 안전하게 연결하는 기능이 완전히 지원됩니다. Batch 위험 처리 그리드 작업자 노드는 하이브리드 네트워크를 통해 Azure에 연결하는 경우 온-프레미스에 저장된 데이터에 연결할 때 모델링 데이터에 액세스할 수 있습니다. 또한 고객은 Azure 내의 적절한 스토리지에 데이터를 업로드하여 Batch가 데이터에 직접 액세스하도록 허용할 수 있습니다.

## <a name="secure-connectivity-to-azure"></a>Azure에 대한 보안 연결

Azure에서 위험 그리드 컴퓨팅 솔루션을 빌드할 때 비즈니스에서 거래 시스템, 중간 사무실 위험 관리, 위험 분석 등의 기존 온-프레미스 애플리케이션을 계속 사용하는 경우가 많습니다. Azure는 이러한 기존 투자에 대한 확장이 됩니다.

클라우드에 연결할 때 주요 고려 사항은 보안입니다. 현재 보안 모델을 고려하는 것이 Azure에 직접 연결하는 과정의 첫 번째 단계입니다. 이미 온-프레미스에서 AD(Active Directory?WT.mc_id=gridbank-docs-dastarr)를 사용 중인 고객의 경우 Azure에 연결할 때 기존 ID 리소스를 활용할 수 있습니다. 서비스 계정은 온-프레미스 AD에 상주할 수 있습니다.

### <a name="hybrid-network-solution"></a>하이브리드 네트워크 솔루션

하이브리드 네트워크는 Azure를 고객의 온-프레미스 네트워크에 직접 연결합니다. Azure는 현재 온-프레미스 시스템을 Azure, [Microsoft Azure ExpressRoute](/azure/expressroute/expressroute-introduction?WT.mc_id=gridbank-docs-dastarr) 및 [VPN Gateway](/azure/vpn-gateway/?WT.mc_id=gridbank-docs-dastarr)에 안전하고 안정적으로 연결하기 위한 두 가지 모델을 제공합니다. 구현, 성능, 비용 및 기타 특성에서 차이가 있긴 하지만 둘 다 신뢰할 수 있는 연결 솔루션입니다.

![Azure 연결](./assets/risk-grid-compute-assets/02-connectivity.png)

&quot;클라우드로 확장&quot;은 기존 리소스가 급증할 때 컴퓨팅 작업을 클라우드 기반 시스템에 오프로드하여 고객의 데이터 센터 또는 사설 클라우드 리소스를 보강합니다. 클라우드 기반 위험 컴퓨팅 그리드는 기존 네트워크의 간단한 확장이므로 하이브리드 네트워크 모델을 사용하면 클라우드 시나리오로 쉽게 확장할 수 있습니다.

위의 논리 아키텍처에 제공된 간단한 모델의 구성 외에도 여러 가지 네트워크 연결 구성이 있습니다. 네트워크를 Azure에 연결하는 방법과 관련된 의사 결정 및 아키텍처 지침을 보려면 [_Azure에 온-프레미스 네트워크 연결_](/azure/architecture/reference-architectures/hybrid-networking/) 문서를 참조하세요.

### <a name="rest-api-solution-over-internet"></a>인터넷을 통한 REST API 솔루션

하이브리드 네트워크를 만드는 대신 데이터를 Azure Storage(File 또는 Blob Storage일 가능성이 큼?WT.mc_id=gridbank-docs-dastarr)에 업로드하고 Batch를 통해 스토리지에서 데이터 파일을 읽을 수 있습니다. 이렇게 하려면 보안(SSL?WT.mc_id=gridbank-docs-dastarr) 연결을 사용하여 Azure에 연결하고, 문서를 Azure Storage에 저장한 다음, [Batch 서비스 REST API](/rest/api/batchservice/?WT.mc_id=gridbank-docs-dastarr) 또는 목적에 맞는 애플리케이션이 포함된 SDK를 통해 위험 그리드 컴퓨팅 작업을 관리하고, Batch 실행을 오케스트레이션합니다.

### <a name="azure-data-factory"></a>Azure 데이터 팩터리

시나리오에 대한 또 다른 솔루션은 클라우드 기반 데이터 통합 서비스인 [Azure Data Factory](/azure/data-factory/?WT.mc_id=gridbank-docs-dastarr)를 사용하여 대규모 스토리지, 이동 및 처리 파이프라인을 구성하는 것일 수 있습니다. Data Factory 파이프라인을 통해 요청 시 데이터를 업로드할 수 있습니다. 서비스는 Azure에서 ETL(추출, 변환 및 로드?WT.mc_id=gridbank-docs-dastarr) 솔루션을 빌드하기 위한 시각적 디자이너를 Azure Portal에 제공합니다. Data Factory는 추가 처리를 위해 데이터를 Azure로 수집하는 데 도움이 될 수 있습니다.

## <a name="matching-processing-needs-with-demand"></a>처리 요구와 수요 일치

매일 또는 부하가 높은 월말에 위험을 계산할 때 계산에 상당한 계산 리소스가 사용됩니다. 이러한 계산이 연중무휴로 실행되지는 않습니다. 위험 계산이 온-프레미스 그리드에서 실행되지 않을 때는 조직이 워크로드 없이 비용이 많이 드는 귀중한 서버를 실행하여 전력, 냉각 및 데이터 센터 공간을 위한 지속적인 비용과 기타 고정 비용이 발생합니다.

### <a name="augmenting-on-premises-grid-with-azure-batch"></a>Azure Batch를 사용하여 온-프레미스 그리드 보강

비용을 최소화하기 위해 비즈니스는 수요가 낮을 때의 요구 사항을 충족할 수 있는 작업자 노드만 소유하고 관리하도록 선택할 수 있습니다. 그런 다음, 수요가 높은 위험 그리드 컴퓨팅 작업을 Azure의 고성능 서버로 푸시하여 워크로드 수요에 따라 탄력적으로 확장하고 축소할 수 있습니다.

Azure Batch 처리 모델은 위험 그리드 컴퓨팅과 관련해서 여러 가지 혜택이 있습니다.

- 다양한 온-프레미스 시스템에 대한 기존 투자를 보강합니다.
- 수요가 낮을 때 기존 인프라가 위험 분석을 수행하도록 허용하여 Azure 기반 작업자 노드의 할당을 해제합니다.
- 수요가 높을 때 위험 컴퓨팅 그리드에 추가 용량을 제공합니다.
- 부하로 인해 [HPC(고성능 컴퓨팅)](/azure/virtual-machines/windows/hpcpack-cluster-options?WT.mc_id=gridbank-docs-dastarr) 구성이 요청되는 경우에도 Batch 워크로드에 필요한 처리 능력과 머신 프로필을 일치시킬 수 있습니다.

일반적인 솔루션은 온-프레미스 작업자가 모두 사용 중일 때 Azure에 자동으로 작업자 노드를 추가하는 것입니다. 위험 그리드 헤드 노드는 단순히 더 많은 작업자를 요청합니다. 그러면 Azure에서 그리드 작업자 노드 수가 자동으로 확장되어 탄력적 수요 솔루션이 가능합니다.

![하이브리드 클라우드](./assets/risk-grid-compute-assets/03-hybrid-cloud.png)

효율적인 리소스 사용과 더불어, 이 방식은 다른 혜택도 제공합니다. 독립 작업의 경우 더 많은 작업자를 추가하면 부하의 선형 확장이 가능합니다. Azure는 매우 큰 VM 인스턴스나 여러 개의 GPU 카드가 있는 머신을 시도할 수 있는 유연성도 제공합니다. 이 유연성을 통해 실험과 혁신을 시도할 수 있습니다.

분기별 가치 평가와 같이 더 많은 컴퓨팅 용량이 필요한 경우 Azure Batch 자동 크기 조정을 통해 추가 용량을 얻을 수도 있습니다. 자동 크기 조정은 Batch 솔루션에 탄력성을 제공합니다. 필요한 부하와 일치하도록 리소스 크기를 조정함으로써 Azure는 하드웨어를 소유하는 것보다 저렴한 비용으로 훨씬 더 큰 용량을 제공합니다.

대부분의 상용 그리드 제품은 어떤 형태로든 클라우드로 확장 기능을 지원하므로 위험 분석 부하에 대한 개념 증명이 보다 용이합니다. 예를 들어 [Microsoft HPC Pack](/azure/virtual-machines/windows/hpcpack-cluster-options?WT.mc_id=gridbank-docs-dastarr)은 TIBCO, Univa 등의 회사 제품과 마찬가지로 Azure에서 실행될 수 있습니다. 이러한 타사 도구 또는 시스템은 대부분 [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/?WT.mc_id=gridbank-docs-dastarr)를 통해 제공됩니다.

### <a name="migrating-additional-resources-to-the-cloud"></a>클라우드로 추가 리소스 마이그레이션

워크로드가 증가하거나 온-프레미스 데이터 센터 인프라가 노후화됨에 따라 조직은 위험 그리드 컴퓨팅을 위한 전체 Batch 처리를 Azure로 이동할 수 있습니다.

### <a name="growing-into-azure"></a>Azure로 확장

온-프레미스 머신의 수명이 끝나면 클라우드에 작업자 노드를 더 배포할 수 있습니다. Batch 헤드 노드의 경우도 마찬가지입니다. 이 경우 온-프레미스 네트워크와 Azure 간의 관계가 반전되며, Azure ExpressRoute와 같은 네트워크 간 제품과 나머지 온-프레미스 작업자 노드를 해제하여 비용을 줄일 수 있습니다.

이 변경의 일부로 다양한 파일 수신 기술을 사용하여 Azure에 데이터를 제공할 수 있습니다. Azure는 컴퓨팅 작업이 온-프레미스 네트워크에서 픽업하도록 하는 대신 데이터를 직접 업로드할 수 있는 REST 엔드포인트를 포함하여, 선택할 수 있는 다양한 스토리지 옵션을 제공합니다.

 ![온-프레미스 일괄 처리](./assets/risk-grid-compute-assets/04-batch-process.png)

이 모델에서는 모든 위험 그리드 컴퓨팅 작업이 클라우드에서 수행될 수 있습니다. 작업자가 처리한 데이터 파일을 Azure Storage에 저장할 수 있고, Azure Data Lake에 데이터를 직접 피드할 수 있으며, Azure HDInsight에서 기계 학습 요구를 처리할 수 있습니다. 마지막으로, Power BI와 Azure Analytics는 뛰어난 데이터 분석 도구이며 Azure에 저장된 모든 데이터에 대해 작업할 수 있습니다.

### <a name="data-security-considerations-for-risk-grid-computing"></a>위험 그리드 컴퓨팅에 대한 데이터 보안 고려 사항

계산 데이터에는 PII(개인 식별 정보?WT.mc_id=gridbank-docs-dastarr)가 없는 경우가 많지만, 대부분의 은행은 여전히 워크로드를 클라우드에 배치하기 전에 보안 위험 평가를 수행할 가능성이 높습니다. 이 평가에는 Microsoft의 입력이 필요할 수 있으며, 보안 권장 사항이 발생할 수 있습니다.

위험 그리드 컴퓨팅과 관련해서 주목할 만한 고려 사항은 [Azure VNet 내에서 일괄 처리 프로세스를 실행](/azure/batch/batch-virtual-network?WT.mc_id=gridbank-docs-dastarr)하는 것입니다. 이렇게 하면 풀 컴퓨팅 노드가 다른 컴퓨팅 노드 또는 온-프레미스 네트워크와 안전하게 통신할 수 있습니다. 일괄 처리 컴퓨팅 노드에서 적절한 서비스 계정 및 NSG(네트워크 보안 그룹)를 만들고 사용해야 합니다. [Azure에는 전송 중인 데이터와 Azure Storage에 저장된 데이터의 암호화를 위한 솔루션도 있습니다](/azure/security/blueprints/financial-services-regulated-workloads?WT.mc_id=gridbank-docs-dastarr).

고려해야 하는 일부 영역은 AD(Active Directory) 또는 AD에 연결되지 않은 컴퓨팅 노드(Windows Server nodes?WT.mc_id=gridbank-docs-dastarr), [VM 디스크 암호화](/azure/security/azure-security-disk-encryption?WT.mc_id=gridbank-docs-dastarr), 저장되었거나 전송 중인 컴퓨팅 입력 및 미사용 데이터의 출력 보안, Azure 네트워크 구성, 사용 권한 등입니다. 인증은 비밀 키를 통해 REST API 수준에서 처리할 수도 있습니다.

## <a name="getting-started"></a>시작하기

사내 위험 컴퓨팅 그리드를 이미 사용하고 있는 고객이 많습니다. 회사 내부에서 그리드를 개발한 경우 Azure Batch를 통해 그리드를 확장하는 것이 좋습니다. Azure Batch를 시작하기에 좋은 지점은 현재 처리 중인 애플리케이션 로직을 복제하고 Azure에서 Batch 작업으로 실행하여 현재 온-프레미스 솔루션을 확장하는 것입니다. 이 경우 애플리케이션의 기능에 따라 Azure Batch 계산 노드를 온-프레미스 네트워크에 연결하기 위한 네트워킹 솔루션이 필요할 수 있습니다.

보안, 속도 및 연결 안정성에 대한 염려를 완화하려면 Azure ExpressRoute 또는 VPN Gateway를 사용하여 온-프레미스 네트워크를 Azure에 연결하는 것이 좋습니다. 여기서 온-프레미스 헤드 노드가 Azure 기반 작업자 노드 클러스터를 프로비전하여 필요에 따라 확장 및 축소되도록 할 수 있습니다.

마지막으로, 위험 컴퓨팅 인프라 전체를 Azure로 마이그레이션할 준비가 되었을 수 있습니다. 이 경우 [여기에 제공된 문서](/azure/batch/?WT.mc_id=gridbank-docs-dastarr)를 참조하여 지금 시작하세요.

## <a name="technologies-presented"></a>제공되는 기술

[Azure Batch](/azure/batch/?WT.mc_id=gridbank-docs-dastarr)를 사용하면 온-프레미스 위험 컴퓨팅 작업자 노드를 보강하여 수요에 따라 계산 리소스를 동적으로 제공할 수 있습니다.

[Azure DataLake](/azure/data-lake-store?WT.mc_id=gridbank-docs-dastarr)는 위험 분석 데이터에 대한 스토리지, 처리 및 분석을 제공합니다.

[Azure ExpressRoute](/azure/expressroute/expressroute-introduction?WT.mc_id=gridbank-docs-dastarr)는 연결 공급자가 지원하는 개인 연결을 통해 온-프레미스 네트워크를 Azure로 확장합니다.

[Azure HDInsight](/azure/hdinsight/?WT.mc_id=gridbank-docs-dastarr)는 월말 일괄 처리 실행에 제공되는 데이터와 같은 대량 데이터를 처리하기 위한 완전 관리형 오픈 소스 분석 서비스입니다.

[Microsoft HPC Pack](/azure/virtual-machines/windows/hpcpack-cluster-options?WT.mc_id=gridbank-docs-dastarr)을 사용하면 일괄 처리를 위해 고성능 컴퓨팅 클러스터를 프로비전할 수 있습니다.

[Power BI](/power-bi/?WT.mc_id=gridbank-docs-dastarr)는 위험 분석가가 인사이트를 얻고 공유하기 위해 사용하는 비즈니스 분석 도구 모음입니다.

[VPN Gateway](/azure/vpn-gateway/?WT.mc_id=gridbank-docs-dastarr)는 인터넷을 통해 온-프레미스 네트워크를 Azure 클라우드로 확장합니다.

## <a name="conclusion"></a>결론

이 문서에서 다루는 솔루션은 금융의 위험 그리드 컴퓨팅에 대한 접근 방식입니다. Azure 제품 및 서비스의 풍부한 기능과 다양한 기존 클라이언트 시스템 아키텍처를 감안할 때 다른 아키텍처를 사용할 수도 있습니다. 이 경우에도 Batch는 이 문서에서 설명한 장점이 있는, 합리적인 위험 그리드 컴퓨팅 모델을 제공합니다.

[온-프레미스 네트워크를 Azure로 확장](/azure/architecture/reference-architectures/hybrid-networking/?WT.mc_id=gridbank-docs-dastarr)하면 Azure가 네트워크 리소스 및 온-프레미스 네트워크에 이미 있는 기타 처리 시스템에 쉽게 액세스할 수 있습니다. 온-프레미스 머신의 수명이 끝나면 하이브리드 모델을 지원하는 대신 완전히 Azure에서 Batch 컴퓨팅을 사용하는 것이 더 타당할 수 있습니다.

Batch 작업이 시작되기 전에 파일을 Azure Storage로 업로드하는 것도 하이브리드 네트워크 없이 Batch를 활용하는 또 다른 방법입니다. 이 작업은 증분 방식으로 수행하거나 Batch 실행에 대한 시작 프로세스로 수행할 수 있습니다.

연결 전략을 선택한 후, 위험 컴퓨팅을 시작할 논리적 지점은 Azure 컴퓨팅 작업자 노드에 기존 작업을 배치하고 테스트 환경에서 실행하여 코드를 변경해야 하는지 확인하는 것입니다. [이 문서에서는 선택한 언어 또는 도구에서 Azure Batch를 시작하기 위한 시작점을 제공](/azure/batch/batch-virtual-network?WT.mc_id=gridbank-docs-dastarr)합니다.

[금융의 위험 그리드 컴퓨팅 솔루션 가이드](/azure/industry/financial/risk-grid-banking-solution-guide?WT.mc_id=banking-docs-dastarr)
