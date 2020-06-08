# 준비

## Hands-on Lab 진행 순서

다음과 같은 단계를 거쳐 최종적인 예측(forecast)를 얻게 됩니다.  
데이터-셋 그룹 작성 > 데이터 입수 > 예측기 학습 > 예측 생성 > 예측 보기  
이것을 크게 묶음지어, console에서는 다음과 같이 표현하고 있습니다.

![Forecast 수행 단계](../../images/forecast/steps/00-00-steps.png)

이 모든 것은 AWS console에서 진행되며 별도의 도구나 웹 브라우저 외의 다른 환경이 요구되지 않습니다.
사용하는 서비스는 Amazon Forecast와 Amazon S3/Simple Storage Service 뿐입니다.

한국어로 만들어진 개발자 안내서에서는 forecast를 '예상'이라고 번역하였지만, 여기에서는 '예측'으로 씁니다.
predictor는 '예측기'라고 번역되어 있는데, 이는 그대로 가져옵니다. 저의 관점에서 '예상豫想'은 
정성적인 그리고 감성적인 그리고 비계측 분야의 언어라고 생각하며, predictor의 예측기와 겹치는 것에 대한
우려가 있음에도 불구하고, 이성적 계측적 그리고 정량적이라고 판단되는 '예측豫測'이라는 단어로 번역합니다.

### 지름길이 필요하시다면

GitHub에 공개된 [aws-samples/amazon-forecast-samples의 notebook](https://github.com/aws-samples/amazon-forecast-samples/tree/master/notebooks)을 보면,
CloudFormation으로 해당 데모를 손쉽게 배포하고 결과를 조회하는 방법이 소개되고 있습니다.
만약 Amazon Forecast의 결과 중심의 데모를 빠르게 확인하고 여러분의 비즈니스에 적용여부를
알고 싶으시다면 이 방법을 추천드립니다.

GitHub의 그것과 달리, 본 페이지에서 소개하는 것은 개별 옵션에 대한 설명과 과정에 대한 안내입니다.

## 여러분의 컴퓨팅 환경을 점검하세요

Amazon Forecast hands-on lab을 수행하기 위해 다음의 준비가 필요합니다.

* 웹 브라우저  
    [console.aws.amazon.com](https://console.aws.amazon.com)에
    접근하고 어떤 값을 입력하고 출력값을 조회하는 데에 문제가 없다고
    판단되는 웹 브라우저 - 여러분께서 지금 사용하고 계시는 웹 브라우저가
    [Chromium](https://www.chromium.org/) 혹은
    [Mozilla Gecko](https://developer.mozilla.org/en-US/docs/Mozilla/Gecko)
    엔진을 사용하면 대략 괜찮다 판단합니다.
    이제 세상과 이별을 준비하고 있는 [EdgeHTML](https://en.wikipedia.org/wiki/EdgeHTML)
    엔진도 무리가 없다는 이야기가 있던데, 경험이 없어 확신할 수는 없습니다.
    어떤 분은 iPadOS의 기본 웹 브라우저, Safari로 이 모든 것을 해내던데 어떤 불편함은 없었는지 궁금합니다.
    참, 이 방법은 추천하지 않습니다. macOS의 Safari도 특정 부분에서 라디오 버튼을 클릭할 수 없는 등의
    문제가 봉착할 수도 있습니다.
* IT에 관한 기본 그리고 AWS에 관한 짧은 지식  
    [CSV](https://en.wikipedia.org/wiki/Comma-separated_values)는 무엇이고
    원시 정보를 어떻게 CSV로 변환하는지, 그리고 S3는 무엇이고 S3에 파일을 어떻게 업로드하며,
    등에 관한 이야기는 전혀 하지 않을 것입니다.
* AWS account  
    본 학습을 진행하면서 다루게 되는 각종 서비스로 인하여 사용자에게 일정 요금이 청구될 수 있습니다.

## 기타

2020-05-20, 현재 Amazon Forecast는 여러분의 기대와는 다르게 오로지 미국식 영어(enUS)만 화면 출력됩니다.
한국어가 지원되면 다음 페이지부터 선보일 화면 캡쳐본을 영문에서 한국어로 전환할지는 그 때가서 고민해 보겠습니다.

예제로써 활용된 리전은 ap-northeast-2, '서울'입니다.
현 시점 Amazon Forecast는 서울 리전 포함, 제한된 리전에서만 서비스가 개시되어 있습니다.

### 이제 접속합시다

Amazon Forecast를 본격적으로 경험하기 위하여 다음의 URL로 접속합니다.
[console.aws.amazon.com/forecast](https://console.aws.amazon.com/forecast/) 그리고,
다음 장으로 넘어가 봅시다.
