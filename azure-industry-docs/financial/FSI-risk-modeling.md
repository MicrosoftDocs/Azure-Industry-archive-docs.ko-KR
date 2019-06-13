---
ms.openlocfilehash: cff221055e76d7334793782d19eadd0960712a1f
ms.sourcegitcommit: 461c520509d53bae1021eebf9733a98edbf71e4d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66716843"
---
# <a name="enabling-the-financial-services-risk-lifecycle-with-azure-and-r"></a>Azure 및 R을 통한 금융 서비스 위험 수명 주기 사용


위험 계산은 주요 금융 서비스 운영 수명 주기의 여러 단계에서 중심적인 역할을 합니다. 예를 들어 단순화된 형태의 보험 제품 관리 수명 주기는 아래 다이어그램과 같을 수 있습니다. 위험 계산 측면은 파란색 텍스트로 표시됩니다.

![](./assets/fsi-risk-modeling/image1.png)

자본 시장 회사의 시나리오는 다음과 같을 수 있습니다.

![](./assets/fsi-risk-modeling/image2.png)

이러한 프로세스를 통해 위험 모델링과 관련된 일반적인 요구 사항은 다음과 같습니다.

1.  위험 분석가, 즉 보험 회사의 보험 회계사 또는 자본 시장 회사의 금융 시장 분석가의 임시 위험 관련 실험에 대한 요구 사항.
    이러한 분석가는 일반적으로 자신의 도메인에서 인기 있는 다음과 같은 코드와 모델링 도구를 사용합니다. R 및 Python. 많은 대학 교육 과정의 수학 재무 및 MBA 과정에는 R 또는 Python 학습이 포함됩니다.
    두 언어는 모두 인기 있는 위험 계산을 지원하는 광범위한 오픈 소스 라이브러리를 제공합니다. 분석가에게는 적절한 도구와 함께 다음에 대한 액세스 권한이 필요한 경우가 많습니다.

    a.  정확한 시장 가격 데이터

    b.  기존 정책 및 청구 데이터

    다.  기존 시장 위치 데이터

    d.  기타 외부 데이터. 자료에는 사망률표와 경쟁력 있는 가격 데이터와 같은 구조화된 데이터가 포함됩니다. 날씨, 뉴스 및 기타 자료와 같이 덜 전통적인 자료도 사용할 수 있습니다.

    e.  빠른 대화형 데이터 조사를 가능하게 하는 계산 용량

2.  가격 책정 또는 시장 전략 결정에 임시 기계 학습 알고리즘을 사용할 수도 있습니다.

3.  제품 계획, 거래 전략 및 이와 비슷한 논의에 사용할 데이터의 시각화 및 제시에 대한 요구 사항

4.  분석가가 가격, 가치 평가 및 시장 위험에 대해 구성하여 정의한 모델의 빠른 실행. 가치 평가는 전용 위험 모델링, 시장 위험 도구 및 사용자 지정 코드의 조합을 사용합니다. 분석은 워크로드에서 급증하는 다양한 매일 밤, 매주, 매월, 분기별 및 연간 계산을 통해 일괄적으로 실행됩니다.

5.  통합 위험 보고를 위해 다른 엔터프라이즈 규모의 위험 측정값을 통한 데이터 통합. 더 큰 조직에서는 낮은 수준의 위험 추정치를 엔터프라이즈 규모 위험 모델링 및 보고 도구로 전송할 수 있습니다.

6.  결과는 투자자 및 규제 요구 사항을 충족시키기 위해 필요한 간격으로 정의된 형식으로 보고해야 합니다.

Microsoft는 [Azure Marketplace](https://azuremarketplace.microsoft.com/?WT.mc_id=fsiriskmodelr-docs-scseely)의 Azure 서비스와 파트너 제안을 결합하여 위의 문제를 지원합니다. 이 문서에서는 R을 사용하여 임시 실험을 수행하는 방법에 대한 실제 예제를 보여 줍니다. 먼저 단일 머신에서 실험을 실행하는 방법을 설명한 다음, [Azure Batch](https://docs.microsoft.com/azure/batch/?WT.mc_id=fsiriskmodelr-docs-scseely)에서 동일한 실험을 실행하는 방법을 보여 주고, 모델링에서 외부 서비스를 활용하는 방법을 보여 줌으로써 마무리합니다. Azure에서 정의된 모델 실행에 대한 옵션과 고려 사항은 [은행](https://docs.microsoft.com/azure/industry/financial/risk-grid-banking-solution-guide?WT.mc_id=fsiriskmodelr-docs-scseely) 및 [보험](https://docs.microsoft.com/azure/industry/financial/actuarial-risk-analysis-and-financial-modeling-solution-guide?WT.mc_id=fsiriskmodelr-docs-scseely)에 초점을 맞춘 문서에서 설명하고 있습니다.

## <a name="analyst-modelling-in-r"></a>R의 분석가 모델링 

먼저 간편하고 대표적인 자본 시장 시나리오에서 분석가가 R을 사용하는 방법을 살펴보겠습니다. 계산을 위해 기존 R 라이브러리를 참조하거나 처음부터 코드를 작성하여 이를 빌드할 수 있습니다. 이 예제에서는 가격 책정 데이터도 가져와야 합니다. 예제를 간단하지만 설명적으로 유지하기 위해 주식 선물 계약의 PFE(잠재적 미래 위험 노출)를 계산합니다.
이 예제는 복잡한 파생 상품과 같은 증권에 대한 복잡한 정량적 모델링 기법을 방지하고 위험 수명 주기에 집중하는 단일 위험 요소에 중점을 둡니다. 이 예제에서 수행하는 작업은 다음과 같습니다.

1.  관심 있는 증권을 선택합니다.

2.  증권에 대한 과거 가격을 제공합니다.

3.  다음과 같이 GBM(기하학적 브라운 운동)을 사용하는 단순 MC(몬테카를로) 계산을 통해 주식 가격을 모델링합니다.

    a.  μ(뮤) 기대 수익률 및 σ(세타) 변동성을 추정합니다.

    b.  모델을 과거 데이터로 보정합니다.

4.  다양한 경로를 시각화하여 결과를 전달합니다.

5.  max(0,주식 가치)를 도표로 구성하여 PFE의 의미, 즉 VaR(Value at Risk, 최대 손실 금액)에 대한 차이를 보여 줍니다.

    a.  명확히 하기: PFE = 주가(T) - 선물 계약 가격 K

6.  0\.95 변위치(Quantile)를 사용하여 시뮬레이션 기간 동안 각 시간 단계 및 종료의 PFE 값을 얻습니다.

MSFT 주식에 기반하여 주식 선물에 대한 PFE(잠재적 미래 위험 노출)를 계산할 것입니다. 위에서 설명한 대로 주식 가격을 모델링하려면 모델을 과거 데이터로 보정할 수 있도록 MSFT 주식에 대한 과거 가격이 필요합니다. 과거 주가를 얻는 여러 가지 방법이 있습니다. 이 예제에서는 [Quandl](https://www.quandl.com/) 외부 서비스 공급자의 주가 서비스 무료 버전을 사용합니다.


> 참고: 이 예제에서는 개념을 학습하는 데 사용할 수 있는 [WIKI 가격 데이터 세트](https://www.quandl.com/databases/WIKIP)를 사용합니다. Quandl은 미국 기반 주식을 프로덕션에 사용하기 위해 [End of Day US Stock Prices(종가 기준 미국 주가) 데이터 세트](https://www.quandl.com/data/EOD-End-of-Day-US-Stock-Prices)를 사용하도록 권장합니다.

데이터를 처리하고 주식과 관련된 위험을 정의하려면 다음 작업을 수행해야 합니다.

1.  주식의 과거 데이터를 검색합니다.

2.  과거 데이터에서 μ 기대 수익률 및 σ 변동성을 결정합니다.

3.  몇 가지 시뮬레이션을 사용하여 기초 주가를 모델링합니다.

4.  모델을 실행합니다.

5.  선물 주식의 미래 위험 노출을 결정합니다.

Quandl 서비스에서 주식을 검색하고 지난 180일 동안의 종가 기록을 도표에 표시하는 것으로 시작합니다.

````R
# Lubridate package must be installed
if (!require(lubridate)) install.packages('lubridate')
library(lubridate)

# Quandl package must be installed
if (!require(Quandl)) install.packages('Quandl')
library(Quandl)

# Get your API key from quandl.com
quandl_api = "enter your key here"

# Add the key to the Quandl keychain
Quandl.api_key(quandl_api)

quandl_get <-
    function(sym, start_date = "2018-01-01") {
        require(devtools)
        require(Quandl)
        # Retrieve the Open, High, Low, Close and Volume Column for a given Symbol
        # Column Indices can be deduced from this sample call
        # data <- Quandl(c("WIKI/MSFT"), rows = 1)

        tryCatch(Quandl(c(
        paste0("WIKI/", sym, ".8"),    # Column 8 : Open
        paste0("WIKI/", sym, ".9"),    # Column 9 : High
        paste0("WIKI/", sym, ".10"),   # Column 10: Low
        paste0("WIKI/", sym, ".11"),   # Column 11: Close
        paste0("WIKI/", sym, ".12")),  # Column 12: Volume
        start_date = start_date,
        type = "raw"
        ))
    }

# Define the Equity Forward
instrument.name <- "MSFT"
instrument.premium <- 100.1
instrument.pfeQuantile <- 0.95

# Get the stock price for the last 180 days
instrument.startDate <- today() - days(180)

# Get the quotes for an equity and transform them to a data frame
df_instrument.timeSeries <- quandl_get(instrument.name,start_date = instrument.startDate)

#Rename the columns
colnames(df_instrument.timeSeries) <- c()
colnames(df_instrument.timeSeries) <- c("Date","Open","High","Low","Close","Volume")

# Plot the closing price history to get a better feeling for the data
plot(df_instrument.timeSeries$Date, df_instrument.timeSeries$Close)
````

데이터를 직접 사용하여 GBM 모델을 보정합니다.

````R
# Code inspired by the book Computational Finance, An Introductory Course with R by 
#    A. Arratia.

# Calculate the daily return in order to estimate sigma and mu in the Wiener Process
df_instrument.dailyReturns <- c(diff(log(df_instrument.timeSeries$Close)), NA)

# Estimate the mean of std deviation of the log returns to estimate the parameters of the Wiener Process

estimateGBM_Parameters <- function(logReturns,dt = 1/252) {

    # Volatility
    sigma_hat = sqrt(var(logReturns)) / sqrt(dt)

    # Drift
    mu_hat = mean(logReturns) / dt + sigma_hat**2 / 2.0

    # Return the parameters
    parameter.list <- list("mu" = mu_hat,"sigma" = sigma_hat)

    return(parameter.list)
}

# Calibrate the model to historic data
GBM_Parameters <- estimateGBM_Parameters(df_instrument.dailyReturns[1:length(df_instrument.dailyReturns) - 1])
````

다음으로, 기초 주가를 모델링합니다. 불연속 GBM 프로세스를 처음부터 구현하거나 이 기능을 제공하는 여러 R 패키지 중 하나를 활용할 수 있습니다. 이 문제를 해결하는 방법을 제공하는 [*sde* R 패키지(확률적 미분 방정식에 대한 시뮬레이션 및 유추)](https://cran.r-project.org/web/packages/sde/index.html)를 사용합니다. GBM 방법에는 과거 데이터로 보정되거나 시뮬레이션 매개 변수로 제공되는 매개 변수 세트가 필요합니다. 시뮬레이션을 시작할 때(P0) μ, σ 및 주가를 제공하는 과거 데이터를 사용합니다.

````R
if (!require(sde)) install.packages('sde')
library(sde)

sigma <-  GBM_Parameters$sigma
mu <- GBM_Parameters$mu
P0 <- tail(df_instrument.timeSeries$Close, 1)

# Calculate the PFE looking one month into the future
T <- 1 / 12

# Consider nt MC paths
nt=50

# Divide the time interval T into n discrete time steps
n = 2 ^ 8 

dt <- T / n
t <- seq(0,T,by=dt)
````

이제 몬테카를로 시뮬레이션을 시작하여 몇 가지 시뮬레이션 경로에 대한 잠재적 위험 노출을 모델링할 준비가 되었습니다. 시뮬레이션은 50개의 몬테카를로 경로와 256개의 시간 단계로 제한됩니다. 시뮬레이션을 확장하고 R의 병렬화를 활용하도록 준비하기 위해 몬테카를로 시뮬레이션 루프에서 foreach 문을 사용합니다.

````R
# Track the start time of the simulation
start_s <- Sys.time()

# Instead of a simple for loop to execute a simulation per MC path, call the
# simulation with the foreach package
# in order to demonstrate the similarity to the AzureBatch way to call the method.

library(foreach)
# Execute the MC simulation for the wiener process utilizing the GBM method from the sde package
exposure_mc <- foreach (i=1:nt, .combine = rbind ) %do%  GBM(x = P0, r = mu, sigma = sigma, T = T, N = n)
rownames(exposure_mc) <- c()

# Track the end time of the simulation
end_s <- Sys.time()

# Duration of the simulation

difftime(end_s, start_s) 
````

이제 MSFT 주식의 기초 가격을 시뮬레이션했습니다. 선물 주식의 위험 노출을 계산하기 위해 발행 차액(premium)을 빼고 위험 노출을 양수 값으로만 제한합니다.

````R
# Calculate the total Exposure as V_i(t) - K, put it to zero for negative exposures
pfe_mc <- pmax(exposure_mc - instrument.premium ,0)

ymax <- max(pfe_mc)
ymin <- min(pfe_mc)
plot(t, pfe_mc[1,], t = 'l', ylim = c(ymin, ymax), col = 1, ylab="Credit Exposure in USD", xlab="time t in Years")
for (i in 2:nt) {
    lines(t, pfe_mc[i,], t = 'l', ylim = c(ymin, ymax), col = i)
}
````

다음 두 그림에서는 시뮬레이션 결과를 보여 줍니다. 첫 번째 그림은 50개 경로의 기초 주가에 대한 몬테카를로 시뮬레이션을 보여 줍니다. 두 번째 그림은 선물 주식의 발행 차액을 빼고 위험 노출을 양수 값으로 제한한 이후의 선물 주식에 대한 기초 신용 위험 노출을 보여 줍니다.


|       |    |
|---    |--- |
| <img src="./assets/fsi-risk-modeling/image3.png" width="400px" alt="Figure 1 - 50 Monte Carlo Paths"/> | <img src="./assets/fsi-risk-modeling/image4.png" width="400px" alt="Figure 2 - Credit Exposure for Equity Forward"/> |
| 그림 1 - 50개 몬테카를로 경로 | 그림 2 - 선물 주식에 대한 신용 위험 노출 |


마지막 단계에서 1월 0.95 변위치 PFE는 다음 코드로 계산됩니다.

````R
# Calculate the PFE at each time step
df_pfe <- cbind(t,apply(pfe_mc,2,quantile,probs = instrument.pfeQuantile ))

resulting in the final PFE plot
plot(df_pfe, t = 'l', ylab = "Potential Future Exposure in USD", xlab = "time t in Years")
````

<img src="./assets/fsi-risk-modeling/image5.png" width="500px" alt="Potential Future Exposure for MSFT Equity Forward" /> 

그림 3 MSFT 선물 주식에 대한 잠재적 미래 위험 노출

## <a name="using-azure-batch-with-r"></a>R에서 Azure Batch 사용 

위에서 설명한 R 솔루션은 Azure Batch에 연결하고 클라우드를 활용하여 위험을 계산할 수 있습니다. 이와 같은 병렬 계산에는 약간의 추가 노력이 필요합니다. [Azure Batch를 사용하여 병렬 R 시뮬레이션 실행](https://docs.microsoft.com/en-us/azure/batch/tutorial-r-doazureparallel?WT.mc_id=fsiriskmodelr-docs-scseely) 자습서에서는 R을 Azure Batch에 연결하는 방법에 대해 자세히 설명하고 있습니다. 아래에는 Azure Batch에 연결하는 프로세스 및 간단한 PFE 계산에서 클라우드에 대한 확장을 활용하는 방법에 대한 코드와 요약이 나와 있습니다.

이 예제에서는 앞에서 설명한 것과 동일한 모델을 다룹니다. 앞에서 살펴본 대로 이 계산은 개인용 컴퓨터에서 실행할 수 있습니다. 몬테카를로 경로의 수를 늘리거나 더 작은 시간 단계를 사용하면 실행 시간이 훨씬 더 길어집니다. 거의 모든 R 코드는 변경되지 않은 채 그대로 유지됩니다. 이 섹션에서는 차이점을 강조합니다.

몬테카를로 시뮬레이션의 각 경로는 Azure에서 실행됩니다. 각 경로에서 다른 경로와 독립적으로 "처치 곤란한 병렬" 계산을 제공하므로 이 작업을 수행할 수 있습니다.

Azure Batch를 사용하려면 먼저 기본 클러스터를 정의하고 코드에서 이를 참조한 후에 해당 클러스터를 계산에 사용합니다. 계산을 실행하기 위해 다음 cluster.json 정의를 사용합니다.

````JavaScript
{
  "name": "myMCPool",
  "vmSize": "Standard_D2_v2",
  "maxTasksPerNode": 4,
  "poolSize": {
    "dedicatedNodes": {
      "min": 1,
      "max": 1
    },
    "lowPriorityNodes": {
      "min": 3,
      "max": 3
    },
    "autoscaleFormula": "QUEUE"
  },
  "containerImage": "rocker/tidyverse:latest",
  "rPackages": {
    "cran": [],
    "github": [],
    "bioconductor": []
  },
  "commandLine": [],
  "subnetId": ""
}
````

다음 R 코드는 이 클러스터 정의를 통해 해당 클러스터를 사용합니다.
````R
# Define the cloud burst environment
library(doAzureParallel)

# set your credentials
setCredentials("credentials.json")

# Create your cluster if not exist
cluster <- makeCluster("cluster.json")

# register your parallel backend
registerDoAzureParallel(cluster)

# check that your workers are up
getDoParWorkers()
````

마지막으로 doAzureParallel 패키지를 사용하도록 이전의 foreach 문을 업데이트합니다. 이는 sde 패키지에 대한 참조를 추가하고 %do%를 %dopar%로 변경하는 사소한 변경입니다.

````R
# Execute the MC simulation for the wiener process utilizing the GBM method from the sde package and extend the computation to the cloud
exposure_mc <- foreach(i = 1:nt, .combine = rbind, .packages = 'sde') %dopar% GBM(x = P0, r = mu, sigma = sigma, T = T, N = n)
rownames(exposure_mc) <- c()
````

각 몬테카를로 시뮬레이션은 Azure Batch에 작업으로 제출됩니다. 이 작업은 클라우드에서 실행됩니다. 결과는 분석가 워크벤치로 다시 보내기 전에 병합됩니다. 과도한 리프팅 및 계산은 클라우드에서 실행되어 요청된 계산에 필요한 크기 조정 및 기본 인프라를 최대한 활용합니다.

계산이 완료되면 다음 단일 명령을 호출하여 추가 리소스를 쉽게 종료할 수 있습니다.

````R
# Stop the Cloud cluster
stopCluster(cluster)
````


## <a name="use-a-saas-offering"></a>SaaS 제안 사용

앞의 두 예제에서는 적절한 가치 평가 모델을 개발하기 위해 로컬 및 클라우드 인프라를 활용하는 방법을 보여 줍니다. 이 패러다임은 변화되기 시작했습니다. 온-프레미스 인프라가 클라우드 기반 IaaS 및 PaaS 서비스로 전환되는 것처럼 관련 위험 수치의 모델링이 서비스 지향 프로세스로 전환되고 있습니다.
오늘날의 분석가들은 다음 두 가지 주요 문제에 직면해 있습니다.

1.  규정 요구 사항은 모델링 요구 사항에 추가할 계산 용량을 늘립니다. 규정자는 더 빈번하고 최신의 위험 수치를 요구하고 있습니다.

2.  기존의 위험 인프라는 시간이 지남에 따라 유기적으로 성장해 왔으며, 새로운 요구 사항 및 더 향상된 고급 위험 모델링을 민첩한 방식으로 구현할 때 문제가 발생합니다.

클라우드 기반 서비스는 필요한 기능을 제공하고 위험 분석을 지원할 수 있습니다. 이러한 방법이 제공하는 몇 가지 이점은 다음과 같습니다.

-   규정자가 요구하는 가장 일반적인 위험 계산은 규정에 해당하는 모든 사용자가 구현해야 합니다. 분석가는 전문 서비스 공급자의 서비스를 활용하여 즉시 사용할 수 있는 규정자 준수 위험 계산의 이점을 누릴 수 있습니다. 이러한 서비스에는 시장 위험 계산, 거래 상대방 위험 계산, XVA(X-Value Adjustment, X 값 조정) 및 심지어 FRTB(Fundamental Review of Trading Book, 거래 계정에 대한 근본적 재검토) 계산도 포함될 수 있습니다.

-   이러한 서비스는 웹 서비스를 통해 해당 인터페이스를 표시합니다. 기존의 위험 인프라는 이러한 다른 서비스를 통해 향상될 수 있습니다.

이 예제에서는 FRTB 계산을 위해 클라우드 기반 서비스를 호출하려고 합니다. 다음 중 일부는 [AppSource](https://appsource.microsoft.com/?WT.mc_id=fsiriskmodelr-docs-scseely)에서 찾을 수 있습니다. 이 문서에서는 [벡터 위험](http://www.vectorrisk.com/)의 평가판 옵션을 선택했습니다. 시스템은 계속 수정됩니다. 이번에는 관심 있는 위험 수치를 계산하기 위해 서비스를 사용합니다. 이 프로세스는 다음 단계로 구성됩니다.

1.  적절한 매개 변수를 사용하여 관련 위험 서비스를 호출합니다.

2.  서비스에서 계산이 완료될 때까지 기다립니다.

3.  결과를 검색하여 위험 분석에 통합합니다.

R로 변환된 코드(R 코드)는 준비된 입력 템플릿에서 필요한 입력 값을 정의하여 향상시킬 수 있습니다.

````R
Template <- readLines('RequiredInputData.json')
data <- list(
# drilldown setup
  timeSteps = seq(0, n, by = 1),
  paths = as.integer(seq(0, nt, length.out = min(nt, 100))),
# calc setup
  calcDate = instrument.startDate,
  npaths = nt,
  price = P0,
  vol = sigma,
  drift = mu,
  premium = instrument.premium,
  maturityDate = today()
  )
body <- whisker.render(template, data)
````


다음으로, 웹 서비스를 호출해야 합니다. 이 경우 StartCreditExposure 메서드를 호출하여 계산을 트리거합니다. API의 엔드포인트를 *endpoint*라는 변수에 저장합니다.

````R
# make the call
result <- POST( paste(endpoint, "StartCreditExposure", sep = ""),  
                authenticate(username, password, type = "basic"),
                content_type("application/json"),
                add_headers(`Ocp-Apim-Subscription-Key` = api_key),
                body = body, encode = "raw"
               )

result <- content(result)
````

계산이 완료되면 결과를 검색합니다.

````R
# get back high level results
result <- POST( paste(endpoint, "GetCreditExposureResults", sep = ""), 
                authenticate(username, password, type = "basic"),
                content_type("application/json"),
                add_headers(`Ocp-Apim-Subscription-Key` = api_key),
               body = sprintf('{"getCreditExposureResults": {"token":"DataSource=Production;Organisation=Microsoft", "ticket": "%s"}}', ticket), encode = "raw")

result <- content(result)
````

이렇게 하면 분석가가 받은 결과를 사용하여 계속 진행할 수 있습니다. 관심 있는 관련 위험 수치는 결과에서 추출되어 도표로 표시됩니다.

````R
if (!is.null(result$error)) {
    cat(result$error$message)
} else {
    # plot PFE
    result <- result$getCreditExposureResultsResponse$getCreditExposureResultsResult
    df <- do.call(rbind, result$exposures)
    df <- as.data.frame(df)
    df <- subset(df, term <= n)   
}

plot(as.numeric(df$term[df$statistic == 'PFE']) / 365, df$result[df$statistic == 'PFE'], type = "p", xlab = ("time t in Years"), ylab = ("Potential Future Exposure in USD"), ylim = range(c(df$result[df$statistic == 'PFE'], df$result[df$statistic == 'PFE'])), col = "red")
````


결과 도표는 다음과 같습니다.

|       |     |
|----    |---- |
| <img src="./assets/fsi-risk-modeling/image6.png" width="400px" alt="Figure 4 - Credit Exposure for MSFT Equity Forward - Calculated with a Cloud Based Risk Engine"/> | <img src="./assets/fsi-risk-modeling/image7.png" width="400px" alt="Figure 5 - Potential Future Exposure for MSFT Equity Forward - Calculated with a Cloud  Based Risk Engine" /> |
| 그림 4 - MSFT 선물 주식에 대한 신용 위험 노출 - <br/>클라우드 기반 위험 엔진을 사용하여 계산 | 그림 5 - MSFT 선물 주식에 대한 잠재적 미래 위험 노출 - <br/> 클라우드 기반 위험 엔진을 사용하여 계산 |



## <a name="next-steps"></a>다음 단계

계산 인프라와 SaaS 기반 위험 분석 서비스를 통해 클라우드에 유연하게 액세스할 수 있으므로 자본 시장 및 보험 분야에 종사하는 위험 분석가는 더 빠르고 민첩하게 작업을 수행할 수 있습니다. 이 문서에서는 위험 분석가가 알고 있는 도구를 사용하여 Azure 및 다른 서비스를 사용하는 방법을 설명하는 예제를 살펴보았습니다. 위험 모델을 만들고 향상시킬 때 Azure의 기능을 활용해 보세요.

### <a name="tutorials"></a>자습서


-   R 개발자: [Azure Batch를 사용하여 병렬 R 시뮬레이션 실행](https://docs.microsoft.com/azure/batch/tutorial-r-doazureparallel?WT.mc_id=fsiriskmodelr-docs-scseely)

-   [기본 R 명령 및 RevoScaleR 함수: 25가지 일반적인 예제](https://docs.microsoft.com/machine-learning-server/r/tutorial-r-to-revoscaler?WT.mc_id=fsiriskmodelr-docs-scseely)

-   [RevoScaleR을 사용하여 데이터 시각화 및 분석](https://docs.microsoft.com/machine-learning-server/r/tutorial-revoscaler-data-model-analysis?WT.mc_id=fsiriskmodelr-docs-scseely)

-   [HDInsight의 ML Services 및 오픈 소스 R 기능 소개](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-overview?WT.mc_id=fsiriskmodelr-docs-scseely)

_이 문서는 Darko Mocelj 박사와 Rupert Nicolay에 의해 작성되었습니다._