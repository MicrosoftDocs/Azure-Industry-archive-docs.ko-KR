---
title: Azure에서 권장 시스템 및 R의 알고리즘 다시 사용
author: kbaroni
ms.author: kbaroni
ms.date: 11/20/2019
ms.topic: article
ms.service: industry
description: R 언어로 작성된 권장 앱을 다시 사용하고 최적화하는 방법입니다. Azure VM에서 Machine Learning Server를 사용합니다.
ms.openlocfilehash: c5c35de681abc52641952f8bc9e95095b9d99d97
ms.sourcegitcommit: b8f9ccc4e4453d6912b05cdd6cf04276e13d7244
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74263496"
---
# <a name="optimize-and-reuse-an-existing-recommendation-system"></a>기존 권장 시스템 최적화 및 재사용  

이 문서에서는 R로 작성된 기존 권장 시스템을 성공적으로 다시 사용하고 향상시키는 프로세스를 설명합니다. 이에 대한 핵심은 Microsoft Machine Learning Server에 기본적으로 제공된 MicrosoftML 및 RevoScaleR 라이브러리의 병렬 처리입니다.

## <a name="recommendation-systems-and-r"></a>권장 시스템 및 R

소매업체의 경우 소비자의 선호도와 구매 이력을 이해하는 것은 경쟁적 우위 요소입니다. 소매업체는 수년간 기계 학습과 함께 이러한 데이터를 사용하여 소비자와 관련된 제품을 식별하고 개인 설정된 쇼핑 환경을 제공해 왔습니다. 이 방법을 **제품 권장 사항**이라고 하며, 소매업체에게 상당한 수익 흐름을 창출합니다. 권장 시스템은 다음과 같은 질문에 답변하는 데 도움이 됩니다. *이 사용자가 다음에 볼 영화는 무엇인가요? 이 고객은 어떤 추가 서비스에 관심을 가질까요? 이 고객은 어디에서 휴가를 보내려고 할까요?*
최근 고객이 다음 사항에 대해 알고 싶어합니다. *소비자(구독자)가 계약을 갱신하나요?* 고객은 구독자가 계약을 갱신할 확률을 예측하는 기존 권장 모델을 가지고 있었습니다. 예측이 생성되면 추가 처리가 적용되어 응답을 예, 아니요 또는 나중에 결정으로 분류했습니다. 그런 다음, 모델 응답은 호출 센터 비즈니스 프로세스에 통합되었습니다. 이 프로세스를 통해 서비스 에이전트는 사용자에게 개인화된 권장 사항을 전달할 수 있었습니다.  
이 고객의 초기 분석 제품 중 대다수는 권장 시스템의 핵심에 있는 기계 학습 모델을 포함하여 [R 프로그래밍 언어](https://docs.microsoft.com/machine-learning-server/rebranding-microsoft-r-server)로 구축되었습니다. 구독자 기반이 커짐에 따라 데이터와 컴퓨팅 요구 사항도 증가합니다. 따라서 권장 워크로드가 이제는 매우 느리고 처리하기가 번거롭습니다. 이제 Python은 점차적으로 분석 제품 전략의 일부가 되었습니다. 그러나 가까운 시일 내에 R 투자를 유지하고 더 효율적인 개발 및 배포 프로세스를 찾아야 합니다. 과제는 Azure의 기능을 사용하여 기존 방식을 최적화하는 것이었습니다. 권장 워크로드에 개념 증명 기술 스택을 제공하고 유효성을 검사하는 작업에 착수했습니다. 여기에는 비슷한 프로젝트에 사용할 수 있는 일반적인 방법이 요약되어 있습니다.  

## <a name="design-goals"></a>설계 목표

주요 우선 순위는 모델 워크플로를 재설계 및 자동화하여 다음과 같은 몇 가지 이점을 제공하는 것이었습니다.

- 모델 개발 주기 단축 및 혁신 시간 단축. 데이터 준비 및 모델 개발 주기가 효율적이면 실험의 양이 증가할 수 있습니다.  
- 새 데이터에 대한 더 빈번한 재학습 주기를 통해 더 정확한 모델 결과 및 더 나은 권장 사항 제공
- 확장성이 더 뛰어난 POC(개념 증명) 구현 - 전체 프로덕션에서 작동
- 개발, 테스트 및 배포 프로세스를 자동화하여 프로덕션 시간 단축. 또한 자동화를 통해 운영 오류와 지연 기간을 줄일 수 있습니다.  

## <a name="requirements"></a>요구 사항

워크플로 재설계에는 다음과 같은 요구 사항이 있었습니다.

- 팀의 기존 R 기술 및 전문 지식을 활용합니다.
- 의미가 있는 위치에서 코드를 다시 사용합니다.
- 모델 학습 및 재학습 프로세스에 빠르고 쉽게 통합될 수 있도록 구독자 데이터를 데이터베이스에 저장합니다.
- 웹앱 인터페이스를 통해 다시 학습하고 점수를 매기는 모델을 호출합니다.
- Azure Active Directory를 사용하여 웹 프런트 엔드에서 인증합니다.

## <a name="technology-stack"></a>기술 스택

이 프로젝트에 대한 파이프라인에는 여러 기술과 도구가 필요했으며 다음 네 가지 영역에 집중되었습니다.

1. 구독자 데이터의 지속성 및 접근성
2. 모델의 개발, 학습 및 선택  
3. 배포 및 운영화 모델링
4. 웹 애플리케이션을 사용하여 점수 매기기 및 모델 재학습 모델 호출

파이프라인 다이어그램의 기술 구성 요소에 대해 아래에서 자세히 설명합니다.

![최적화 아키텍처](assets/recommendation-engine-optimization/recommendation-architecture.png)

### <a name="microsoft-machine-learning-server"></a>Microsoft Machine Learning 서버

R 워크로드를 선택하는 주된 이유는 **[RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler)** 및 **Microsoft ML**입니다. 이러한 패키지에 포함된 함수는 코드 전체에서 광범위하게 사용되어 데이터를 가져오고, 분류 모델을 만들고, 이를 프로덕션 환경에 배포했습니다.
MLS는 Azure에서 두 개의 Linux 가상 머신에 배포되었습니다. 하나는 "개발"용으로, 다른 하나는 "운영"용으로 구성되었습니다. 훨씬 더 많은 메모리와 처리 능력을 갖춘 개발 VM을 프로비전하여 수백 개의 모델을 학습하고 테스트할 수 있도록 했습니다. 또한 원격 사용자를 위해 RStudio IDE에 쉽게 액세스할 수 있도록 RStudio Server도 호스팅했습니다. 운영 서버는 REST API를 통해 웹 애플리케이션에서 호출할 수 있는 R 모델을 호스팅하는 데 필요한 추가 확장을 갖춘 더 작은 VM에 구성되었습니다.

### <a name="rstudio-server"></a>RStudio Server

**[RStudio Server](https://www.rstudio.com/products/rstudio/#Server)** 는 원격 또는 랩톱 클라이언트에 대한 브라우저 기반 인터페이스를 제공하는 Linux 애플리케이션입니다. Azure VM에 네트워크 보안 규칙이 만들어지면, 8787 포트에서 실행되며 원격 연결에 사용할 수 있습니다. RStudio IDE를 선호하는 분석가 및 데이터 과학자에게 큰 컴퓨팅 및 메모리 용량을 갖춘 가상 머신에 대한 액세스를 제공하는 효율적인 옵션이 될 수 있습니다. 원본 및 상용 버전 모두에서 다운로드할 수 있습니다.

### <a name="azure-sql-database"></a>Azure SQL Database

원래 구독자 데이터는 500명의 고유 구독자를 위해 600만 개 행의 구매 및 기본 설정 정보가 포함된 하나의 매우 큰 .csv 파일에 저장되었습니다. 데이터베이스에 데이터를 저장하는 것은 R 내의 더 빠른 데이터 액세스를 의미하며 필터링된 읽기를 허용합니다. 학습 또는 재학습을 위해 더 이상 전체 데이터 세트를 가져올 필요가 없습니다. 구독자가 데이터베이스 원본의 데이터를 필터링하여 데이터를 가져오고 처리하는 데 필요한 리소스가 크게 줄어듭니다.  
Azure에는 몇 가지 관리 클라우드 데이터베이스 옵션이 있습니다. 고객이 SQL Server에 익숙하므로 [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/)가 선택되었으며, 더 중요한 것은 향후 더 광범위한 규모의 Azure SQL Database에 SQL Server Machine Learning Services를 도입할 계획입니다. [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning?view=sql-server-2017)는 저장 프로시저를 통해 R 및 Python 워크로드를 실행하기 위한 데이터베이스 내 기능입니다.

### <a name="nodejs-and-reactjs"></a>Node.js 및 React.js

R 스크립트를 호출하고 웹 사이트를 보호하기 위한 웹 인터페이스가 만들어졌습니다. Node.js는 분산 환경 내에서 웹 애플리케이션을 위한 가볍고 효율적인 런타임 환경을 가능하게 하므로 웹 서버 프레임워크로 선택되었습니다. React는 사용자 인터페이스를 작성하는 데 사용되며, 프런트 엔드에 위치하고, 웹 서버에서 호스팅되는 웹 서비스를 호출합니다. Node 및 React에서 모델 파이프라인의 프로토타입 생성 웹 서비스에 대한 가장 빠른 경로를 제공할 것으로 믿었습니다.  

## <a name="infrastructure-implementation"></a>인프라 구현

아래 섹션에서는 이 프로젝트를 위해 서버 인프라가 배포된 방법에 대해 설명합니다. 적절한 개발 및 배포 인프라를 확보하는 것은 적용될 모델링 방법과 기술을 결정하는 것만큼 중요합니다.

### <a name="initial-database-load"></a>초기 데이터베이스 로드

첫 번째 단계는 매우 큰 .csv 파일에서 Azure SQL Database로 구독자 데이터를 가져오는 것이었습니다. Azure SQL Database로 데이터를 가져오는 여러 가지 옵션이 있으며, 이 [참고 자료](https://blogs.msdn.microsoft.com/sqlcat/2010/07/30/loading-data-to-sql-azure-the-fast-way/)에서 설명하고 있습니다. 작업을 수행한 방법은 다음과 같습니다.

1. [여기](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal)의 단계에 따라 Azure Portal을 통해 데이터베이스를 만들었습니다.
2. [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)를 다운로드하여 VM에서 데이터베이스에 연결하는 데 사용했습니다.
3. [SQL 가져오기/내보내기 마법사](https://docs.microsoft.com/sql/integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard?view=sql-server-2017)를 선택했습니다(시간 제한이 있는 경우 성능이 더 뛰어난 데이터 가져오기 옵션이 있음). 가져오기/내보내기 마법사는 데이터 형식을 데이터 원본에서 대상으로 매핑하며, 이 시나리오에서는 모든 데이터 요소가 허용되는 varchar(max) 데이터 형식에 매핑되었습니다. 시나리오에 다른 매핑이 필요하면 마법사에서 데이터 형식을 수정할 수 있습니다([참고 자료](https://docs.microsoft.com/sql/integration-services/import-export-data/data-type-mapping-in-the-sql-server-import-and-export-wizard?view=sql-server-2017)).  
4. 데이터베이스에 제출된 대부분의 쿼리는 *subscriber_id* 필드를 필터링하므로 해당 필드에 대한 인덱스를 만들었습니다.

### <a name="web-application"></a>웹 애플리케이션

웹 애플리케이션은 다음 세 가지 함수를 수행해야 합니다.

- 인증: 웹 사용자가 *React* 프런트 엔드를 통해 *Azure Active Directory*에 대해 인증합니다.
- 모델 점수 매기기: 특정 구독자에 대한 사용자의 입력 데이터를 받아들이고, REST API를 사용하여 구독자 데이터를 웹 서비스에 제출하고, 예측 응답을 반환합니다.  
- 모델 재학습: 구독자 식별자를 입력으로 받아들이고, 개발 서버에서 R 스크립트를 호출하여 해당 구독자에 대한 모델을 다시 학습합니다.

*Azure Active Directory*에서 SSO(Single Sign-On)를 구현하는 것은 예상보다 더 어려운 과제였습니다. 이는 SPA(단일 페이지 애플리케이션) 프레임워크 때문이었습니다. 특정 Azure Active Directory 라이브러리, 즉 [react-adal](https://github.com/salvoravida/react-adal)이 성공의 열쇠였습니다. 다음 참고 자료에서는 인증 구현에 유용한 지침을 제공했습니다.

- [Azure AD의 인증 시나리오](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios)
- [단일 페이지 애플리케이션](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios#single-page-application-spa)

### <a name="development-vm-mls-930"></a>개발 VM(MLS 9.3.0)

개발 VM은 분류 모델의 개발, 학습, 재학습 및 배포를 호스팅했습니다. Azure VM(DS13 V2)이 Linux/Ubuntu 16.10을 통해 프로비전되었으며 다음 항목이 기본 VM에 설치되었습니다.

- [여기](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-install)서 사용할 수 있는 지침에 따라 Machine Learning Server 9.3.0을 설치했습니다. 설치 확인 단계를 통해 실행하여 설치를 확인합니다. 이 설치는 개발 VM이므로 '*웹 서비스 배포 및 원격 연결 사용*' 섹션이 무시되었습니다.
- [RStudio Server](https://www.rstudio.com/products/rstudio/download-server/)(오픈 소스 버전)를 설치했습니다. R/r-base(이전에 MLS 9.3.0과 함께 설치됨)를 다시 설치하지 않도록 주의하세요.  
- VM에 [네트워크 보안 그룹을 추가](https://docs.microsoft.com/azure/virtual-machines/windows/nsg-quickstart-portal)하여 RStudio Server에 8787 포트를 통한 인바운드 연결을 허용했습니다.  
- 개발 VM과 Azure SQL Database 간의 통신을 처리하는 ODBC 드라이버를 설치했습니다. VM에 설치된 ODBC 드라이버는 다음과 같습니다.  
- Linux/Ubuntu 16.10과 호환되는 [SQL Server 17용 ODBC 드라이버](https://www.microsoft.com/download/details.aspx?id=56567)  
- [ODBC를 사용하여 관계형 데이터 가져오기](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-data-odbc)의 설치 지침에 따라 설치된 unixodbc 오픈 소스 ODBC 드라이버. 참고: 이 문서에는 Ubuntu 지침에 대한 두 가지 오타가 있습니다.  
- unixodbc가 설치되어 있는지 확인하는 경우: ````Apt list –installed | grep unixODBC (should be unixodbc)````
      - 드라이버를 설치하는 경우: ````sudo apt-get install unixodbc-devel unixodbc-bin unixodbc (should be unixodbc-dev)````

### <a name="operations-vm-mls-930"></a>운영 VM(MLS 9.3.0)

운영 VM은 모델 웹 서비스 및 엔드포인트를 호스팅하고, Swagger 파일을 저장하며, 분류 모델의 직렬화된 버전을 저장했습니다. 구성은 MLS 개발 서버와 매우 비슷합니다. 그러나 이는 REST 엔드포인트를 제공하는 데 필요한 웹 서비스가 설치되어 있음을 의미하는 운영화에 대해 구성됩니다. 운영 VM을 배포하기 위해 빠르게 배포할 수 있는 ARM 템플릿이 있습니다. 다음을 참조하세요. [ARM 템플릿을 사용하여 분석을 운영하도록 Microsoft Machine Learning Server 9.3 구성](https://blogs.msdn.microsoft.com/mlserver/2018/02/27/configuring-microsoft-machine-learning-server-9-3-to-operationalize-analytics-using-arm-templates/). 이 프로젝트의 경우 이 [ARM 템플릿](https://github.com/Microsoft/microsoft-r/tree/master/mlserver-arm-templates/one-box-configuration/linux)을 사용하여 *One-Box* 구성이 배포되었습니다.  
이를 통해 모델 파이프라인을 지원하는 서버 구성 요소가 가동되어 실행되었습니다.

## <a name="model-implementation"></a>모델 구현

한 가지 중요한 결정은 이 프로젝트의 최종 모델 설계에 영향을 미쳤으며, 단일 모놀리식 모델이 아니라 "여러 모델" 설계로 전환하는 것이었습니다. 차이점은 각 구독자에게 모든 구독자에게 서비스를 제공하는 하나의 큰 분류 모델이 아니라 자신의 고유한 분류 모델이 있다는 것입니다. 이 고객의 경우 모델이 작을수록 메모리 공간이 더 작고 프로덕션에서 수평으로 확장하기가 더 쉽기 때문에 이 방법을 선호했습니다.

### <a name="data-import"></a>데이터 가져오기

모델 개발에 필요한 모든 데이터는 *Azure SQL Database*에 있었습니다. 모델 학습 및 재학습의 경우 데이터 가져오기는 다음 두 단계로 수행되었습니다.

1. 특정 *subscriber_id*에 대한 데이터를 검색하는 쿼리가 데이터베이스에 제출되었으며 결과 세트가 반환되었습니다. 데이터베이스에 대한 쿼리 액세스에 대해 다음 두 가지 옵션이 고려되었습니다.

- [RxSQLServerData](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxsqlserverdata)라는 RevoScaleR 함수
- R odbc 패키지

데이터베이스 수준에서 데이터 필터링을 사용하도록 설정한 R "odbc" 라이브러리를 사용하도록 결정했습니다. 특정 구독자 모델에 필요한 행에 대해서만 데이터베이스 테이블을 필터링한 경우 R로 읽어들여 처리하는 행 수가 최소화되었습니다. 이를 통해 각 모델을 학습하거나 다시 학습하는 데 필요한 메모리, 컴퓨팅 및 전체 시간을 줄일 수 있었습니다.  

1. 결과 세트는 R 데이터 프레임으로 변환되었으며, 데이터 형식 중 일부는 은 분류 알고리즘에서 요구하는 대로 varchar에서 정수 또는 숫자로 명시적으로 변환되었습니다. 이 기능에는 [rxImport](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rximport) RevoScaleR 함수가 사용되었습니다. *rxImport* 함수는 RevoScaleR 및 MicrosoftML과 함께 번들로 제공되며 다중 스레드로 계층화되도록 설계되었습니다. 사용한 방법에 대한 예제는 다음과 같습니다.

````r

# Populate the data frame and modify column types as needed
input_data <- rxImport(sqlServerDataDS, colClasses = c(  
Approved="integer",
      OnTimeArrivalRate="numeric",
      Amount="numeric",
      IsInformed="numeric",
      <continue with list of columns> )
# View the characteristics of the variables in the data source
rxGetInfo (input_data, getVarInfo = TRUE)
````

## <a name="unbalanced-data"></a>불균형 데이터

권장 모델의 목표는 고객이 계약을 갱신할 확률을 예측하고 이 확률을 '예', '아니요' 또는 '나중에 결정'으로 분류하는 것이었으므로 분류 알고리즘이 사용되었습니다. 분류 알고리즘의 정확성과 성능에 심각한 영향을 줄 수 있는 한 가지 문제는 불균형 데이터 세트입니다.  
한 '클래스'에 대한 샘플이 다른 '클래스'보다 더 많은 경우 데이터 세트의 균형이 맞지 않습니다. 이 경우 각 구독자가 사용할 수 있는 행의 수가 균형을 이루지 못했습니다. 최댓값에는 한 구독자에게 100만 개 이상의 행이 있었고, 최솟값에는 330명의 고객에게 100개 미만의 데이터 행이 있었습니다. 아래 그래프에서는 구독자당 행 수(샘플)에 따른 불균형을 보여 줍니다. ![imbalance-chart](assets/recommendation-engine-optimization/recommendaton_engine_chart.png)

불균형 데이터 세트를 처리하는 한 가지 방법은 데이터 세트를 변경하고 과소 표시된 클래스를 과다 샘플링하거나 과다 표시된 클래스를 과소 샘플링하는 것입니다. 또 다른 방법은 데이터 소유자가 데이터와 해당 특성에 대한 정확한 지식을 사용하여 추가 데이터를 합성하여 생성하는 것입니다. 고객은 구독자에 대한 최소 샘플 크기의 임계값을 설정했습니다. 이 임계값 미만의 구독자에 대한 데이터를 처리해야 합니다. 이 프로젝트에서 두 가지 방법을 모두 살펴보았습니다.

## <a name="algorithm-selection"></a>알고리즘 선택

서로 다른 세 가지 분류 알고리즘 구현, 즉 *rxDForest*, *rxFastTrees* 및 *rxFastForest*가 평가되었습니다. 이 세 가지 알고리즘은 모두 다중 스레딩과 병렬 처리를 활용합니다. 그리고 Microsoft ML은 가능한 경우 다중 CPU 또는 GPU를 사용합니다. 모델 평가 조건은 다음과 같습니다.

- 새 모델이 원래 모델보다 더 정확했습니까?
- 프로덕션에서 실행되는 모델의 메모리 공간은 어떻게 됩니까? 정확성을 손상시키지 않으면서 운영 환경에서 여러 모델의 동시 실행을 실시간에 가까운 예측 응답으로 지원할 수 있습니까?
- 알고리즘에서 불균형 데이터 세트를 얼마나 잘 처리했습니까? 가상 데이터를 생성하여 불균형을 미리 처리해야 합니까?

아래 표에는 결과가 요약되어 있습니다.

| 알고리즘 | 설명 | 결과 | 메모 |
| :--------- | :------------ | :--------- | :--------------- |
| [rxFastTrees](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/rxfasttrees) | FastRank의 다중 스레드 버전을 구현하는 향상된 의사 결정 트리의 병렬 구현입니다. | 성능이 정확하고 가장 빠릅니다. | 불균형 데이터에 대한 특별한 기능이 없습니다. 사전 처리된 데이터를 입력으로 제공해야 합니다. |
| [rxFastForest](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/rxfastforest) | 임의 포리스트의 병렬 구현이며, rxFastTrees를 사용하여 의사 결정 트리의 앙상블 학습자를 작성합니다. | 사전 처리된 데이터를 사용하여 원래 모델보다 더 정확합니다. 메모리 집약도가 낮고, rxDForest보다 빠릅니다. |불균형 데이터에 대한 특별한 기능이 없습니다. 사전 처리된 데이터를 입력으로 제공합니다. |
| [rxDForest](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxdforest) | 랜덤 포리스트의 병렬 구현입니다. RevoScaleR에 포함됩니다. 함수 호출 내에서 불균형 데이터를 처리할 수 있습니다(누락된 데이터 제거, 조건 데이터 제거, 샘플 계층화 처리). | 원래 모델보다 빠릅니다. 정확도는 원래 모델과 동일하거나 더 높습니다. 다시 샘플링하고 합성하는 다양한 기술을 사용하여 불균형 데이터 세트를 처리합니다. 메모리 공간이 가장 큽니다.  | 조건부 데이터가 함수 내에 포함되어 있으므로 메모리 공간이 가장 큽니다.   데이터 처리는 양호했지만, 데이터 소유자가 제공한 사용자 지정 변환만큼 양호하지는 않았습니다. |

결과적으로, 고객은 *rxFastForest* 알고리즘을 선택했고, [vtreat](https://cran.r-project.org/web/packages/vtreat/index.html) 라이브러리를 사용하고 과소 표시된 구독자에 대한 데이터를 합성하여 생성하는 데이터 전처리 단계를 추가하여 불균형 데이터를 처리하도록 결정했습니다.

## <a name="model-deployment--web-services"></a>모델 배포 및 웹 서비스

운영 VM에 배포할 모델을 게시하는 작업은 간단하며, [mrsdeploy를 사용하여 웹 서비스로 R 모델 배포](https://docs.microsoft.com/machine-learning-server/operationalize/quickstart-publish-r-web-service) 빠른 시작 설명서에 문서화되어 있습니다.
이 시나리오에서는 개발 VM에서 모델을 만든 후에 다음 단계를 사용하여 운영 VM에 게시했습니다.

1. 인증을 위해 mrsdeploy 패키지에서 사용할 수 있는 두 가지 함수, 즉 로컬 관리자 이름과 암호를 사용하는 remoteLogin() 및 Azure Active Directory를 사용하는 remoteLoginAAD() 중 하나를 사용하여 개발자 VM에서 운영 VM으로의 원격 로그인을 설정합니다. 두 옵션은 앞에서 언급한 참고 자료에서 설명하고 있습니다. mrsdeploy를 사용하여 Machine Learning Server 또는 R Server에 로그인하고 원격 세션을 엽니다.  
2. 모델이 학습되면 mrsdeploy 패키지의 publishService 또는 updateService 함수를 사용하여 해당 모델을 운영 VM에 게시합니다. 이 프로젝트에서 여러 가지 배포 방법을 사용했으며, 방법에 따라 새 모델이 게시되거나 기존 모델이 업데이트되었습니다. 두 경우를 모두 처리하도록 구현된 코드는 다음과 같습니다.

````r
# If the service does not exist, publish it and if it does exist, update it.  
# No service by this name so publish one
 api <- publishService(serviceName, code =sc_predict, model = model,  
      inputs = list(prop_data="data.frame"),  
      outputs = list(answer = "numeric"), v = "v1.0.0" )
 print("=========== Model Created =============")
} else {
# A service by this name already exists, update it  
 api <- updateService(serviceName, model = model,
     inputs = list(prop_data="data.frame"), v = "v1.0.0" )
 print("=========== Model Updated =============")
}
 ````

모델이 배포되면 직렬화되어 운영 서버에 저장되며, *표준* 또는 *실시간* 모드에서 웹 서비스를 통해 사용될 수 있습니다. 웹 서비스를 표준 모드로 호출할 때마다 R 및 필요한 모든 라이브러리는 각 호출에 따라 로드 및 언로드됩니다. 이와 반대로 *실시간* 모드에서는 R 및 라이브러리가 한 번만 로드되고 후속 웹 서비스 호출에서 다시 사용됩니다. 웹 서비스 호출로 인한 대부분의 오버헤드는 R과 라이브러리를 로드하는 것이므로, 실시간 모드는 모델 점수 매기기에 훨씬 짧은 대기 시간을 제공하며 응답 시간은 10밀리초 미만일 수 있습니다. 표준 및 실시간 옵션 모두에 대해서는 [여기](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services)의 설명서 및 참조 예제를 참조하세요. 실시간 모드는 단일 예측에 매우 적합하지만, 점수 매기기에 대한 입력 데이터 프레임을 전달할 수도 있습니다. 이 내용은 [mrsdeploy를 사용한 일괄 처리를 통한 비동기 웹 서비스 사용](https://docs.microsoft.com/machine-learning-server/operationalize/how-to-consume-web-service-asynchronously-batch) 참고 자료에 설명되어 있습니다.

## <a name="conclusion"></a>결론

Microsoft Machine Learning Server에 기본적으로 제공된 MicrosoftML 및 RevoScaleR 라이브러리의 병렬 처리를 활용하여 수백 명의 구독자에 대한 개별 분류 모델의 개발, 배포 및 점수 매기기가 가속화되었습니다. 모델 정확도가 향상되었고, 기존의 R 코드베이스를 최소한으로 변경하여 학습 및 재학습 시간이 단축되었습니다.
모델 파이프라인을 지원하고 기술 구성 요소가 엔드투엔드에 올바르게 구성되도록 인프라를 구현하는 것은 복잡할 수 있습니다. 사용자 고유의 방법을 시작하기 위한 몇 가지 참고 자료는 다음과 같습니다.

- [Machine Learning Server 설명서](https://docs.microsoft.com/machine-learning-server/)
- [Machine Learning Server용 R 자습서](https://docs.microsoft.com/advanced-analytics/tutorials/sql-server-r-tutorials?view=sql-server-2017)
- [Machine Learning Server용 R 샘플](https://docs.microsoft.com/machine-learning-server/r/r-samples)
- [R 함수 라이브러리 참조](https://docs.microsoft.com/machine-learning-server/r-reference/introducing-r-server-r-package-reference)

## <a name="references"></a>참조

소매 비즈니스를 위한 다른 예측 솔루션을 구축하는 데 관심이 있는 경우 [Azure AI Gallery](https://gallery.azure.ai/)의 [소매 섹션](https://gallery.azure.ai/industries/retail)을 방문하세요.  