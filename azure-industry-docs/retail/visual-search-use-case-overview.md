---
title: Visual Search 개요
author: scseely
ms.author: scseely, mazoroto
ms.date: 07/16/2018
ms.topic: article
ms.service: industry
description: 이 문서에서는 전자상거래 인프라를 온-프레미스에서 Azure로 마이그레이션하는 단계에 대해 설명합니다.
ms.openlocfilehash: 0c80e3068a1b23bf12d2468489fdd0b67c660dfa
ms.sourcegitcommit: 76f2862adbec59311b5888e043a120f89dc862af
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "51654210"
---
# <a name="visual-search-overview"></a>Visual Search 개요

## <a name="executive-summary"></a>요약

현재 알려진 것처럼, 인공 지능은 소매를 변혁할 수 있는 잠재력을 제공합니다. 소매점이 AI를 통해 지원되는 고객 환경 아키텍처를 개발할 것은 분명합니다. AI로 개선된 플랫폼에서 하이퍼 개인 설정으로 인해 매출이 급증할 것이라는 기대도 있습니다. 디지털 상거래는 고객 기대, 기본 설정 및 행동의 수준을 계속해서 높이고 있습니다. 실시간 참여, 관련 권장 사항 및 하이퍼 개인 설정과 같은 요구로 인해 단추만 한 번 클릭하면 되는 편의성과 속도가 개발되고 있습니다. 말하기, 눈 등을 통해 애플리케이션에서 인텔리전스를 사용하면 고객의 쇼핑 방식을 방해하는 동시에 가치를 높이는 소매의 개선 사항이 활성화됩니다.

이 문서에서는 **시각적 검색**의 AI 개념에 대해 중점적으로 설명하고, 해당 구현과 관련해서 고려해야 할 몇 가지 주요 사항을 제공합니다. 워크플로 예제를 제공하며 워크플로 단계를 관련 Azure 기술에 매핑합니다. 개념은 고객이 자신의 모바일 디바이스로 촬영했거나 인터넷에 있는 이미지를 활용하여 환경의 의도에 따라 관련 및/또는 유사 항목을 검색할 수 있다는 것을 기반으로 합니다. 따라서 시각적 검색은 텍스트 입력에서 여러 메타데이터 요소가 있는 이미지로 속도를 개선하여 사용 가능한 해당 항목을 빠르게 표시합니다.

## <a name="visual-search-engines"></a>Visual Search 엔진

Visual Search 엔진은 이미지를 입력으로 사용하고 종종 출력으로도 사용하여 정보를 검색합니다.

소매 산업에서는 엔진이 점점 더 보편화되고 있으며, 충분히 그럴 만한 이유가 있습니다.

- 2017년에 게시된 [Emarketer](https://www.emarketer.com/Report/Visual-Commerce-2017-How-Image-Recognition-Augmentation-Changing-Retail/2002059) 보고서에 따르면 인터넷 사용자의 약 75%가 구입하기 전에 제품의 사진이나 동영상을 검색합니다.
- 또한 [Slyce](https://slyce.it/wp-content/uploads/2015/11/Visual_Search_Technology_and_Market.pdf)(시각적 검색 회사) 2015 보고서에 따르면 소비자의 74%가 텍스트 검색을 비효율적이라고 생각합니다.

따라서 [Markets &amp; Markets](https://www.marketsandmarkets.com/PressReleases/image-recognition.asp)의 조사에 따르면 이미지 인식 시장은 2019년까지 250억 달러의 가치를 갖게 될 것입니다.

이 기술은 주요 전자상거래 브랜드에서 이미 채택되었으며, 해당 브랜드에서 기술의 개발에도 상당한 기여를 했습니다. 가장 두드러진 얼리어답터는 다음과 같습니다.

- eBay - 앱의 Image Search 및 “Find It on eBay” 도구(현재 모바일 환경만 제공됨)
- Pinterest - Lens 시각적 검색 도구
- Microsoft - Bing Visual Search

## <a name="adopt-and-adapt"></a>채택 및 적응

다행히, 시각적 검색을 통해 수익을 올리는 데 엄청난 양의 컴퓨팅 능력이 필요하지는 않습니다. 이미지 카탈로그가 있는 비즈니스는 Azure 서비스에 기본 제공된 Microsoft의 AI 전문 지식을 활용할 수 있습니다.

[Bing Visual Search](https://azure.microsoft.com/en-us/services/cognitive-services/bing-visual-search/?WT.mc_id=vsearchgio-article-gmarchet) API는 홈 퍼니싱, 패션, 여러 종류의 제품 등을 식별하는 컨텍스트 정보를 이미지에서 추출하는 방법을 제공합니다.

또한 자체 카탈로그의 시각적으로 유사한 이미지, 관련 쇼핑 소스의 제품, 관련 검색을 반환합니다. 흥미롭긴 하지만 사용자 회사가 이러한 소스 중 하나가 아니라면 이 기능의 사용은 제한적입니다.

Bing은 다음과 같은 기능도 제공합니다.

- 이미지에서 찾은 개체 또는 개념을 탐색할 수 있는 태그.
- 이미지의 관심 영역에 대한 경계 상자(예: 의류, 가구 항목).

해당 정보를 사용하여 검색 공간(및 시간)을 한 회사의 제품 카탈로그로 줄이고 관심 지역 및 범주의 개체 등으로 제한할 수 있습니다.

## <a name="implement-your-own"></a>고유한 시각적 검색 구현

시각적 검색을 구현할 때 고려해야 할 몇 가지 주요 구성 요소가 있습니다.

- 이미지 수집 및 필터링
- 스토리지 및 검색 기술
- 기능화, 인코딩 또는 “해싱”
- 유사성 측정값 또는 거리와 순위

 ![](./assets/visual-search-use-case-overview/visual-search-pipeline.png)

그림 1: Visual Search 파이프라인 예제 

### <a name="sourcing-the-pictures"></a>그림 소싱

그림 카탈로그를 소유하지 않은 경우 fashion [MNIST](https://www.kaggle.com/zalando-research/fashionmnist), deep [fashion](http://mmlab.ie.cuhk.edu.hk/projects/DeepFashion.html) 등의 공개적으로 사용 가능한 데이터 세트에서 알고리즘을 학습해야 할 수 있습니다. 이러한 데이터 세트는 여러 범주의 제품을 포함하며, 일반적으로 이미지 분류 및 검색 알고리즘을 벤치마크하는 데 사용됩니다.

 ![](./assets/visual-search-use-case-overview/deep-fashion-dataset.png)

그림 2: Deep Fashion 데이터 세트 예제 

### <a name="filtering-the-images"></a>이미지 필터링

앞에서 언급한 것과 같은 대부분의 벤치마크 데이터 세트는 이미 사전 처리되었습니다.

고유한 시각적 검색을 빌드하는 경우 최소한 이미지를 동일한 크기로 유지하는 것이 좋습니다. 이미지 크기는 대체로 모델이 학습된 입력에 의해 결정됩니다.

대부분의 경우 이미지의 명도도 정규화하는 것이 좋습니다. 검색의 세부 정보 수준에 따라 색이 중복 정보일 수도 있으므로 흑백으로 줄이면 처리 시간이 단축됩니다.

마지막으로, 이미지 데이터 세트가 나타내는 다양한 클래스에서 균형을 유지해야 합니다.

### <a name="image-database"></a>이미지 데이터베이스

데이터 계층은 아키텍처에서 특히 손상되기 쉬운 구성 요소입니다. 데이터 계층에는 다음이 포함됩니다.

- 이미지
- 이미지에 대한 모든 메타데이터(크기, 태그, 제품 SKU, 설명)
- 기계 학습 모델에서 생성된 데이터(예: 이미지당 4096개 요소의 숫자 벡터)

다양한 소스에서 이미지를 검색하거나 최적 성능을 위해 여러 개의 기계 학습 모델을 사용하는 경우 데이터 구조가 변경됩니다. 따라서 반구조화된 데이터와 고정되지 않은 스키마를 처리할 수 있는 기술 또는 조합을 선택하는 것이 중요합니다.

최소 개수의 유용한 데이터 요소(예: 이미지 식별자 또는 키, 제품 SKU, 설명, 태그 필드)를 요구할 수도 있습니다.

[Azure CosmosDB](https://azure.microsoft.com/en-us/services/cosmos-db/?WT.mc_id=vsearchgio-article-gmarchet)는 Azure CosmosDB에 빌드된 애플리케이션에 대해 필수 유연성과 다양한 액세스 메커니즘을 제공하며, 카탈로그 검색에 도움이 됩니다. 그러나 최상의 가격/성능을 얻을 수 있도록 주의해야 합니다. CosmosDB를 사용하면 문서 첨부 파일을 저장할 수 있지만 계정당 총 한도가 있으며 비용이 많이 들 수 있습니다. 실제 이미지 파일을 Blob에 저장하고 파일 링크를 데이터베이스에 삽입하는 것이 일반적입니다. CosmosDB의 경우 해당 이미지와 연관된 카탈로그 속성(SKU, 태그 등)을 포함하는 문서와 이미지 파일의 URL(예: Azure Blob Storage, OneDrive 등)을 포함하는 첨부 파일을 만들면 됩니다.

 ![](./assets/visual-search-use-case-overview/cosmosdb-data-model.png)

그림 3: CosmosDB 계층적 리소스 모델 

Cosmos DB의 전 세계 배포를 활용하려는 경우 문서 및 첨부 파일을 복제하지만 연결된 파일은 복제하지 않는다는 것에 유의하세요. 연결된 파일에는 콘텐츠 배포 네트워크를 사용하는 것이 좋습니다.

기타 적용 가능한 기술은 Azure SQL Database(고정 스키마가 허용되는 경우) 및 Blob의 조합 또는 저렴하고 빠른 스토리지와 검색을 위한 Azure 테이블 및 Blob의 조합입니다.

### <a name="feature-extraction-amp-encoding"></a>피처 추출 및 인코딩

인코딩 프로세스는 데이터베이스의 그림에서 핵심적인 피처를 추출하고 각 피처를 수천 개의 구성 요소를 포함할 수 있는 스파스 “피처” 벡터(많은 0을 포함하는 벡터)에 매핑합니다. 이 벡터는 코드와 유사하게 그림을 특성화하는 피처(예: 가장자리, 도형)의 숫자 표현입니다.

피처 추출 기술은 일반적으로 _학습 이전 메커니즘_을 사용합니다. 이 메커니즘은 미리 학습된 인공신경망을 선택하고, 인공신경망을 통해 각 이미지를 실행하고, 생성된 피처 벡터를 다시 이미지 데이터베이스에 저장할 때 발생합니다. 이러한 방법으로, 네트워크를 학습한 사람으로부터 학습을 “이전”합니다. Microsoft는 [ResNet50](https://www.kaggle.com/keras/resnet50)과 같은, 이미지 인식 작업에 널리 사용되는 여러 개의 미리 학습된 네트워크를 개발하고 게시했습니다.

인공신경망에 따라 피처 벡터가 다소 길고 스파스하므로 메모리 및 스토리지 요구 사항도 달라집니다.

또한 각 범주마다 다른 네트워크를 적용할 수 있으므로 시각적 검색 구현은 실제로 다양한 크기의 피처 벡터를 생성할 수 있습니다.

미리 학습된 인공신경망은 비교적 쉽게 사용할 수 있지만 이미지 카탈로그에서 학습된 사용자 지정 모델만큼 효율적이지 않을 수 있습니다. 이러한 미리 학습된 인공신경망은 일반적으로 특정 이미지 컬렉션에서 검색하는 대신 벤치마크 데이터 세트의 분류를 위해 설계되었습니다.

검색 공간을 제한하고 메모리 및 스토리지 요구 사항을 줄이는 데 유용한 범주 예측 및 덴스(즉, 더 작고 스파스가 아닌) 벡터를 둘 다 생성하도록 인공신경망을 수정하고 다시 학습하는 것이 좋습니다. 이진 벡터를 사용할 수 있으며, 일반적으로 “[의미 체계 해시](https://www.cs.utoronto.ca/~rsalakhu/papers/semantic_final.pdf)”라고 합니다. 이 용어는 문서 인코딩 및 검색 기술에서 파생되었습니다. 이진 표시는 추가 계산을 간소화합니다.

 ![](./assets/visual-search-use-case-overview/resnet-modifications.png)

그림 4: Visual Search용 ResNet에 대한 수정 사항 - F. Yang et al., 2017 

미리 학습된 모델을 선택하든, 사용자 고유의 모델을 개발하든 간에 모델 자체의 피처화 및/또는 학습을 실행할 위치를 결정해야 합니다.

Azure는 VM, Azure Batch, [Batch AI](https://azure.microsoft.com/en-us/services/batch-ai/?WT.mc_id=vsearchgio-article-gmarchet), Databricks 클러스터 등 여러 가지 옵션을 제공합니다. 그러나 어떤 경우든지 GPU를 사용하면 최상의 가격/성능이 제공됩니다.

또한 Microsoft는 GPU 비용 대비 적은 금액으로 빠른 계산이 가능한 FPGA 출시를 최근에 발표했습니다(프로젝트 [Brainwave](https://www.microsoft.com/en-us/research/blog/microsoft-unveils-project-brainwave/?WT.mc_id=vsearchgio-article-gmarchet)). 그러나 문서가 작성될 당시 이 제공은 특정 네트워크 아키텍처로 제한되었으므로 성능을 자세히 평가해야 합니다.

### <a name="similarity-measure-or-distance"></a>유사성 측정값 또는 거리

이미지가 피처 벡터 공간에 표시될 때 유사성을 찾으려면 해당 공간의 요소 간 거리 측정값을 정의해야 합니다. 거리가 정의되고 나면 유사한 이미지 클러스터를 컴퓨팅하고 유사성 매트릭스를 정의할 수 있습니다. 선택한 거리 메트릭에 따라 결과가 달라질 수 있습니다. 예를 들어 실수 벡터에 대한 가장 일반적인 유클리드 거리 측정값은 거리의 크기를 캡처하므로 이해하기 쉽습니다. 그러나 계산 측면에서는 다소 비효율적입니다.

[코사인](https://en.wikipedia.org/wiki/Cosine_similarity) 거리는 주로 크기가 아닌 벡터의 방향을 캡처하는 데 사용됩니다.

이진 표시에 대한 [해밍](https://en.wikipedia.org/wiki/Hamming_distance) 거리와 같은 대안은 정확도는 다소 떨어지지만 효율성과 속도가 높습니다.

벡터 크기와 거리 측정값의 조합에 따라 검색에 사용되는 계산과 메모리 양이 결정됩니다.

### <a name="search-amp-ranking"></a>검색 및 순위

유사성이 정의되고 나면, 입력으로 전달된 값에 가장 가까운 N개 항목을 검색하고 식별자 목록을 반환하는 효율적인 방법을 고안해야 합니다. 이를 “이미지 순위 지정”이라고도 합니다. 큰 데이터 세트에서는 모든 거리를 계산하는 데 오랜 시간이 걸리므로 근사치 인접 알고리즘을 사용합니다. 해당 알고리즘에 대한 여러 오픈 소스 라이브러리가 있으므로 알고리즘을 처음부터 코딩할 필요는 없습니다.

마지막으로, 메모리 및 계산 요구 사항에 따라 학습된 모델에 대해 선택할 배포 기술과 고가용성이 결정됩니다. 일반적으로 검색 공간은 분할되며, 순위 지정 알고리즘의 여러 인스턴스가 병렬로 실행됩니다. 확장성 및 가용성을 허용하는 한 가지 옵션은 [Azure Kubernetes](https://azure.microsoft.com/en-us/services/container-service/kubernetes/?WT.mc_id=vsearchgio-article-gmarchet) 클러스터입니다. 이 경우 여러 컨테이너(각각 검색 공간의 한 파티션 처리) 및 여러 노드(고가용성에 사용)에 순위 지정 모델을 배포하는 것이 좋습니다.

## <a name="next-steps"></a>다음 단계

시각적 검색 구현이 반드시 복잡한 것은 아닙니다. Microsoft의 AI 연구 및 도구를 활용하는 동시에 Azure 서비스로 고유한 시각적 검색을 빌드하거나 Bing을 사용할 수 있습니다.

### <a name="trial"></a>평가판

- [Visual Search API 테스트 콘솔](https://dev.cognitive.microsoft.com/docs/services/878c38e705b84442845e22c7bff8c9ac)을 사용해 보세요.

### <a name="develop"></a>개발

- 사용자 지정 서비스를 만들기 시작하려면 [Bing Visual Search API 개요](https://docs.microsoft.com/en-us/azure/cognitive-services/bing-visual-search/overview/?WT.mc_id=vsearchgio-article-gmarchet)를 참조하세요.
- 첫 번째 요청을 만들려면 [C#](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/quickstarts/csharp) | [Java](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/quickstarts/java) | [node.js](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/quickstarts/nodejs) | [Python](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/quickstarts/python) 빠른 시작을 참조하세요.
- [Visual Search API 참조](https://aka.ms/bingvisualsearchreferencedoc)를 숙지합니다.

### <a name="background"></a>백그라운드

- [Deep Learning Image Segmentation](https://www.microsoft.com/developerblog/2018/04/18/deep-learning-image-segmentation-for-ecommerce-catalogue-visual-search/?WT.mc_id=vsearchgio-article-gmarchet)(딥 러닝 이미지 분할): Microsoft 문서에서는 이미지를 배경에서 분리하는 프로세스를 설명합니다.
- [Visual Search at Ebay](https://arxiv.org/abs/1706.03154)(Ebay의 Visual Search): Cornell University 연구
- [Visual Discovery at Pinterest](https://arxiv.org/abs/1702.04680)(Pinterest의 Visual Discovery): Cornell University 연구
- [Semantic Hashing](https://www.cs.utoronto.ca/~rsalakhu/papers/semantic_final.pdf)(의미 체계 해싱): University of Toronto 연구

_이 문서는 Giovanni Marchetti 및 Mariya Zorotovich에 의해 작성되었습니다._