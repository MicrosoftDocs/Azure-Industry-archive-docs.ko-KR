---
title: IoT 데이터에서 작업 가능한 인사이트 추출 개요
description: Azure 서비스를 사용하여 IoT 데이터에서 인사이트를 추출합니다. 솔루션 가이드에 대한 개요.
author: ercenk
ms.author: ercenk
manager: gmarchet
ms.service: industry
ms.topic: article
ms.date: 09/26/2018
ms.openlocfilehash: 9528983a06602bd469a4d651bda8478aa758152b
ms.sourcegitcommit: 76f2862adbec59311b5888e043a120f89dc862af
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "51654120"
---
# <a name="extracting-actionable-insights-from-iot-data"></a><span data-ttu-id="748dc-104">IoT 데이터에서 작업 가능한 인사이트 추출</span><span class="sxs-lookup"><span data-stu-id="748dc-104">Extracting actionable insights from IoT data</span></span>

<span data-ttu-id="748dc-105">제조업체는 디지털 도구를 사용하여 프로덕션 및 프로세스를 개선하기 위해 최신 생산 현장을 고려하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="748dc-105">Manufacturers are looking at a modern factory floor with an eye to improving production and processes using digital tools.</span></span> <span data-ttu-id="748dc-106">그리고 가장 큰 공통 분모는 IoT(사물 인터넷)입니다.</span><span class="sxs-lookup"><span data-stu-id="748dc-106">And the greatest common denominator is the Internet of Things (IoT).</span></span> <span data-ttu-id="748dc-107">지금까지 한동안 머신은 데이터를 내보내지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="748dc-107">Machines have been emitting data for some time now.</span></span> <span data-ttu-id="748dc-108">새 머신은 분명히 더 많은 데이터를 제공할 것입니다.</span><span class="sxs-lookup"><span data-stu-id="748dc-108">New machines will doubtlessly be providing even more data.</span></span>
<span data-ttu-id="748dc-109">그러나 데이터를 보유하는 것은 첫 번째 단계일 뿐입니다.</span><span class="sxs-lookup"><span data-stu-id="748dc-109">But having the data is just the first step.</span></span> <span data-ttu-id="748dc-110">솔루션 가이드는 예측 유지 관리 등의 작업을 위해 데이터를 사용하는 경로를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="748dc-110">Our solution guide provides a path to using the data for actions such as predictive maintenance.</span></span>

<span data-ttu-id="748dc-111">간단히 설명하면, 솔루션은 세 가지 주요 단계인 사물(데이터), 인사이트(고급 분석을 통해) 및 작업에 중점을 둡니다.</span><span class="sxs-lookup"><span data-stu-id="748dc-111">In brief, the solution focuses on three major steps: things (data), insights (via advanced analytics) and actions.</span></span>

![사물, 인사이트, 작업 순으로 진행됩니다.](assets/extracting-insights-from-iot/things-insights-actions.png)

<span data-ttu-id="748dc-113">데이터를 보유하는 것도 중요하지만, 데이터는 원자재일 뿐입니다.</span><span class="sxs-lookup"><span data-stu-id="748dc-113">Having data is great, but data is just raw materials.</span></span> <span data-ttu-id="748dc-114">분석을 통한 인사이트가 작업을 안내합니다.</span><span class="sxs-lookup"><span data-stu-id="748dc-114">Insights through analysis guides your actions.</span></span>

![람다 아키텍처](assets/extracting-insights-from-iot/lambda-architecture.png)

<span data-ttu-id="748dc-116">이 아키텍처는 유연하며, 높은 처리량으로 데이터를 수집 및 분석할 수 있게 해줍니다.</span><span class="sxs-lookup"><span data-stu-id="748dc-116">The architecture is flexible and lets you ingest and analyze data at high throughput.</span></span> <span data-ttu-id="748dc-117">요구가 증가함에 따라 구성 요소를 확장 및 축소하여 수요를 충족하거나 비용을 절약할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="748dc-117">As your needs grow, the components can also scale up and down to meet demand or save cost.</span></span> <span data-ttu-id="748dc-118">가이드는 Azure Event Hubs를 사용한 수집, Power BI를 분석 클라이언트로 사용 등 아키텍처를 구현하는 권장 사항도 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="748dc-118">The guide also shows you recommendations for implementing the architecture—for example, using Azure Event Hubs to ingest, and Power BI for an analytics client.</span></span>

[<span data-ttu-id="748dc-119">IoT 데이터에서 작업 가능한 인사이트 추출</span><span class="sxs-lookup"><span data-stu-id="748dc-119">Extracting actionable insights from IoT data</span></span>](./extracting-insights-from-iot-data.md)
