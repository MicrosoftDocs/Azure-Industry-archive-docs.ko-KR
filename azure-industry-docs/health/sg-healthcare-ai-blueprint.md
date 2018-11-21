---
title: AI용 Azure 청사진 구현
author: dastarr
ms.author: dastarr
ms.date: 08/24/2018
ms.topic: article
ms.service: industry
description: 이 문서에서는 AI용 Microsoft Azure 청사진에 대한 지침을 제공합니다.
ms.openlocfilehash: f7c9290e6bbc0d500a9f7774c2020f78b5e94aca
ms.sourcegitcommit: 76f2862adbec59311b5888e043a120f89dc862af
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "51654350"
---
# <a name="implementing-the-azure-blueprint-for-ai"></a>AI용 Azure 청사진 구현

## <a name="introduction"></a>소개

의료 조직은 AI(인공 지능) 및 ML(기계 학습)이 환자 결과 개선부터 일상적인 운영 간소화에 이르기까지 비즈니스의 많은 부분에서 중요한 도구가 될 수 있음을 인식하고 있습니다. 대체로 의료 조직에는 AI/ML 시스템을 구현하기 위한 기술 직원이 없습니다. 이러한 상황을 개선하고 Azure에서 AI/ML 솔루션을 빠르게 실행하기 위해 Microsoft는 [Azure 의료 AI 청사진](/azure/security/blueprints/azure-health?WT.mc_id=ms-docs-dastarr)을 만들었습니다. 청사진을 사용하여 안전하고 신뢰할 수 있는 보안 준수 방법으로 AI/ML을 빠르게 시작하는 방법을 보여 줍니다.

AI용 건강 청사진은 Azure를 사용하여 조직에 AI/ML을 부트스트랩합니다. 이 문서에서는 청사진 설치, 해당 구성 요소 및 이 청사진을 사용하여 환자의 입원 기간을 예측하는 AI/ML 실험을 실행하는 방법을 설명합니다.

### <a name="benefits"></a>이점

청사진은 시스템이 HIPAA 및 HITRUST 준수 요구 사항을 유지하도록 하는 등 규제가 엄격한 의료 환경에서 AI/ML을 지원하는 적절한 PaaS(Platform as a Service) 아키텍처에 대한 지침과 빠른 시작을 조직에 제공하기 위해 작성되었습니다.

의료 조직의 기술 직원은 대체로 새 프로젝트, 특히 복잡한 신기술을 배워야 하는 프로젝트에 투입할 시간이 거의 없습니다. 청사진은 기술 직원이 Azure 및 여러 서비스를 빠르게 익혀 학습 곡선의 비용을 절약하도록 지원할 수 있습니다. 기술 직원은 청사진이 설치된 후 참조 구현으로 활용하여 학습할 수 있으며, 해당 지식을 사용하여 청사진의 기능을 확장하거나 청사진과 유사한 패턴의 AI/ML 솔루션을 새로 만들 수 있습니다.

청사진을 통해 조직은 새로운 AI/ML 기능을 빠르게 가동하고 실행할 수 있습니다. AI 및 ML이 구현되고 나면, 기술 직원이 다양한 원본을 통해 수집된 데이터를 사용하여 AI/ML 실험을 실행할 수 있습니다. 예를 들어 이전 패혈증 인스턴스 및 상태와 함께 개별 환자에 대해 추적된 여러 개의 수반되는 변수에 대한 데이터가 이미 존재할 수 있습니다. 기술 직원은 이 데이터를 익명 형식으로 사용하여 환자의 잠재적인 패혈증 지표를 찾고, 상태 방지 개선을 위해 운영 절차 변경을 지원할 수 있습니다.

청사진은 환자의 입원 기간 예측 방법 학습을 위한 데이터 및 예제 코드를 제공합니다. 이는 AI/ML 솔루션의 구성 요소에 대한 학습에 사용할 수 있는 샘플 사용 사례입니다.

### <a name="platform-or-infrastructure-as-a-service"></a>PaaS(Platform as a Service) 또는 IaaS(Infrastructure as a Service)

Microsoft Azure는 PaaS 및 SaaS 제품을 둘 다 제공하며, 사용자 요구에 맞는 제품 선택은 사용 사례에 따라 다릅니다. 청사진은 환자의 입원 기간 예측 문제를 해결하는 PaaS 서비스를 사용하도록 설계되었습니다. Azure 의료 AI 청사진은 의료 조직용으로 미리 구성된 보안 및 준수 AI/ML 솔루션을 인스턴스화하는 데 필요한 모든 것을 제공합니다. 이 청사진에서 사용하는 PaaS 모델은 청사진을 전체 솔루션으로 설치하고 구성합니다.

### <a name="paas-option"></a>PaaS 옵션

PaaS 서비스 모델을 사용하면 관리할 하드웨어가 없으므로 TCO(총 소유 비용)가 감소합니다. 조직에서 하드웨어 또는 VM을 구입하고 유지 관리할 필요가 없습니다. 청사진은 PaaS 서비스만 사용합니다.

이 때문에 온-프레미스 솔루션을 유지 관리하는 비용이 감소하며, 기술 직원이 인프라가 아닌 전략적 이니셔티브에 집중할 수 있습니다. 또한 계산 및 스토리지 요금을 자본 비용 예산에서 운영 비용 예산으로 이동할 수 있습니다. 이 청사진 시나리오를 실행하는 비용은 서비스 사용량과 데이터 스토리지 비용에 따라 결정됩니다.

### <a name="iaas-option"></a>IaaS 옵션

청사진과 이 문서에서는 PaaS 구현에 중점을 두지만, IaaS(Infrastructure as a Service) 환경에서 사용할 수 있게 해주는 청사진에 대한 [오픈 소스 확장](https://github.com/Azure/Azure-Health-Extension)이 있습니다.

IaaS 호스팅 모델에서 고객은 Azure에 호스트된 VM의 가동 시간과 해당 처리 능력에 대한 요금을 지불합니다. IaaS의 경우 고객이 자신의 VM을 관리하기 때문에 제어 수준은 더 높지만 일반적으로 사용량 대비 가동 시간으로 VM 요금이 청구되기 때문에 비용이 증가합니다. 또한 패치를 적용하고 맬웨어로부터 보호하는 등 VM을 유지 관리할 책임이 고객에게 있습니다.

IaaS 모델은 청사진의 PaaS 배포에 중점을 두는 이 문서의 범위를 벗어납니다.

## <a name="the-healthcare-aiml-blueprint"></a>의료 AI/ML 청사진

청사진은 의료 컨텍스트에서 이 기술을 사용하기 위한 시작점을 만듭니다. Azure에 청사진을 설치하면 적절한 작업자, 사용 권한 및 서비스로 AI/ML 시나리오를 지원하기 위한 모든 리소스, 서비스 및 여러 사용자 계정이 생성됩니다.

청사진에는 환자의 입원 기간을 예측하는 AI/ML 실험이 포함되어 있습니다. 이 실험은 직원, 병상 개수 및 기타 물류 예측에 도움이 될 수 있습니다. 패키지에는 설치 스크립트, 예제 코드, 테스트 데이터, 보안 및 개인 정보 보호 등이 포함되어 있습니다.

## <a name="blueprint-technical-resources"></a>청사진 기술 리소스

아래 리소스는 모두 이 GitHub 리포지토리에 있습니다.

주요 리소스는 다음과 같습니다.

1. 배포, 구성 및 기타 작업용 [PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/powershell-scripting?WT.mc_id=ms-docs-dastarr) 스크립트.
2. 설치 스크립트 사용 방법을 포함하는 [자세한 설치 지침](https://github.com/Azure/Health-Data-and-AI-Blueprint/blob/master/deployment.md).
3. [종합적인 FAQ](https://github.com/Azure/Health-Data-and-AI-Blueprint/blob/master/faq.md).

이 모델에 대한 범분야 관심 사항에는 환자 데이터를 처리할 때 특히 중요한 ID 및 보안이 포함됩니다. 이 그림에는 ML 파이프라인의 구성 요소가 표시되어 있습니다.

![ML 파이프라인](assets/sg-healthcare-ai-blueprint-assets/ml-pipeline.png)

아래 그림은 설치된 Azure 제품을 보여 줍니다. 각 리소스 또는 서비스는 범분야 관심 사항인 ID 및 보안을 포함하여 AI/ML 처리 솔루션의 구성 요소를 제공합니다.

![구성 요소 영역](assets/sg-healthcare-ai-blueprint-assets/component-zones.png)

규제가 엄격한 의료 환경에서 새 시스템을 구현하는 것은 복잡합니다. 예를 들어 시스템의 모든 측면이 HIPAA를 준수하고 HITRUST 인증을 받을 수 있게 하려면 경량 솔루션 개발보다 많은 노력이 소요됩니다. 청사진은 이러한 복잡성을 지원하는 식별 및 리소스 권한을 설치합니다.

또한 청사진은 환자를 입원하거나 퇴원할 경우의 결과를 시뮬레이션하고 연구하는 데 사용되는 추가 스크립트 및 데이터를 제공합니다. 이러한 스크립트를 통해 직원은 안전하고 격리된 시나리오에서 솔루션을 사용하여 AI 및 ML을 구현하는 방법에 대한 학습을 즉시 시작할 수 있습니다.

### <a name="additional-blueprint-resources"></a>추가 청사진 리소스

청사진은 기술 직원을 위한 뛰어난 안내와 지침을 제공하며, 완벽하게 작동하는 설치를 만드는 데 도움이 되는 아티팩트도 포함합니다. 이러한 기타 아티팩트는 다음과 같습니다.

1. [Microsoft Threat Modeling Tool](https://www.microsoft.com/en-us/download/details.aspx?id=49168&WT.mc_id=ms-docs-dastarr)과 함께 사용하기 위한 [위협 모델](https://servicetrust.microsoft.com/ViewPage/HIPAABlueprint?command=Download&downloadType=Document&downloadId=01828de2-9555-4bac-a2a0-44e9ed2eeeaf&docTab=d7c399a0-2b92-11e8-9910-13dc07d708f7_Data_Analytics&WT.mc_id=ms-docs-dastarr). 이 위협 모델은 솔루션의 구성 요소, 구성 요소 간의 데이터 흐름 및 신뢰 경계를 보여 줍니다. 이 도구는 기본 청사진을 확장하려는 사용자가 위협 모델링에 사용하거나 보안 관점에서 시스템 아키텍처에 대해 알아보는 데 사용할 수 있습니다.

2. Excel 통합 문서의 [HITRUST 고객 책임 매트릭스](https://servicetrust.microsoft.com/ViewPage/HIPAABlueprint?command=Download&downloadType=Document&downloadId=eab85244-b9ab-490a-9e2a-611153f7d3af&docTab=d7c399a0-2b92-11e8-9910-13dc07d708f7_Data_Analytics&WT.mc_id=ms-docs-dastarr).  이 매트릭스는 매트릭스의 각 요구 사항에 대해 Microsoft가 제공하는 사항과 고객이 제공해야 하는 사항을 비교해서 보여 줍니다. 이 책임 매트릭스에 대한 자세한 정보는 이 문서의 보안 및 준수 > 청사진 책임 매트릭스 섹션에 포함되어 있습니다.

3. [HITRUST health data and AI review](https://servicetrust.microsoft.com/ViewPage/HIPAABlueprint?command=Download&downloadType=Document&downloadId=ffc32e44-665e-46c5-b753-163d55a17d27&docTab=d7c399a0-2b92-11e8-9910-13dc07d708f7_Data_Analytics&WT.mc_id=ms-docs-dastarr)(HITRUST 건강 데이터 및 AI 검토) 백서에서는 HITRUST 인증을 받기 위해 충족해야 하는 요구 사항의 렌즈를 통해 청사진을 검사합니다.

4. [HIPAA health data and AI review](https://servicetrust.microsoft.com/ViewPage/HIPAABlueprint?command=Download&downloadType=Document&downloadId=d5ce675c-3e83-45db-98a6-ae77fc439436&docTab=d7c399a0-2b92-11e8-9910-13dc07d708f7_Data_Analytics&WT.mc_id=ms-docs-dastarr)(HIPAA 건강 데이터 및 AI 검토) 백서에서는 HIPAA 규정을 기반으로 해서 아키텍처를 검토합니다.

이러한 리소스는 [GitHub의 여기](https://github.com/Azure/Health-Data-and-AI-Blueprint)에서 확인할 수 있습니다.

## <a name="installing-the-blueprint"></a>청사진 설치

이 청사진 솔루션을 가동하고 실행하는 데는 시간이 별로 소요되지 않습니다. 약간의 PowerShell 스크립팅 지식이 권장되지만, 설치를 안내하는 단계별 지침이 제공되므로 기술자는 스크립팅 기술에 관계없이 이 청사진을 성공적으로 배포할 수 있습니다.

Azure 사용 경험이 거의 없는 기술 직원도 30분에서 1시간 이내에 청사진을 설치할 수 있습니다.

### <a name="the-installation-script"></a>설치 스크립트

청사진은 뛰어난 설치 안내와 지침을 제공합니다. 또한 청사진은 청사진 서비스 및 리소스의 설치와 제거를 위한 스크립팅을 제공합니다. PowerShell 배포 스크립트 호출은 간단합니다. 청사진을 설치하기 전에 특정 데이터를 수집하여 아래와 같이 deploy.ps1 스크립트에 대한 인수로 사용해야 합니다.

```powershell
.\deploy.ps1 -deploymentPrefix <prefix> `
            -tenantId <tenant id> ` # also known as the AAD directory
            -tenantDomain <tenant domain> `
            -subscriptionId <subscription id> `
            -globalAdminUsername <user id> ` # ID from your AAD account
            -deploymentPassword <universal password> ` # applied to all new users and service accounts
            -appInsightsPlan 1 # we want app insights set up
```

### <a name="the-installation-environment"></a>설치 환경

**중요!** Azure 외부 머신에서 청사진을 설치하지 마세요. Azure에서 Windows 10(또는 다른 Windows VM)을 새로 만들고 여기서 설치 스크립트를 실행하면 설치가 성공할 가능성이 훨씬 더 큽니다. 이 기술은 클라우드 기반 VM을 사용하여 대기 시간을 완화하고 원활한 설치를 만들도록 지원합니다.

설치 중에 스크립트는 로드하고 사용할 다른 패키지를 호출합니다. Azure의 VM에서 설치하는 경우 설치 머신과 대상 리소스 간의 지연이 훨씬 낮아집니다. 그러나 스크립트 패키지가 Azure 환경 외부에 상주하기 때문에 다운로드한 스크립팅 패키지 중 일부는 여전히 대기 시간에 취약하며, 이로 인해 시간 초과 실패가 발생할 수 있습니다.

### <a name="install-failure-dont-panic"></a>설치 실패! (겁 먹지 마세요.)

설치 관리자는 설치 중에 일부 외부 패키지를 다운로드합니다. 때로는 설치 머신과 패키지 간의 지연으로 인해 스크립트 리소스 요청이 시간 초과되는 경우가 있습니다. 이 경우 다음 두 가지 선택 사항이 있습니다.

1. 변경 사항 없이 설치 스크립트를 다시 실행합니다. 설치 관리자가 이미 할당된 리소스를 확인하고 필요한 리소스만 설치합니다. 이 기술이 작동할 수도 있지만, 설치 스크립트가 이미 설치된 리소스를 할당하려고 시도할 위험이 있습니다. 이 경우 오류가 발생할 수 있으며 설치에 실패합니다.

2. 청사진 서비스를 제거하려는 경우 여전히 deploy.ps1 스크립트를 실행하지만 다른 인수를 전달합니다. 

```powershell
.\deploy.ps1 -clearDeploymentPrefix <prefix> `
             -tenantId <value> `
             -subscriptionId <value> `
             -tenantDomain <value> `
             -globalAdminUsername <value> `
             -clearDeployment
```

제거가 완료된 후 설치 스크립트의 접두사를 변경하고 다시 설치를 시도합니다. 대기 시간 문제가 다시 발생하지 않을 수 있습니다. 스크립트 패키지를 다운로드하는 동안 설치에 실패하는 경우 제거 프로그램 스크립트를 실행한 후 설치 관리자를 다시 실행합니다.

제거 스크립트를 실행하면 다음이 사라집니다.

- 설치 관리자 스크립트가 설치한 사용자
- 리소스 그룹 및 데이터 스토리지를 포함한 해당 서비스
- AAD(Azure Active Directory)에 등록된 애플리케이션

Key Vault는 “소프트 삭제”로 유지되며 포털에는 표시되지 않지만 30일 동안 할당 해제되지 않습니다. 이렇게 하면 필요한 경우 Key Vault를 다시 구성할 수 있습니다. 이 기술의 의미와 처리 방법에 대한 자세한 내용은 이 문서의 기술 문제 > Key Vault 섹션을 참조하세요.

### <a name="reinstall-after-an-uninstall"></a>제거 후 다시 설치

청사진을 제거한 후 다시 설치해야 하는 경우 접두사를 변경하지 않으면 제거된 Key Vault로 인해 오류가 발생하므로 다음 배포의 접두사를 변경해야 합니다. 이 기술에 대한 자세한 내용은 이 문서의 _기술 문제 > Key Vault_ 섹션을 참조하세요.

### <a name="required-administrator-roles"></a>필수 관리자 역할

청사진을 설치하는 사람은 AAD의 전역 관리자 역할에 있어야 합니다. 또한 설치 계정이 사용 중인 구독에 대한 Azure 구독 관리자여야 합니다. 설치를 수행하는 사람이 두 역할에 모두 속하지 않을 경우 설치에 실패합니다.

![청사진 설치 관리자](assets/sg-healthcare-ai-blueprint-assets/blueprint-installer.png)

또한 이 설치는 AAD와의 긴밀한 통합으로 인해 MSDN 구독에서 작동하도록 설계되지 않았습니다. 표준 Azure 계정을 사용해야 합니다. 필요한 경우, 청사진 솔루션을 설치하고 해당 데모를 실행하는 데 사용할 크레딧을 포함하는 [평가판을 다운로드](https://azure.microsoft.com/en-us/free/?WT.mc_id=ms-docs-dastarr)합니다.

## <a name="adding-other-resources"></a>다른 리소스 추가

Azure 청사진 설치에는 AI/ML 사용 사례 구현에 필요한 것보다 많은 서비스가 포함되어 있지 않습니다. 그러나 더 많은 리소스 또는 서비스를 Azure 환경에 추가하여 추가 이니셔티브에 적합한 테스트 베드나 프로덕션 시스템의 시작점으로 만들 수 있습니다. 예를 들어 동일한 구독 및 AAD에 다른 PaaS 서비스 또는 IaaS 리소스를 추가할 수 있습니다.

더 많은 Azure 기능이 필요할 경우 [Cosmos DB](/azure/cosmos-db/introduction?WT.mc_id=ms-docs-dastarr)와 같은 새 리소스나 새 [Azure Functions](/azure/azure-functions/functions-overview?WT.mc_id=ms-docs-dastarr)를 솔루션에 추가할 수 있습니다. 새 리소스 또는 서비스를 추가하는 경우 보안 및 개인 정보 보호 정책이 규정 및 정책을 준수하도록 구성되었는지 확인합니다.

[Azure REST API](https://docs.microsoft.com/en-us/rest/api/?view=Azure&WT.mc_id=ms-docs-dastarr), [Azure PowerShell 스크립팅](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps?view=azurermps-6.6.0&WT.mc_id=ms-docs-dastarr) 또는 [Azure Portal](http://portal.azure.com/?WT.mc_id=ms-docs-dastarr)을 사용하여 새 리소스와 서비스를 만들 수도 있습니다.

## <a name="using-machine-learning-with-the-blueprint"></a>청사진과 함께 기계 학습 사용

청사진은 [환자의 입원 기간](/azure/security/blueprints/azure-health?WT.mc_id=ms-docs-dastarr#example-use-case)을 예측하기 위해 모델에 회귀 알고리즘을 사용하는 ML 시나리오를 보여 주도록 작성되었습니다. 이 예측은 직원 및 기타 운영 결정을 예약하는 데 도움이 되므로 의료 공급자가 실행하는 일반적인 예측입니다. 또한 시간이 지남에 따라 지정된 상태의 평균 입원 기간이 증가하거나 감소하는 변칙을 검색할 수 있습니다.

### <a name="ingesting-training-data"></a>학습 데이터 수집

청사진이 설치되고 모든 서비스가 제대로 작동하면 분석할 데이터를 수집할 수 있습니다. 100,000명의 환자 [기록을 수집](/azure/security/blueprints/azure-health?WT.mc_id=ms-docs-dastarr#ingest)하고 모델에서 작업할 수 있습니다. 환자 기록 수집은 [Azure Machine Learning Studio](/azure/machine-learning/studio/what-is-ml-studio?WT.mc_id=ms-docs-dastarr)를 사용하여 아래와 같이 환자의 입원 기간 실험을 실행하는 첫 번째 단계입니다.

![수집](assets/sg-healthcare-ai-blueprint-assets/ingest.png)

청사진에는 MLS(Machine Learning Studio)에서 ML 작업을 실행하는 데 필요한 데이터와 실험이 포함되어 있습니다. 예제에서는 학습된 모델을 실험에 사용하여 많은 변수를 기준으로 환자의 입원 기간을 예측합니다.

이 데모 환경에서 Azure SQL Database에 수집된 데이터에는 결함이나 누락된 데이터 요소가 없습니다. 이 데이터는 정리되었습니다. 정리되지 않은 데이터가 수집되는 경우도 많으며, 이러한 데이터는 “정리”해야 기계 학습의 학습 알고리즘 피드에 사용하거나 ML 작업에 사용할 수 있습니다. 누락된 데이터 또는 데이터의 잘못된 값은 ML 분석 결과에 부정적인 영향을 줍니다.

## <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio

많은 의료 조직에는 ML 프로젝트에 집중할 기술 직원이 없습니다. 이 때문에 중요한 데이터가 사용되지 않거나, 비용이 많이 드는 컨설턴트를 고용해 ML 솔루션을 만드는 경우가 많습니다.

AI/ML 전문가뿐 아니라 AI/ML 초보자도 Azure Machine Learning Studio를 사용하여 실험을 설계할 수 있습니다. MLS는 ML 실험을 만드는 데 사용되는 웹 기반 설계 환경입니다. MLS에서 모델을 만들고 학습, 평가 및 채점하면 다른 도구를 사용하여 모델을 개발하는 경우에 비해 귀중한 시간을 절약할 수 있습니다.

MLS는 ML 워크로드에 대한 전체 도구 집합을 제공합니다. 이는 ML 초보자도 바로 도구를 사용할 수 있으며, 다른 ML 도구를 사용하는 경우에 비해 결과를 더 빠르게 생성할 수 있음을 의미합니다. 이 덕분에 IT 직원이 ML 전문가 없이 다른 곳에서 가치를 제공하는 데 집중할 수 있습니다. 사용자 고유의 의료 조직에서 이 기능은 환자 간섭주의가 끌어서 놓기 캔버스에 사용할 [미리 작성된 모듈](/azure/machine-learning/studio-module-reference/index?WT.mc_id=ms-docs-dastarr#help-for-machine-learning-modules)을 제공하여 시각적으로 엔드투엔드 데이터 과학 워크플로를 실험으로 작성하는 것처럼 다양한 가설을 테스트하고 작업 가능한 인사이트를 위해 결과 데이터를 분석할 수 있음을 의미합니다.

의사 결정 트리, 의사 결정 포리스트, 클러스터링, 시계열, 변칙 검색 등의 특정 알고리즘을 캡슐화하는 미리 작성된 모듈이 있습니다.

사용자 지정 모듈을 임의 실험에 추가할 수 있습니다. 이러한 모듈은 [R 언어](/azure/machine-learning/studio/extend-your-experiment-with-r?WT.mc_id=ms-docs-dastarr) 또는 [Python](/azure/machine-learning/studio/execute-python-scripts?WT.mc_id=ms-docs-dastarr)으로 작성됩니다. 이 때문에 미리 빌드된 모듈과 사용자 지정 논리를 사용하여 보다 정교한 실험을 만들 수 있습니다.

MLS에서는 학습 모델을 [만들고 사용](/azure/machine-learning/studio-module-reference/machine-learning-initialize-model?WT.mc_id=ms-docs-dastarr)할 수 있으며, 일반적인 애플리케이션에서 사용할 미리 설계된 일련의 실험을 제공할 수도 있습니다. 또한 청사진의 리소스를 변경하지 않고 새 실험을 MLS에 추가할 수 있습니다.

시간을 절약하려면 [Azure AI Gallery](https://gallery.azure.ai/industries/healthcare?WT.mc_id=ms-docs-dastarr)를 방문하여 의료를 비롯한 특정 산업에 바로 사용할 수 있는 ML 솔루션을 찾아보세요. 예를 들어 갤러리에는 유방암 검색 및 심장병 예측을 위한 솔루션과 실험이 포함되어 있습니다.

## <a name="security-and-compliance"></a>보안 및 규정 준수

보안 및 준수는 의료 환경에서 소프트웨어 시스템을 만들고 설치 또는 관리할 때 주의해야 하는 가장 중요한 사항 중 두 개입니다. 필수 보안 정책 및 인증을 충족하지 않을 경우 소프트웨어 시스템을 채택하면서 투자한 것이 제대로 효과를 낼 수 없습니다.

이 문서와 의료 청사진에서는 기술 보안에 중점을 두지만, 물리적 보안 및 관리 보안을 비롯한 기타 유형의 보안도 중요합니다. 이러한 보안 주제는 청사진의 기술 보안에 중점을 두는 이 문서의 범위를 벗어납니다.

### <a name="principle-of-least-privilege"></a>최소 권한 원칙

청사진은 역할과 함께 명명된 사용자를 설치하여 솔루션의 리소스에 대한 액세스 요구를 지원 및 제한합니다. 이 모델을 시스템 설계에서 리소스 액세스에 대한 접근 방식인 “최소 권한 원칙”이라고 합니다. 원칙은 서비스 및 사용자 계정이 합법적인 목적으로 필요한 시스템 및 서비스에만 액세스할 수 있도록 해야 한다는 것입니다.

이 보안 모델은 시스템이 HIPAA 및 HITRUST 요구 사항을 준수하도록 하여 조직의 위험을 제거합니다.

### <a name="defense-in-depth"></a>심층 방어

여러 추상화 계층의 보안 제어를 사용하는 시스템 설계는 심층 방어를 사용하는 것입니다. 심층 방어는 여러 수준에서 보안 중복성을 제공합니다. 이는 단일 계층의 방어에 종속되지 않음을 의미합니다. 이렇게 하면 사용자 및 서비스 계정이 리소스, 서비스 및 데이터에 대한 적절한 액세스 권한을 갖게 됩니다. Azure는 시스템 아키텍처의 모든 수준에서 보안 및 모니터링 리소스를 제공하여 전체 기술 환경에 대한 심층 방어를 제공합니다.

청사진을 통해 설치되는 것과 같은 소프트웨어 시스템에서는 사용자가 로그인할 수 있지만 특정 리소스에 대한 사용 권한이 없을 수 있습니다. 이 심층 방어 예제는 RBAC(역할 기반 액세스 제어) 및 AAD를 통해 제공되며, 최소 권한 원칙을 지원합니다.

2단계 인증도 기술 심층 방어의 한 형태이며, 청사진을 설치할 때 선택적으로 포함할 수 있습니다.

### <a name="azure-key-vault"></a>Azure Key Vault

Azure Key Vault 서비스는 비밀, 인증서 및 애플리케이션에서 사용하는 기타 데이터를 저장하는 데 사용되는 컨테이너(자격 증명 모음)입니다. 여기에는 데이터베이스 문자열, REST 엔드포인트 URL, API 키 및 개발자가 애플리케이션에 하드 코드하거나 .config 파일에서 배포하지 않으려는 기타 항목이 포함됩니다.

또한 자격 증명 모음은 애플리케이션 서비스 ID 또는 AAD 사용 권한이 있는 다른 계정에서 액세스할 수 있습니다. 이 경우 자격 증명 모음의 콘텐츠가 필요한 애플리케이션이 런타임에 비밀에 액세스할 수 있습니다.

자격 증명 모음에 저장된 키를 암호화 또는 서명할 수 있으며, 모든 보안 문제에 대해 키 사용을 모니터링할 수 있습니다.

Key Vault를 삭제하는 경우 Azure에서 즉시 제거되지 않습니다. 이 기술의 의미는 이 문서의 _기술 문제 > Key Vault_ 섹션을 참조하세요.

### <a name="application-insights"></a>Application Insights

의료 조직에는 중요 업무용 시스템 및 사람의 생명과 관련된 시스템이 있는 경우가 많으며, 해당 시스템은 안정적이고 복원력이 있어야 합니다. 서비스의 변칙 또는 중단이 가능한 한 빨리 검색되고 수정되어야 합니다. [Application Insights](/azure/application-insights/app-insights-proactive-application-security-detection-pack?WT.mc_id=ms-docs-dastarr)는 애플리케이션을 모니터링하고 문제가 발생할 때 경고를 보내는 APM(애플리케이션 성능 관리) 기술입니다. Application Insights는 런타임 시 오류 또는 애플리케이션 변칙에 대해 애플리케이션을 모니터링합니다. 여러 프로그래밍 언어에서 작동하도록 설계되었으며, 애플리케이션이 정상이고 원활하게 실행되고 있는지 확인하는 풍부한 기능 집합을 제공합니다.

예를 들어 애플리케이션에 메모리 누수가 있을 수 있습니다. Application Insights는 모니터링하는 다양한 보고 기능 및 KPI를 통해 이러한 문제를 찾고 진단하도록 지원할 수 있습니다. Application Insights는 애플리케이션 개발자를 위한 강력한 APM 서비스입니다.

이 [대화식 데모](https://analytics.applicationinsights.io/demo#/discover/home?WT.mc_id=ms-docs-dastarr)는 의료 조직의 기술자가 애플리케이션 상태 및 성능 상태를 모니터링하는 데 사용할 수 있는 종합적인 모니터링 대시보드를 포함하여 Application Insights의 주요 기능 및 특징을 보여 줍니다.

#### <a name="azure-security-center"></a>Azure Security Center

실시간 보안 및 KPI 모니터링은 중요 업무용 애플리케이션에서 반드시 필요한 기능입니다. ASC(Azure Security Center)는 Azure 리소스가 안전하고 보호되도록 지원합니다. ASC는 보안 관리 및 지능형 위협 방지 서비스입니다. ASC를 사용하여 워크로드에 보안 정책을 적용하고, 위협에 대한 노출을 제한하고, 공격을 탐지 및 대응할 수 있습니다.

Security Center 표준은 다음 서비스를 제공합니다.

- **하이브리드 보안** – 모든 온-프레미스 및 클라우드 워크로드의 보안을 통합해서 확인할 수 있습니다. 의료 조직이 Azure와 함께 사용하는 하이브리드 클라우드 네트워크에서 특히 유용합니다.
- **지능형 위협 탐지** - ASC는 고급 분석 및 [Microsoft Intelligent Security Graph](http://cloud-platform-assets.azurewebsites.net/intelligent-security-graph/?WT.mc_id=ms-docs-dastarr)를 사용하여 갈수록 발전하는 사이버 공격을 효율적으로 대응하고 즉시 완화합니다.
- **액세스 및 애플리케이션 제어** - 기계 학습을 통해 제공되며 특정 워크로드에 맞게 조정되는 허용 목록 권장 사항을 적용하여 맬웨어 및 기타 원치 않는 애플리케이션을 차단합니다.

건강 AI 청사진 컨텍스트에서 ASC는 시스템 구성 요소를 분석하고 구독에 포함된 서비스 및 리소스의 취약성을 보여 주는 대시보드를 제공합니다. 개별 대시보드 요소는 다음과 같은 솔루션의 관심 사항에 대한 가시성을 제공합니다.

- 정책 및 규정 준수
- 리소스 보안 예방 조치
- 위협 보호

다음은 시스템 위협 취약성을 개선하기 위한 13가지 제안을 확인하는 예제 대시보드입니다. 또한 HIPAA 및 정책 준수가 46%에 불과함을 보여 줍니다.

![위협 보호](assets/sg-healthcare-ai-blueprint-assets/threat_protection.png)

심각도가 높은 보안 문제를 드릴하면 아래와 같이 영향을 받는 리소스와 각 리소스에 필요한 수정 작업이 표시됩니다.

IT 직원은 수동으로 모든 리소스 및 네트워크의 보안을 유지하는 데 많은 시간을 소비할 수 있습니다. ASC를 통해 지정된 시스템의 취약성을 확인하면 다른 전략적 작업에 시간을 할애할 수 있습니다. 확인된 여러 취약성에 대해 ASC는 관리자가 문제를 자세히 조사하지 않아도 자동으로 수정 작업을 적용하고 리소스의 보안을 유지할 수 있습니다.

![위험 수준 높음](assets/sg-healthcare-ai-blueprint-assets/high_risks.png)

ACS는 위협 탐지 및 경고 기능을 통해 더 많은 작업을 수행합니다. ACS를 사용하여 네트워크, 머신 및 클라우스 서비스에서 들어오는 공격 및 위반 후 활동을 모니터링함으로써 환경의 보안을 유지할 수 있습니다.
ASC는 다양한 Azure 리소스에서 보안 정보와 로그를 자동으로 수집, 분석 및 통합합니다. 

ASC의 ML 기능을 통해 수동 방법으로는 발견되지 않는 위협을 탐지할 수 있습니다. 우선 순위가 지정된 보안 경고 목록이 문제를 신속하게 조사하는 데 필요한 정보 및 공격을 수정하는 방법에 대한 권장 사항과 함께 ASC에 표시됩니다.

### <a name="rbac-security"></a>RBAC 보안

RBAC([역할 기반 액세스 제어](/azure/role-based-access-control/role-assignments-portal?WT.mc_id=ms-docs-dastarr))는 때때로 리소스별 특정 권한을 사용하여 보호된 리소스에 대한 액세스 권한을 제공하거나 거부합니다. 이렇게 하면 적절한 사용자만 지정된 시스템 구성 요소에 액세스할 수 있습니다. 예를 들어 데이터베이스 관리자는 암호화된 환자 데이터를 포함하는 데이터베이스에 액세스할 수 있는 반면, 의료 공급자는 환자 기록을 표시하는 애플리케이션을 통해 해당 환자의 기록에만 액세스할 수 있습니다. 일반적으로 전자 의료 기록 또는 전자 건강 기록 시스템입니다. 간호사는 데이터베이스에 액세스할 필요가 없고, 데이터베이스 관리자는 환자의 건강 기록 데이터를 확인할 필요가 없습니다.

이 기능을 사용하기 위해 RBAC가 Azure 보안에 통합되었으며 Azure 리소스에 대한 정확히 집중화된 액세스 관리를 가능하게 합니다. 각 사용자에 대한 세분화된 설정을 통해 보안 및 시스템 관리자는 각 사용자에게 제공하는 권한을 정확하게 지정할 수 있습니다.

### <a name="blueprint-responsibility-matrix"></a>청사진 책임 매트릭스

HITRUST 고객 책임 매트릭스는 고객이 Azure에 빌드된 시스템에 대한 보안 제어를 구현하고 문서화할 수 있도록 지원하는 Excel 문서입니다. 통합 문서에는 관련 HITRUST 요구 사항이 나와 있으며, 각각의 HITRUST 요구 사항을 충족하기 위한 Microsoft와 고객의 책임에 대해 설명합니다.

Azure에 시스템을 빌드하는 고객은 클라우드 환경에서 보안 제어를 구현하기 위한 공유 책임을 이해해야 합니다. 특정 보안 제어 구현은 Microsoft 책임, 고객 책임 또는 Microsoft와 고객의 공유 책임일 수 있습니다. 클라우드 구현마다 Microsoft와 고객 간에 책임이 공유되는 방식이 다릅니다.

예제를 보려면 아래의 책임 표를 참조하세요.

|Azure 책임  |고객 책임  |
|---------|---------|
|Azure는 서비스 프로비전 환경과 관련하여 정보 보호 프로그램 방법 및 메커니즘의 구현, 관리 및 모니터링을 담당합니다. | 고객은 Azure 서비스를 액세스하고 이용하는 데 사용되는 고객 제어 자산에 대한 정보 보호 프로그램 방법 및 메커니즘의 구현, 구성, 관리 및 모니터링을 담당합니다. |
|Azure는 서비스 프로비전 환경과 관련하여 계정 관리 방법 및 메커니즘의 구현, 구성, 관리 및 모니터링을 담당합니다. |또한 고객은 배포된 Azure 가상 머신 인스턴스 및 상주 애플리케이션 구성 요소의 계정 관리를 담당합니다.|

이러한 책임은 클라우드 시스템을 배포할 때 고려해야 하는 많은 책임의 두 가지 예일 뿐입니다. HITRUST 고객 책임 매트릭스는 Azure 시스템 구현에서 조직의 HITRUST 준수를 지원하도록 설계되었습니다.

## <a name="customization"></a>사용자 지정

설치 후 청사진을 사용자 지정하는 것이 일반적입니다. 환경을 사용자 지정하는 이유와 기술은 다양합니다.

설치 스크립트를 수정하여 설치 전에 청사진을 사용자 지정할 수도 있습니다. 이 작업도 가능하지만 초기 설치가 완료된 후에 실행할 독립적인 PowerShell 스크립트를 만드는 것이 좋습니다. 초기 설치가 수행되고 나면 포털을 통해 새 서비스를 시스템에 추가할 수도 있습니다.

사용자 지정에는 다음 중 하나 이상이 포함될 수 있습니다.

- Machine Learning Studio에 새 실험 추가
- 환경에 관련 없는 서비스 추가
- Azure SQL patientdb 데이터베이스가 아닌 다른 데이터 원본을 사용하도록 데이터 수집 및 ML 실험 출력 수정
- ML 실험에 프로덕션 데이터 제공
- 실험에 필요한 데이터와 일치하도록 수집 중인 소유 데이터 정리

설치 사용자 지정은 Azure 솔루션 작업과 다르지 않습니다. 서비스 또는 리소스를 추가하거나 제거하여 새로운 기능을 제공할 수 있습니다. 청사진을 사용자 지정하는 경우 구현이 계속 작동할 수 있게 전반적인 ML 파이프라인을 변경하지 않도록 주의하세요.

## <a name="technical-issues"></a>기술 문제

다음 문제로 인해 청사진 설치에 실패하거나 원치 않는 구성으로 설치될 수 있습니다.

### <a name="key-vault"></a>Key Vault

키 자격 증명 모음은 Azure 리소스를 삭제할 때 고유한 특성이 있습니다. 자격 증명 모음은 복구 목적으로 Azure에 유지됩니다. 따라서 설치 스크립트를 실행할 때마다 다른 접두사를 설치 스크립트에 전달해야 합니다. 그러지 않으면 이전 자격 증명 모음 이름과 충돌하여 설치에 실패합니다. 키 자격 증명 모음 및 기타 모든 리소스의 이름은 설치 스크립트에 제공하는 접두사를 사용하여 지정됩니다.

설치 스크립트에서 만든 Key Vault는 30일 동안 “소프트 삭제”로 보존됩니다. 포털을 통해 현재 액세스할 수는 없지만 소프트 삭제된 [Key Vault는 PowerShell에서 관리할 수 있으며](/azure/key-vault/key-vault-soft-delete-powershell?WT.mc_id=ms-docs-dastarr) 수동으로 삭제할 수도 있습니다.

### <a name="azure-active-directory"></a>Azure Active Directory

프로덕션 시스템이 아닌 빈 AAD에 청사진을 설치하는 것이 좋습니다. 새 AAD 인스턴스를 만들고 설치 중에 해당 테넌트 ID를 사용하여 라이브 AAD 인스턴스에 청사진 계정이 추가되지 않도록 합니다.

## <a name="technologies-presented"></a>제공되는 기술

- [Azure 건강 데이터 및 AI 청사진](/azure/security/blueprints/azure-health?WT.mc_id=ms-docs-dastarr)에 대해 자세히 알아보세요.
- [여기에서 GitHub 리포지토리](https://github.com/Azure/Health-Data-and-AI-Blueprint/blob/master/deployment.md)를 다운로드, 복제 또는 포크합니다.
- [Machine Learning Studio](/azure/machine-learning/studio/?WT.mc_id=ms-docs-dastarr)는 데이터 과학자가 Machine Learning 실험을 만드는 데 사용하는 작업 영역 및 도구입니다. 기본 제공 알고리즘, 특수 목적 위젯, Python 및 R 스크립트를 사용할 수 있습니다.
- 비밀, 인증서 및 기타 개인 데이터는 [Azure Key Vault](/azure/key-vault/key-vault-whatis?WT.mc_id=ms-docs-dastarr)에 보관됩니다.
- 필요한 명령은 설치 지침에 제공되지만, 스크립팅 언어 PowerShell이 청사진 설정에 중요합니다.
- [Azure AI Gallery](https://docs.microsoft.com/en-us/powershell/scripting/powershell-scripting?WT.mc_id=ms-docs-dastarr)는 업계에서 고객에게 유용한 AI/ML 솔루션의 레시피 상자를 제공합니다. 다른 의료 전문가와 함께 데이터 과학자가 게시한 여러 가지 솔루션이 있습니다.
- [Azure Security Center](/azure/security-center/?WT.mc_id=ms-docs-dastarr)는 애플리케이션의 동작, 취약성 및 완화 기술에 대한 인사이트를 제공합니다.
- [Microsoft Threat Modeling Tool](/azure/security/blueprints/azure-health?WT.mc_id=ms-docs-dastarr)은 시스템 환경에 대한 위협을 계획하고 예측하는 데 사용됩니다. 청사진에 포함된 위협 모델을 검토해야 합니다.

## <a name="wrapping-up"></a>요약

[Azure 건강 데이터 AI 청사진](/azure/security/blueprints/azure-health?WT.mc_id=ms-docs-dastarr)은 완전한 ML 솔루션이며, Azure 및 시스템이 의료 규제 요구 사항을 준수하도록 하는 방법을 보다 잘 이해하기 위한 기술자용 학습 도구로 사용될 수 있습니다. Azure Machine Learning Studio를 중심으로 하여, 프로덕션 시스템의 시작점으로 사용할 수도 있습니다.

[청사진을 다운로드](https://github.com/Azure/Health-Data-and-AI-Blueprint/blob/master/deployment.md)하면 며칠이나 몇 주가 아닌 몇 시간 이내에 구현을 시작할 수 있습니다. 설치와 관련된 문제가 있거나 청사진에 대한 궁금한 점이 있으면 [FAQ 페이지](https://github.com/Azure/Health-Data-and-AI-Blueprint/blob/master/faq.md)를 방문하세요.

설치 및 ML 실험 이외에 청사진 구현에 대한 추가 정보를 얻으려면 지원 자료를 다운로드합니다. 이 자료에는 다음이 포함되어 있습니다.

1. [HITRUST 고객 책임 매트릭스](https://servicetrust.microsoft.com/ViewPage/HIPAABlueprint?command=Download&downloadType=Document&downloadId=eab85244-b9ab-490a-9e2a-611153f7d3af&docTab=d7c399a0-2b92-11e8-9910-13dc07d708f7_Data_Analytics&WT.mc_id=ms-docs-dastarr)
2. [종합적인 위협 모델](https://servicetrust.microsoft.com/ViewPage/HIPAABlueprint?command=Download&downloadType=Document&downloadId=01828de2-9555-4bac-a2a0-44e9ed2eeeaf&docTab=d7c399a0-2b92-11e8-9910-13dc07d708f7_Data_Analytics&WT.mc_id=ms-docs-dastarr)
3. [HITRUST health data and AI review](https://servicetrust.microsoft.com/ViewPage/HIPAABlueprint?command=Download&downloadType=Document&downloadId=ffc32e44-665e-46c5-b753-163d55a17d27&docTab=d7c399a0-2b92-11e8-9910-13dc07d708f7_Data_Analytics&WT.mc_id=ms-docs-dastarr)(HITRUST 건강 데이터 및 AI 검토) 백서
4. [HIPAA health data and AI review](https://servicetrust.microsoft.com/ViewPage/HIPAABlueprint?command=Download&downloadType=Document&downloadId=d5ce675c-3e83-45db-98a6-ae77fc439436&docTab=d7c399a0-2b92-11e8-9910-13dc07d708f7_Data_Analytics?WT.mc_id=ms-docs-dastarr)(HIPAA 건강 데이터 및 AI 검토)

청사진을 학습 목적으로 사용하든 또는 조직에 대한 AI/ML 솔루션의 시드로 사용하든 간에 청사진은 의료를 중심으로 하여 Azure에서 AI/ML 작업을 시작하는 지점을 제공합니다.