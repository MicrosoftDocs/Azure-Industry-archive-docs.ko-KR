---
title: 금융의 데이터 관리 개요
author: dstarr
ms.author: dastarr
ms.date: 10/30/2019
ms.topic: article
ms.service: industry
description: Microsoft Azure를 사용하여 규제가 엄격한 금융 환경에서 데이터를 관리하는 기술을 설명합니다.
ms.openlocfilehash: 1314054018c04e45b6450604febbf0142ead380d
ms.sourcegitcommit: f42a60539bec2a7769b42b6574f09eed4d1b6c79
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/07/2019
ms.locfileid: "73750539"
---
# <a name="data-management-in-banking-overview"></a>금융의 데이터 관리 개요

## <a name="introduction"></a>소개

오늘날의 은행은 방화벽 안에 고객과 변화하는 금융 상황에 대한 엄청난 양의 귀중한 정보를 보호하고 저장할 책임을 지고 있습니다. 데이터를 사용할 경우 여러 금융 활동에서 의사 결정을 개선할 수 있지만, 쉽게 액세스하거나 검색할 수 없어 정보가 사용되지 않는 경우가 많습니다.

이 데이터를 사용함으로써 은행은 대출 채무 불이행의 위험이 있는 사람에 대한 정보를 더 빠르게 찾거나, 포트폴리오 가치 평가를 조정해야 하는 시장을 결정할 수 있습니다. 또한 은행은 규제 요구 사항을 충족하기 위해 데이터가 저장 및 관리되는 방식을 보다 명확하게 파악할 수 있으므로, 규정에 맞게 데이터를 활용, 유지, 보관 또는 삭제할 수 있습니다.

일상적인 금융 기능 요구 사항을 충족하려면 크고 작은 수천 가지의 결정이 필요하기 때문에 데이터가 점점 중요해지고 있습니다. 이뿐만 아니라 엄격한 규제 요구 사항 및 금융 범죄 의무를 감안할 때 은행은 초기 정보 랜딩 및 데이터 리포지토리에 이르기까지 모든 데이터 분석 프로세스의 결과를 감사할 수 있어야 합니다. 추적 기능을 사용하려면 수집부터 작업 가능한 데이터 생성에 이르기까지 투명성이 필요합니다.

은행이 처리하는 많은 계정이나 비즈니스를 관리하려면 이러한 모든 데이터를 신속하고 비용 효율적으로 파악해야 합니다. 은행의 디지털화가 성숙됨에 따라 데이터 양과 해당 데이터를 새로운 방식으로 활용할 수 있는 기회가 기하급수적으로 증가하여 은행이 새로운 비즈니스 모델 및 고객 중심 기회의 영역을 추구할 수 있게 되었습니다.

적절한 데이터 스토리지 전략 수립은 운영 효율성, 뛰어난 애플리케이션 성능 및 규정 준수의 관건이 됩니다. 또한 데이터 스토리지 전략은 비즈니스 인텔리전스 및 작업 가능한 인사이트에 사용할 수 있는 형식으로 데이터를 변환하기 위한 초기 핵심입니다.

데이터 관리의 일반적인 패턴은 다음과 같습니다.

![데이터 관리 흐름](./assets/data-mgmt-in-banking-overview/data-mgmt-00.png)

이 모델에서 “데이터 서비스”는 모든 변환, 데이터 조인 또는 보관 이외의 기타 모든 데이터 작업을 설명합니다. 이는 데이터를 활용하여 보다 현명한 결정을 내리는 데 필요한 주요 작업입니다.

모든 은행과 금융 기관은 데이터를 수집, 이동 및 저장합니다. 이 문서에서는 데이터를 Azure로 가져오고, 기존의 온-프레미스 데이터 스토리지, 처리, 보관 및 삭제에서 벗어나는 과정을 중점적으로 다룹니다. 데이터를 Azure로 이동하면 은행과 금융 기관이 다음과 같은 근본적인 혜택을 활용할 수 있습니다.

- 필요한 시간과 장소에서만 컴퓨팅 리소스 및 데이터 용량을 사용하는, 효율적이고 제한 없는 글로벌 규모의 비용 제어.

- 온-프레미스에서 물리적 서버 사용 중단을 통해 자본 지출 및 관리 비용 절감.

- 통합 백업 및 재해 복구를 통해 데이터 보호의 비용과 복잡성 감소.

- 준수 요구를 충족하는 동시에 콜드 데이터를 저비용 스토리지에 자동 보관.

- 학습, 예측, 변환 또는 기타 요구에 맞게 데이터를 처리하기 위한 고급 및 통합 데이터 서비스 액세스.

이 문서에서는 Azure에 데이터가 효율적으로 수신되도록 하는 권장 기술 및 데이터가 클라우드에 배치된 후 사용할 기본 데이터 관리 기술을 제공합니다.

## <a name="data-ingest"></a>데이터 수집

금융 기관에는 이미 수집되어 현재 애플리케이션에서 사용 중인 데이터가 있습니다. 이 데이터를 Azure로 이동하는 몇 가지 옵션이 있습니다. 대부분의 경우 기존 애플리케이션에 대한 변경을 최소화하여 기존 애플리케이션이 온-프레미스에 있는 것처럼 Azure의 데이터에 연결할 수 있습니다. 이 내용은 특히 Microsoft [Azure SQL Database](/azure/sql-database/?WT.mc_id=bankdm-docs-dastarr)를 사용하는 경우에 해당되지만, [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/databases?WT.mc_id=bankdm-docs-dastarr)를 통해 Oracle, TeraData MongoDB 등의 솔루션을 찾을 수 있습니다.

온-프레미스에서 Azure로 데이터를 이동하기 위한 다양한 데이터 마이그레이션 전략이 있으며, 각기 대기 시간이 다릅니다. 아래에 참조된 모든 기술은 데이터 투명성 및 안정적인 보안을 제공합니다.

### <a name="virtual-network-vnet-service-endpoints"></a>VNet(Virtual Network) 서비스 엔드포인트

고객의 금융 정보를 처리하는 경우 보안이 중요합니다.
Azure 내의 리소스(예: 데이터베이스) 보안은 Azure 자체의 네트워크 인프라 설정 후 특정 엔드포인트를 통한 해당 네트워크 액세스에 따라 결정되는 경우가 많습니다.

데이터를 Azure로 전송하기 전에 Azure 리소스 및 온-프레미스와 해당 리소스 간의 연결을 모두 보호하는 네트워크 토폴로지를 고려하는 것이 좋습니다.
[VNet(Virtual Network?WT.mc_id=bankdm-docs-dastarr) 서비스 엔드포인트](/azure/virtual-network/virtual-network-service-endpoints-overview?WT.mc_id=bankdm-docs-dastarr)는 Azure에서 정의된 VNet에 대한 보안 직접 연결을 제공합니다.

VNet는 제한된 VNet 안에 Azure 리소스를 포함하기 위해 Azure에서 정의됩니다. 그런 다음, 해당 VNet에 대한 엔드포인트를 사용하여 중요한 Azure 서비스 리소스에 대한 액세스를 보호하고 정의된 VNet의 리소스로만 액세스를 제한할 수 있습니다.

## <a name="database-lift-and-shift"></a>데이터베이스 리프트 앤 시프트

데이터베이스 마이그레이션의 “리프트 앤 시프트” 모델은 Azure SQL Database를 사용하는 가장 일반적인 시나리오 중 하나입니다. 리프트 앤 시프트는 단순히 기존 온-프레미스 데이터베이스를 클라우드로 직접 이동하는 것을 의미합니다. 이 작업을 수행하는 이유는 다음과 같습니다.

- 가격이 더 높거나 다른 운영 이유로 현재 데이터 센터에서 이동
- 현재 온-프레미스 SQL Server 데이터베이스 하드웨어가 만료되거나 수명이 거의 끝남
- 회사에 대해 일반적인 “클라우드로 이동” 전략을 지원하려면 다음을 수행합니다.
- SQL Azure의 가용성 및 재해 복구 기능 활용

더 작은 데이터베이스의 경우 데이터 수집의 첫 번째 단계는 일반적으로 Azure Portal, Azure CLI 또는 Azure SDK를 통해 필요한 데이터 저장소 및 구조(테이블)를 만드는 것입니다. 이러한 작은 데이터 저장소의 경우 올바른 데이터를 적절한 Azure 데이터 스토리지에 복사하도록 작성된 사용자 지정 애플리케이션에서 다음 단계를 수행할 수 있습니다. 더 큰 데이터 마이그레이션의 경우 Azure에서 백업을 복원하는 것이 일반적으로 가장 빠른 경로입니다.

데이터를 안전하고 빠르게 Azure에 전송하는 방법에는 여러 가지가 있습니다. 일부 표준 기술과 각 기술의 장점 및 단점에 대해서는 [이 문서를 참조](/azure/architecture/data-guide/scenarios/data-transfer?WT.mc_id=bankdm-docs-dastarr)하세요.

### <a name="azure-database-migration-service"></a>Azure Database Migration Service

SQL Server 데이터베이스를 리프트 앤 시프트하는 경우 [Microsoft Azure Database Migration Service](/azure/dms/dms-overview?WT.mc_id=bankdm-docs-dastarr)를 사용하여 데이터베이스를 Azure로 이동할 수 있습니다. 서비스는 [Data Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?WT.mc_id=bankdm-docs-dastarr)를 사용하여 온-프레미스 데이터베이스가 Azure SQL에 제공된 기능과 호환되는지 확인합니다. 데이터베이스를 마이그레이션하기 전에 필요한 변경 작업은 사용자 책임입니다. 또한 서비스를 사용하려면 온-프레미스 네트워크와 Azure 간에 사이트 간 인터넷 연결이 필요합니다.

### <a name="bulk-copy-program-for-sql-server"></a>SQL Server에 대한 대량 복사 프로그램

현재 SQL Server가 온-프레미스에 있고 SQL Azure로 이동하려는 경우 또 다른 뛰어난 기술은 SQL Server Management Studio와 [BCP 유틸리티를 사용하여 데이터를 SQL Azure로 이동](https://azure.microsoft.com/blog/bcp-and-sql-azure/?WT.mc_id=bankdm-docs-dastarr)하는 것입니다. 원본 온-프레미스 서버에서 Azure SQL 데이터베이스를 스크립팅하고 만든 후 BCP를 사용하여 데이터를 SQL Azure로 신속하게 전송할 수 있습니다.

### <a name="blob-and-file-storage"></a>Blob 및 File Storage

개별 은행 지점의 로컬 온-프레미스 서버에 자체 파일 저장소가 있는 경우가 많습니다. 이 경우 지점 간의 파일 공유에서 문제가 발생하여 지정된 파일에 대한 단일 데이터 소스(single source of truth)가 없을 수 있습니다. 더욱이, 기관의 “공식” 파일 저장소에 지점이 액세스하지만 연결이 간헐적이거나 파일 공유 액세스에 다른 문제가 있을 수 있습니다.

Azure에는 이러한 문제를 완화하는 데 도움이 되는 서비스가 있습니다. 이 데이터를 Azure로 이동하면 모든 데이터에 대한 단일 데이터 소스(single source of truth)와 사용 권한 및 액세스가 중앙에서 제어되는, 어디서나 액세스 가능한 스토리지가 제공됩니다.

특정 데이터 형식에는 다른 데이터 스토리지 솔루션이 더 적합할 수 있습니다.
예를 들어 SQL Server에 온-프레미스로 저장된 데이터가 Azure SQL에 가장 적합할 가능성이 큽니다. Blob service에 구현된 [Azure Blob](/azure/storage/blobs/storage-blobs-introduction?WT.mc_id=bankdm-docs-dastarr) 스토리지 또는 [Azure Files](/azure/storage/files/storage-files-introduction?WT.mc_id=bankdm-docs-dastarr) 스토리지에는 .csv 또는 Excel 파일에 저장된 데이터가 가장 적합할 가능성이 큽니다.

Azure에 들어오고 나가는 거의 모든 데이터는 데이터 이동의 일부로 Blob Storage를 통과합니다. Blob Storage에는 다음과 같은 영역이 있습니다.

- 지속형 및 사용 가능
- 보안 및 준수
- 관리 가능 및 비용 효율적
- 확장성 및 고성능
- 개방형 및 상호 운용 가능

모든 지점을 Azure의 동일한 파일 공유에 연결하는 작업은 그림 1과 같이 은행의 기존 데이터 센터를 통해 수행되는 경우가 많습니다. 회사 데이터 센터는 SMB(Server Message Block) 연결을 통해 Files 스토리지에 연결합니다.
논리적으로 사이트 네트워크의 관점에서 파일 공유는 회사 데이터 센터에 있을 수도 있고, 네트워크로 연결된 다른 파일 공유로 탑재될 수도 있습니다.
이 기술을 사용하는 경우 데이터가 저장 시 및 데이터 센터와 Azure 간에 전송되는 동안 암호화됩니다.

![논리 파일 공유](./assets/data-mgmt-in-banking-overview/data-mgmt-01.png)

그림 1

엔터프라이즈는 Files 스토리지를 사용하여 대용량 파일을 통합하고 보호하는 경우가 많습니다. 이렇게 하면 오래된 파일 서버의 사용을 중단하거나 하드웨어의 용도를 변경할 수 있습니다.
Files 스토리지로 이동할 경우의 또 다른 장점은 데이터 관리 및 복구 서비스를 중앙 집중화할 수 있다는 것입니다.

### <a name="azure-data-box"></a>Azure Data Box

대체로 은행은 페타바이트는 아니라 해도 테라바이트 단위의 정보를 Azure로 가져와야 합니다. 다행히, Azure의 데이터 저장소는 매우 탄력적이며 확장성이 큽니다.

대량의 데이터를 Azure로 마이그레이션하는 데 중점을 두는 서비스는 [Azure Data Box](https://azure.microsoft.com/services/storage/databox/?WT.mc_id=bankdm-docs-dastarr)입니다. 이 서비스는 Azure 연결을 통해 데이터 또는 백업을 전송하지 않고 데이터를 마이그레이션하도록 설계되었습니다. 테라바이트 단위의 데이터에 적합한 Azure Data Box는 Azure Portal에서 주문할 수 있는 어플라이언스입니다. 사용자 위치로 배송되며, 여기서 사용자 네트워크에 연결하고 표준 NAS 프로토콜을 통해 데이터를 로드하고 표준 256-AES 암호화를 통해 보호할 수 있습니다. 데이터가 어플라이언스에 배치되고 나면 Azure 데이터 센터로 다시 배송되며, 여기서 데이터가 Azure에 하이드레이션됩니다.
그런 다음 디바이스는 안전하게 지워집니다.

## <a name="azure-information-protection"></a>Azure Information Protection

AIP(Azure Information Protection)는 조직이 문서와 메일을 분류하고, 레이블을 지정하고, 보호할 수 있게 해주는 클라우드 기반 솔루션입니다. 이러한 작업은 규칙 및 조건을 정의하는 관리자에 의해 자동으로, 사용자에 의해 또는 사용자에게 권장 사항이 제공되는 조합에 의해 수동으로 수행될 수 있습니다.

## <a name="data-services"></a>Data services

은행은 마스터 데이터 관리, 별개의 핵심 금융 시스템으로 인한 메타데이터 충돌 및 발생 시스템, 온보딩 시스템, 제안 관리 시스템, CRM 시스템 등에서 들어오는 데이터로 곤란을 겪고 있습니다. Azure에는 이러한 문제뿐 아니라 일반적으로 발생하는 기타 데이터 문제의 완화에 도움이 되는 도구가 있습니다.

금융 서비스 조직이 해당 데이터에 대해 수행해야 하는 여러 가지 작업이 있습니다. Azure 데이터 저장소에 데이터를 쓸 때 해당 데이터를 변환하거나 수집되는 내용을 보강하는 다른 데이터와 조인해야 경우도 있습니다.

### <a name="azure-data-factory"></a>Azure 데이터 팩터리

[Microsoft Azure Data Factory](/azure/data-factory/introduction?WT.mc_id=bankdm-docs-dastarr)는 Data Factory 파이프라인에서 데이터 이동을 수신, 처리 및 모니터링하는 데 도움이 되는 완전 관리형 서비스입니다. Data Factory 작업은 데이터 관리 파이프라인의 구조를 형성합니다.

Data Factory를 사용하면 Azure로 들어오거나 다른 Azure 서비스 간에 전달되는 데이터를 변환하거나 보강할 수 있습니다. Data Factory는 복잡한 하이브리드 ETL(추출-변환-로드), ELT(추출-로드-변환) 및 데이터 통합 프로젝트를 위해 빌드된 관리되는 클라우드 서비스입니다.

예를 들어 데이터를 분석 파이프라인이나 도구에 피드하여 작업 가능한 인사이트를 생성할 수 있습니다. 데이터가 기계 학습 솔루션으로 전달하거나 이후의 다운스트림 처리를 위해 다른 형식으로 변환할 수도 있습니다. 예를 들어 .csv 파일을 기계 학습 시스템에 더 적합한 parquet 파일로 변환하고 해당 parquet 파일을 Blob Storage에 저장할 수 있습니다.

[Azure HDInsight](/azure/hdinsight/hadoop/apache-hadoop-introduction?WT.mc_id=bankdm-docs-dastarr), [Spark](/azure/hdinsight/spark/apache-spark-overview?WT.mc_id=bankdm-docs-dastarr), [Azure Data Lake Analytics](/azure/data-lake-analytics/data-lake-analytics-overview?WT.mc_id=bankdm-docs-dastarr) 및 [Azure Machine Learning](/azure/machine-learning/?WT.mc_id=bankdm-docs-dastarr)과 같은 다운스트림 컴퓨팅 서비스에 데이터를 전송할 수도 있습니다. 이렇게 하면 시스템에 직접 피드하여 분석 및 지능형 보고를 수행할 수 있습니다. 데이터 수신의 일반적인 모델 중 하나가 아래 그림 2에 나와 있습니다. 데이터는 다운스트림 분석 서비스에서 사용하도록 일반적인 [Data Lake](/azure/data-lake-store/data-lake-store-overview?WT.mc_id=bankdm-docs-dastarr)에 보관됩니다.

![Data Lake 수집 모델](./assets/data-mgmt-in-banking-overview/data-mgmt-02.png)

그림 2

Data Factory 파이프라인은 데이터 세트를 가져와서 출력하는 작업으로 구성됩니다. 데이터를 가져오려는 위치, 처리하는 방법 및 결과를 저장할 위치를 정의하는 파이프라인으로 작업을 어셈블할 수 있습니다. 작업으로 파이프라인을 빌드하는 것이 Data Factory의 핵심이며, Azure Portal에서 바로 시각적 워크플로를 작성하면 파이프라인을 쉽게 만들 수 있습니다. 작업의 [전체 목록은 여기를 참조](/azure/data-factory/concepts-pipelines-activities?WT.mc_id=bankdm-docs-dastarr)하세요.

### <a name="azure-databricks"></a>Azure Databricks

[Azure Databricks](/azure/azure-databricks/?WT.mc_id=bankdm-docs-dastarr)는 Azure의 관리되는 Apache Spark 기반 분석 플랫폼입니다. 확장성이 크며, 필요한 만큼 큰 머신 클러스터에서 Spark 작업이 실행됩니다. Databricks는 데이터 과학자, 데이터 엔지니어 및 비즈니스 분석가 간의 단일 협업 공간을 제공하는 Notebook에서 작동합니다.

Databricks는 데이터 변환 또는 분석이 필요한 경우 논리적 처리 파이프라인입니다. 인사이트 생성 시간이 중요한 기계 학습 시나리오나 간단한 파일 변환을 위해 Data Factory에서 직접 피드할 수 있습니다.

![Databricks](./assets/data-mgmt-in-banking-overview/data-mgmt-03.png)

## <a name="archiving-data"></a>데이터 보관

활성 데이터 저장소에서 데이터가 더 이상 필요하지 않은 경우 주 정부 및 지역 금융 규정에 따라 준수 또는 감사 추적을 위해 보관할 수 있습니다. Azure에는 자주 액세스하지 않는 데이터의 스토리지에 사용할 수 있는 옵션이 있습니다. 몇 년간 스토리지에 보관해야 하는 데이터의 경우 개인 정보 보호 문제가 발생하는 경우가 많습니다.

특히 온-프레미스 데이터베이스에 저장하는 경우 데이터 저장 비용이 막대할 수 있습니다. 대체로 이러한 데이터베이스는 자주 액세스하지 않으며 새로 보관되는 데이터를 쓰거나 더 이상 보관하지 않으려는 데이터를 데이터베이스에서 제거하는 경우에만 액세스합니다.
온-프레미스 머신에 자주 액세스하지 않는 것은 하드웨어의 총 소유 비용 증가를 의미합니다.

### <a name="azure-archive-storage"></a>Azure Archive Storage

파일이나 이미지와 같은 구조화되지 않은 데이터를 위해 Azure는 핫, 쿨 및 보관을 비롯한 [여러 계층의 스토리지](/azure/storage/blobs/storage-blob-storage-tiers?WT.mc_id=bankdm-docs-dastarr)를 Blob Storage에 제공합니다. 핫 액세스 계층은 활성 상태이며, 애플리케이션에서 가장 활발하게 사용될 것으로 예상되는 데이터를 위한 계층입니다. 쿨 액세스 계층은 단기 백업 및 재해 복구 데이터 세트와 애플리케이션에서 사용할 수 있지만 거의 액세스하지 않는 데이터를 위한 계층입니다. 보관 계층은 비용이 가장 낮으며 오프라인 상태의 데이터를 위한 계층입니다.

보관 계층 데이터는 쿨 또는 핫 계층으로 리하이드레이션될 수 있지만, 이 작업을 완료하는 데 몇 시간이 걸릴 수도 있습니다. 최소 180일간 데이터가 액세스되지 않는 경우 보관 스토리지가 적합할 수 있습니다. Blob이 보관 스토리지에 있는 경우 읽을 수는 없지만 나열, 삭제, 메타데이터 검색과 같은 기존의 다른 작업을 수행할 수 있습니다. 보관 데이터 계층은 Blob Storage에 대한 최저 비용 데이터 계층입니다.

### <a name="azure-sql-database-long-term-retention"></a>Azure SQL Database 장기 보존

Azure SQL을 사용하는 경우 최대 10년까지 백업을 저장하기 위한 [장기 백업 보존 서비스](/azure/sql-database/sql-database-long-term-retention?WT.mc_id=bankdm-docs-dastarr)가 있습니다. 사용자는 백업이 주, 월 또는 년 단위로 보존되도록 장기 스토리지 보존을 예약할 수 있습니다.

장기 스토리지에서 데이터베이스를 복원하려면 해당 타임스탬프에 따라 특정 백업을 선택합니다. 원본 데이터베이스와 동일한 구독 아래의 기존 서버로 데이터베이스를 복원할 수 있습니다.

## <a name="deleting-unwanted-data"></a>원치 않는 데이터 삭제

데이터 보존과 관련된 금융 규정 또는 정책 준수를 유지하려면 더 이상 원치 않는 데이터를 자주 삭제해야 합니다. 이 원치 않는 데이터에 대한 기술 솔루션을 구현하기 전에 합의된 정책이 위반되지 않도록 제거 계획을 수립하는 것이 중요합니다. 언제든지 보관 또는 Azure의 기타 데이터 저장소에서 데이터를 삭제할 수 있습니다.

원치 않는 데이터를 삭제하는 효과적인 전략은 일정한 간격(야간 또는 매주가 가장 일반적임)으로 삭제하는 것입니다. 이 작업을 수행하기 위해 [타이머 트리거 Azure 함수](/azure/azure-functions/functions-bindings-timer?WT.mc_id=bankdm-docs-dastarr)를 작성할 수도 있습니다. 데이터를 삭제하면 Microsoft Azure는 캐시된 복사본 또는 백업 복사본을 모두 포함하여 데이터를 삭제합니다.

## <a name="getting-started"></a>시작하기

시작하는 방법에는 지금 사용하는 데이터 모델의 현재 사용량 및 성숙도에 따라 여러 가지가 있습니다. 어떤 경우든, 데이터 저장소별로 필요한 데이터 스토리지, 처리 및 보존 모델을 지금 검토하는 것이 좋습니다. 이 작업은 규정 준수 시나리오에서 데이터 관리 시스템을 빌드하는 데 중요합니다.
클라우드는 현재 온-프레미스에서 사용할 수 없는 새로운 기회를 여기서 제공합니다. 이는 사용자가 보유한 기존 데이터 모델에 대한 업데이트를 의미할 수도 있습니다.

새로운 데이터 모델에 익숙해지면 데이터 수집 전략을 결정합니다. 어떤 데이터 원본이 있나요? Azure에서 데이터는 어디에 배치되나요? 어떻게, 그리고 언제 Azure로 이동되나요? 콘텐츠 형식, 크기 등에 따라 마이그레이션에 도움이 되는 많은 리소스를 여기서 사용할 수 있습니다. Azure 데이터 마이그레이션 서비스는 이러한 예 중 하나입니다.

데이터가 Azure에 호스트되고 나면, 더 이상 유용하지 않거나 수명이 끝난 데이터에 대한 데이터 제거 계획을 만듭니다. 장기(콜드) 스토리지는 항상 효율적인 보관 옵션이지만, 만료된 데이터를 정리하면 공간과 전반적인 스토리지 비용이 절감됩니다. 백업 및 보관 [Azure 솔루션 아키텍처](https://azure.microsoft.com/solutions/architecture/?solution=backup-archive?WT.mc_id=bankdm-docs-dastarr)는 전반적인 전략을 계획하는 데 도움이 되는 좋은 리소스입니다.

## <a name="relevant-technologies"></a>관련 기술

- [Azure Functions](/azure/azure-functions/functions-bindings-timer?WT.mc_id=bankdm-docs-dastarr)는 시스템 이벤트에 대한 응답이나 타이머로 실행될 수 있는 서버리스 스크립트 및 작은 프로그램입니다.

- [Azure Storage 클라이언트 도구](/azure/storage/common/storage-explorers?WT.mc_id=bankdm-docs-dastarr)는 데이터 저장소에 액세스하기 위한 도구이며 Azure Portal보다 훨씬 더 많은 도구를 포함합니다.

- [Blob Storage](/azure/storage/blobs/storage-blobs-introduction?WT.mc_id=bankdm-docs-dastarr)는 텍스트 또는 이미지와 같은 파일 및 기타 구조화되지 않은 데이터 형식을 저장하는 데 적합합니다.

- [Databricks](/azure/azure-databricks/?WT.mc_id=bankdm-docs-dastarr)는 편리한 Spark 클러스터 구현을 제공하는 완전 관리형 서비스입니다.

- [Data Factory](/azure/data-factory/concepts-pipelines-activities?WT.mc_id=bankdm-docs-dastarr)는 데이터 스토리지, 전송 및 처리 서비스를 자동화된 데이터 파이프라인으로 구성하는 데 사용되는 클라우드 데이터 통합 서비스입니다.

## <a name="conclusion"></a>결론

금융 및 금융 산업의 디지털 환경이 빠르게 변화하면서 느린 진입 작동 시간 없이 즉시 활용할 수 있는 솔루션과 파트너를 원하는 고객이 점점 증가하고 있습니다. 데이터 수집이 기하급수적으로 증가함에 따라 은행은 중요한 데이터를 저장, 분석 및 사용하는 빠르고 혁신적이며 안전한 방법을 요구하고 있습니다.

Azure는 여러 기술과 전략을 사용하여 데이터 수집, 처리, 보관 및 삭제 요구 사항을 지원할 수 있습니다. 간단하게 데이터를 Azure에 수집할 수 있으며 데이터 형식, 구조 등에 따라 다양한 데이터 저장소를 사용하여 데이터를 저장할 수 있습니다. SQL Server 및 SQL Azure 이외의 데이터 솔루션을 사용하여 타사 데이터베이스를 포함할 수 있습니다.

Databricks 및 Data Factory와 같은 Azure 서비스를 사용하여 간단하게 해당 데이터를 조작하고 데이터에 따라 조치를 취할 수 있습니다. 거의 액세스하지 않는 데이터의 장기 스토리지를 위해 보관 스토리지를 사용할 수 있으며, 필요에 따라 롤링 주기로 삭제할 수 있습니다.

데이터 관리 계획의 설계를 시작하려면 [백업 및 보관 스토리지](https://azure.microsoft.com/solutions/architecture/?solution=backup-archive?WT.mc_id=bankdm-docs-dastarr)용 Azure 솔루션 라이브러리를 방문하세요.

**문서 작성자**: Howard Bush 및 David Starr
