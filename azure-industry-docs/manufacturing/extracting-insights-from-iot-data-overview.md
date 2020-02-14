---
title: IoT 데이터에서 작업 가능한 인사이트 추출 요약
description: Azure 서비스를 사용하여 IoT 데이터에서 인사이트를 추출합니다. 솔루션 가이드에 대한 개요.
author: ercenk
ms.author: ercenk
manager: gmarchet
ms.service: industry
ms.topic: article
ms.date: 11/28/2019
ms.openlocfilehash: 8ca31501d976e0b162735b7f55e2db7fae850776
ms.sourcegitcommit: 3b175d73a82160c4cacec1ce00c6d804a93c765d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77052368"
---
# <a name="extracting-actionable-insights-from-iot-data"></a>IoT 데이터에서 작업 가능한 인사이트 추출

제조업체는 디지털 도구를 사용하여 프로덕션 및 프로세스를 개선하기 위해 최신 생산 현장을 고려하고 있습니다. 그리고 가장 큰 공통 분모는 IoT(사물 인터넷)입니다. 지금까지 한동안 머신은 데이터를 내보내지 않았습니다. 새 머신은 분명히 더 많은 데이터를 제공할 것입니다.
그러나 데이터를 보유하는 것은 첫 번째 단계일 뿐입니다. 솔루션 가이드는 예측 유지 관리 등의 작업을 위해 데이터를 사용하는 경로를 제공합니다.

간단히 설명하면, 솔루션은 세 가지 주요 단계인 사물(데이터), 인사이트(고급 분석을 통해) 및 작업에 중점을 둡니다.

![사물, 인사이트, 작업 순으로 진행됩니다.](assets/extracting-insights-from-iot/things-insights-actions.png)

데이터를 보유하는 것도 중요하지만, 데이터는 원자재일 뿐입니다. 분석을 통한 인사이트가 작업을 안내합니다.

![람다 아키텍처](assets/extracting-insights-from-iot/lambda-architecture.png)

이 아키텍처는 유연하며, 높은 처리량으로 데이터를 수집 및 분석할 수 있게 해줍니다. 요구가 증가함에 따라 구성 요소를 확장 및 축소하여 수요를 충족하거나 비용을 절약할 수도 있습니다. 가이드는 Azure Event Hubs를 사용한 수집, Power BI를 분석 클라이언트로 사용 등 아키텍처를 구현하는 권장 사항도 보여 줍니다.

[IoT 데이터에서 작업 가능한 인사이트 추출](./extracting-insights-from-iot-data.md)
