---
title: 제조업체를 위한 고성능 Azure 온디맨드 컴퓨팅 서비스
author: ercenk
ms.author: ercenk
ms.date: 11/20/2019
ms.topic: article
ms.service: industry
description: 제조 산업에 필요한 고성능 컴퓨팅에 대한 개요입니다.
ms.openlocfilehash: fe5200a726b2a65efaed2bc7a8de01e97766d425
ms.sourcegitcommit: 3b175d73a82160c4cacec1ce00c6d804a93c765d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77052538"
---
# <a name="on-demand-scalable-high-power-compute"></a>확장성 있는 주문형 고성능 컴퓨팅

## <a name="introduction"></a>소개

고객은 가볍고, 강력하고, 안전하고, 지속 가능하고, 사용자 지정된 특성을 갖춘 제품을 요구하고 있습니다. 결과적으로 설계 단계가 점점 더 복잡해지고 있습니다. 이 단계에서 컴퓨터는 시각화, 분석, 시뮬레이션 및 최적화에 사용됩니다. 그리고 이러한 작업은 더 정교해지고 더 우수한 계산 성능을 요구합니다. 또한 제품이 점점 더 많이 연결되어 처리 및 분석해야 하는 방대한 양의 데이터가 생성되고 있다는 사실도 추가됩니다.

이러한 모든 상황은 하나의 요구 사항, 즉 주문형 방식의 대규모 컴퓨팅 리소스로 귀결됩니다.

이 문서에서는 대규모 컴퓨팅 성능이 필요한 엔지니어링 및 제조 분야에서 잘 알려진 몇 가지 영역을 안내하고 Microsoft Azure 플랫폼에서 지원하는 방식을 살펴봅니다.

## <a name="cloud-design-workstations"></a>클라우드 설계 워크스테이션

제품 디자이너는 제품 개발 수명 주기의 설계 및 계획 단계에서 다양한 소프트웨어 도구를 사용합니다. CAD 도구는 디자이너의 워크스테이션에서 강력한 그래픽 기능이 필요하며, 이러한 워크스테이션의 비용은 높습니다. 이러한 고성능을 갖춘 워크스테이션은 일반적으로 디자이너의 사무실 안에 있으며 물리적으로 한 위치에 연결됩니다.

클라우드 솔루션의 인기가 높아지고 새로운 기능을 사용할 수 있게 되면서 클라우드 워크스테이션에 대한 아이디어가 점점 더 실현되기 시작했습니다. 클라우드에 호스팅되는 워크스테이션을 사용하면 디자이너가 어느 위치에서나 액세스할 수 있습니다. 그리고 조직에서는 이를 통해 비용 모델을 자본 비용에서 운영 비용으로 변경할 수 있습니다.

### <a name="remote-desktop-protocol"></a>원격 데스크톱 프로토콜

Microsoft의 RDP(원격 데스크톱 프로토콜)는 오랫동안 TCP만 지원했습니다. TCP는 UDP보다 더 많은 오버헤드를 발생시킵니다. RDP 8.0부터 UDP는 Microsoft 원격 데스크톱 서비스를 실행하는 서버에서 사용할 수 있습니다. VM(가상 머신)을 사용하려면 CPU, 메모리 및 가장 중요한 GPU(그래픽 처리 장치) 와 같은 하드웨어 리소스가 충분해야 합니다. (GPU는 고성능 클라우드 워크스테이션의 가장 중요 한 구성 요소입니다.) Windows Server 2016에서는 기본 그래픽 기능에 액세스 하기 위한 몇 가지 옵션을 제공 합니다. WARP(Windows Advanced Rasterization Platform)라고도 하는 기본 RDS GPU 솔루션은 지식 근로자 시나리오에 적합한 솔루션이지만, 클라우드 워크스테이션 시나리오에서는 적합하지 않은 리소스를 제공합니다. RemoteFX vGPU는 원격 연결을 위해 도입된 RemoteFX의 기능으로, 서버당 사용자 밀도가 높은 시나리오를 제공하여 높은 버스트 GPU 사용률을 허용합니다. 그러나 GPU 전원을 완전히 사용하려면 DDA(불연속 디바이스 할당)가 필요합니다.

NV 시리즈 VM은 Azure N 시리즈 제품의 일부로 단일 또는 다중 NVDIA GPU에 사용할 수 있습니다. 이러한 VM은 OpenGL 및 DirectX와 같은 프레임워크를 사용하여 원격 시각화 및 VDI 시나리오에 최적화되어 있습니다. GPU를 최대 4개까지 늘리면 Azure에서 DDA를 통해 GPU를 완전히 활용하는 워크스테이션을 프로비전할 수 있습니다.

언급할 가치가 있는 매우 중요한 점은 Azure 플랫폼의 프로그래밍 가능성입니다. VM에 대한 몇 가지 옵션을 제공합니다. 예를 들어 필요에 따라 워크스테이션을 프로비전할 수 있습니다. 또한 Premium Storage 또는 Azure Files의 Azure Disks를 통해 로컬 파일에 원격 머신의 상태를 유지할 수 있습니다. 이러한 옵션을 통해 비용을 제어할 수 있습니다. Citrix와 Microsoft의 파트너 관계는 XenDesktop 및 XenApp 솔루션을 위해 데스크톱 가상화 솔루션에 대한 또 다른 대안도 제공합니다.

## <a name="analysis-and-simulation"></a>분석 및 시뮬레이션

컴퓨터의 실제 시스템에 대한 분석과 시뮬레이션은 오랫동안 계속되어 왔습니다. FEA(유한 요소 분석)는 많은 분석 문제를 해결하는 데 사용되는 숫자와 관련된 방법입니다. FEA에는 대규모 행렬 계산을 수행하기 위해 많은 계산 능력이 필요합니다. FEA 모델의 솔루션과 관련된 행렬의 수는 2D에서 3D로 전환하고 FEA 메시에 세분성을 추가함에 따라 기하급수적으로 증가합니다. 이를 위해 필요에 따라 배포되는 컴퓨팅 성능이 필요합니다.
문제 해결 코드를 병렬로 실행하여 리소스 확장성을 활용하는 것이 중요합니다.

시뮬레이션 문제를 해결하려면 대규모 컴퓨팅 리소스가 필요합니다. HPC(고성능 컴퓨팅)는 대규모 컴퓨팅의 한 종류입니다. HPC에는 빠른 병렬 계산을 위한 RDMA(원격 직접 메모리 액세스) 기능을 통한 짧은 백 엔드 네트워크 대기 시간이 필요합니다. Azure 플랫폼은 고성능 컴퓨팅을 위해 구축된 VM을 제공합니다. 이러한 VM은 DDR4 메모리와 쌍으로 연결되는 특수 프로세서를 갖추고 있으며, Linux 및 Windows 설치 모두에서 계산 집약적 솔루션을 효과적으로 실행할 수 있습니다. 그리고 여러 가지 크기로 사용할 수 있습니다. [고성능 컴퓨팅 VM 크기](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-hpc?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json?WT.mc_id=computeinmanufacturing-docs-ercenk)를 참조하세요.
Azure에서 다른 방식으로 HPC를 지원하는 방법을 보려면 [큰 Compute: HPC 및 Batch](https://azure.microsoft.com/solutions/big-compute/?WT.mc_id=computeinmanufacturing-docs-ercenk)를 참조하세요.

Azure 플랫폼을 사용 하면 솔루션을 강화 하 고 확장할 수 있습니다. 시뮬레이션에 대 한 일반적으로 알려진 소프트웨어 패키지 중 하나는 CD cd-adapco에서 별 CCM +입니다. "Le Mans 1억 개 셀" CFD(전산 유체 역학) 모델을 실행하는 STAR-CCM+를 시연한 [연구 발표](https://azure.microsoft.com/blog/availability-of-star-ccm-on-microsoft-azure/?WT.mc_id=computeinmanufacturing-docs-ercenk)에서 플랫폼의 확장성을 엿볼 수 있습니다. 다음 차트에서는 시뮬레이션을 실행할 때 더 많은 코어가 추가되면서 관찰된 확장성을 보여 줍니다.

![https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/34f1873b-4db5-4c62-b963-8bdf3966cf60.png](assets/bigcompute-assets/starccm.png)

또 다른 인기 있는 엔지니어링 분석 소프트웨어 패키지는 ANSYS CFD입니다. 엔지니어는 이 패키지를 통해 유체력, 열 효과, 구조적 무결성 및 전자기 방사선을 포함한 다중 물리 분석을 수행할 수 있습니다. [연구 발표](https://azure.microsoft.com/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/?WT.mc_id=computeinmanufacturing-docs-ercenk)에서는 Azure의 솔루션 확장성을 시연했으며, 이와 비슷한 결과를 보여 줍니다.

![https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/77129585-f25c-4c29-b22b-80c627d03daa.png](assets/bigcompute-assets/fluent.png)

로컬 컴퓨팅 클러스터에 투자하는 대신 병렬 실행이 필요한 소프트웨어 패키지는 모든 클라우드 솔루션에 대해 [HPC 및 GPU VM](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview?WT.mc_id=computeinmanufacturing-docs-ercenk) 제품군을 사용하여 Azure 가상 머신 또는 [VMSS(가상 머신 확장 집합)](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-hpc?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json?WT.mc_id=computeinmanufacturing-docs-ercenk)에 배포할 수 있습니다.

### <a name="burst-to-azure"></a>Azure로 버스트

로컬 클러스터를 사용할 수 있는 경우 Azure로 확장한 다음, 최대 워크로드를 Azure로 오프로드하는 옵션(즉, Azure로 버스트)도 있습니다. 이렇게 하려면 Azure를 지원하는 온-프레미스 워크로드 관리자(예: [Alces Flight Compute](https://azuremarketplace.microsoft.com/marketplace/apps/alces-flight-limited.alces-flight-compute-solo?tab=Overview?WT.mc_id=computeinmanufacturing-docs-ercenk), [TIBCO DataSynapse GridServer](https://azure.microsoft.com/blog/tibco-datasynapse-comes-to-the-azure-marketplace/?WT.mc_id=computeinmanufacturing-docs-ercenk), [Bright Cluster Manager](http://www.brightcomputing.com/technology-partners/microsoft), [IBM Spectrum Symphony 및 Symphony LSF](https://azure.microsoft.com/blog/ibm-and-microsoft-azure-support-spectrum-symphony-and-spectrum-lsf/?WT.mc_id=computeinmanufacturing-docs-ercenk), [PBS Pro](http://pbspro.org/), [Microsoft HPC Pack](https://technet.microsoft.com/library/mt744885.aspx?WT.mc_id=computeinmanufacturing-docs-ercenk)) 중 하나를 사용해야 합니다.

다른 옵션으로, 대규모 병렬 및 HPC 일괄 처리 작업을 효율적으로 실행하는 서비스인 Azure Batch가 있습니다. Azure Batch는 MPI(메시지 전달 인터페이스) API를 사용하는 작업을 허용합니다. Batch는 [HPC](https://msdn.microsoft.com/library/bb524831(v=vs.85).aspx?WT.mc_id=computeinmanufacturing-docs-ercenk) 및 [GPU](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-hpc?WT.mc_id=computeinmanufacturing-docs-ercenk) 최적화 VM 제품군을 사용하는 [Microsoft MPI](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu?WT.mc_id=computeinmanufacturing-docs-ercenk) 및 Intel MPI를 모두 지원합니다. 또한 Microsoft는 [Cycle Computing](https://blogs.microsoft.com/blog/2017/08/15/microsoft-acquires-cycle-computing-accelerate-big-computing-cloud/?WT.mc_id=computeinmanufacturing-docs-ercenk)을 인수하여 Azure에서 클러스터를 실행하는 데 더 높은 수준의 추상화를 제공합니다. 또 다른 옵션으로, Azure에서 [Cray 슈퍼 컴퓨터](https://www.cray.com/solutions/supercomputing-as-a-service/cray-in-azure)를 실행하여 [Azure Storage](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage?WT.mc_id=computeinmanufacturing-docs-ercenk) 및 [Azure Data Lake](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview?WT.mc_id=computeinmanufacturing-docs-ercenk)와 같은 보조 Azure 서비스에 원활하게 액세스하는 것입니다.

## <a name="generative-design"></a>생성 설계

설계 프로세스는 항상 반복적인 프로세스입니다. 디자이너는 대상 설계에 대한 일단의 제약 조건과 매개 변수로 시작하여 여러 설계 대안을 반복하며, 최종적으로 제약 조건을 충족하는 설계 대안을 결정합니다.
그러나 계산 능력이 사실상 무한하다면 몇 가지 설계 대안들을 평가하는 대신 *수천* 또는 심지어 *수백만* 개의 설계 대안들을 평가할 수 있습니다. 이 과정은 파라메트릭 모델에서 시작하여 CAD 도구에서 사용하기 시작했습니다. 이제 업계에서 클라우드 플랫폼에 대한 방대한 계산 리소스를 사용하여 다음 단계로 진행합니다. 생성 설계는 소프트웨어 도구에 매개 변수 및 제약 조건을 제공하는 설계 프로세스를 설명하는 용어입니다. 그런 다음, 이 도구는 설계 대안을 생성하여 솔루션의 몇 가지 순열을 만듭니다.
생성 설계에는 토폴로지 최적화, 격자 최적화, 표면 최적화 및 양식 합성과 같은 몇 가지 방법이 있습니다. 이러한 방법에 대한 자세한 내용은 이 문서의 범위를 벗어납니다. 그러나 이러한 방법 전반에 걸친 공통적인 패턴은 계산 집약적 환경에 액세스해야 한다는 것입니다.

생성 설계의 시작점은 알고리즘에서 반복해야 하는 설계 매개 변수를 적절한 증가 및 값 범위와 함께 정의하는 것입니다. 그런 다음, 알고리즘에서 이러한 매개 변수의 유효한 조합 각각에 대한 설계 대안을 만듭니다. 이렇게 하면 수많은 설계 대안이 만들어집니다. 이러한 대안을 만들려면 많은 컴퓨팅 리소스가 필요합니다.
또한 각 설계 대안에 대해 모든 시뮬레이션 및 분석 작업을 실행해야 합니다. 결과적으로 대규모 컴퓨팅 환경이 필요합니다.

[Azure Batch](https://docs.microsoft.com/azure/batch/batch-technical-overview?WT.mc_id=computeinmanufacturing-docs-ercenk) 및 [VMSS](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview?WT.mc_id=computeinmanufacturing-docs-ercenk)를 통해 컴퓨팅 요구 사항에 따라 필요할 때 강화할 수 있는 Azure의 여러 옵션은 이러한 워크로드에 대한 자연스러운 대상입니다.

## <a name="machine-learning-ml"></a>ML(기계 학습)

매우 간단한 수준에서는 ML 시스템을 일반화할 수 있습니다. 즉, 데이터 요소 또는 데이터 요소 세트가 제공되면 시스템에서 상관 관계가 지정된 결과를 반환합니다. 이와 같이 ML 시스템은 다음과 같은 질문을 해결하는 데 사용됩니다.

- 과거 주택 가격과 주택의 속성이 제공되면 특정 주택에 예상되는 시장 거래 가격은 어떻게 되나요?

- 다양한 센서의 판독값과 머신의 과거 오류 사례가 제공되면 다음 기간에 머신에서 오류가 발생할 가능성은 어떻게 되나요?

- 일단의 이미지가 제공되면 어느 것이 집고양이일까요?

- 석유 파이프라인의 비디오 피드가 제공되면 손상된 부분이 심하게 패인 부분인가요?

AI(인공 지능)과 ML(기계 학습)을 사용하는 고급 분석 기능을 추가하는 것은 다음과 비슷한 프로세스를 통해 모델을 개발하는 것으로 시작합니다.

![](assets/bigcompute-assets/aipipeline.png)

[알고리즘 선택](https://docs.microsoft.com/azure/machine-learning/studio/algorithm-choice?WT.mc_id=computeinmanufacturing-docs-ercenk)은 데이터 크기, 품질 및 특성뿐만 아니라 예상되는 답변의 형식에 따라서도 달라집니다. 입력 크기, 선택한 알고리즘 및 컴퓨팅 환경에 따라 이 단계에는 일반적으로 큰 계산 집약적 리소스가 필요하며 이를 완료하는 데 여러 시간이 걸릴 수 있습니다. 다음 차트는 ML 알고리즘의 학습을 벤치마킹하기 위한 [기술 문서](https://blogs.technet.microsoft.com/machinelearning/2017/07/25/lessons-learned-benchmarking-fast-machine-learning-algorithms/?WT.mc_id=computeinmanufacturing-docs-ercenk)의 차트입니다. 여기서는 다양한 알고리즘, 데이터 세트 크기 및 계산 대상(GPU 또는 CPU)에 따라 학습 주기를 완료하는 데 걸리는 시간을 보여 줍니다.

![](assets/bigcompute-assets/vmsizes.png)

결정의 주요 동인은 비즈니스 문제입니다. 문제로 인해 대규모 데이터 세트를 적절한 알고리즘으로 처리해야 하는 경우 중요한 요소는 알고리즘을 학습하기 위한 클라우드 규모의 컴퓨팅 리소스입니다.
[Azure Batch AI](https://azure.microsoft.com/services/batch-ai/?WT.mc_id=computeinmanufacturing-docs-ercenk)는 AI 모델을 대규모로 병렬로 학습하는 서비스입니다.

Azure Batch AI를 사용하면 데이터 과학자는 [Azure DSVM(Data Science Virtual Machine)](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview?WT.mc_id=computeinmanufacturing-docs-ercenk) 또는 [Azure DLVM(Deep Learning Virtual Machine)](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/deep-learning-dsvm-overview?WT.mc_id=computeinmanufacturing-docs-ercenk)을 사용하여 워크스테이션에서 솔루션을 개발하고 학습을 클러스터로 푸시할 수 있습니다. DSVM 및 DLVM은 사전 설치된 일단의 다양한 도구와 샘플이 있는 특별히 구성된 VM 이미지입니다.

![](assets/bigcompute-assets/azurebatchai.png)

## <a name="conclusion"></a>결론

제조 산업에서는 GPU(그래픽 처리 장치)를 포함한 고급 하드웨어 구성 요소에 대한 액세스를 통해 대량의 수학적 계산을 수행해야 합니다. 리소스를 호스팅하는 플랫폼의 확장성과 탄력성은 매우 중요합니다. 비용을 제어하려면 최적의 속도를 제공하면서 필요에 따라 비용을 제어할 수 있어야 합니다.

Microsoft Azure 플랫폼은 이러한 요구 사항을 충족하기 위한 다양한 옵션을 제공합니다. 처음부터 시작하고, 모든 리소스와 관련 측면을 제어하여 고유한 솔루션을 구축할 수 있습니다. 또는 솔루션을 빠르게 만들 수 있도록 하는 Microsoft 파트너를 찾을 수 있습니다. 파트너는 Azure의 기능을 활용하는 방법을 알고 있습니다.

## <a name="next-steps"></a>다음 단계

- [NV 시리즈 VM](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu?WT.mc_id=computeinmanufacturing-docs-ercenk)을 배포하여 클라우드 워크스테이션을 설정합니다.

- 설계 요구 사항에 맞는 도구를 배포하는 [옵션](https://docs.microsoft.com/azure/virtual-machines/linux/high-performance-computing?WT.mc_id=computeinmanufacturing-docs-ercenk)을 검토하여 Azure HPC 기능을 활용합니다.

- [Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/?WT.mc_id=computeinmanufacturing-docs-ercenk)을 통해 가능성을 알아봅니다.
