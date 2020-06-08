# 데이터-셋 그룹 작성

접속을 완료하면, 다음의 화면을 만나게 됩니다. 여기에서 그림의 화살표가 가르키는
'...'을 클릭하여 다음 단계로 진행합니다.

![getting start](/images/forecast/steps/01-00.png)

먼저 데이터-셋 그룹(Dataset Group)을 작성합니다.
데이터-셋 그룹은 여러 데이터-셋(Datasets)의 묶음입니다.
하나의 데이터-셋 그룹에는 세 가지 데이터-셋이 존재할 수 있습니다.[^1]

[^1]:   2020-05-20 현재의 제한입니다.

물론, 데이테-셋 그룹 내에 하나의 데이터-셋만 존재해도 무관합니다.
이 데이터-셋 그룹은 Forecast를 운영하는데 기본 단위가 됩니다.
데이터-셋과 데이터-셋 그룹의 성격과 입력하는 데이터 성격과 스키마에 대해서는
공식 [개발자 안내서](https://docs.aws.amazon.com/forecast/latest/dg/howitworks-datasets-groups.html)를 
참조하길 바랍니다.

![create Dataset Group](/images/forecast/steps/01-01-create-dataset-group.png)

Dataset group name 항목에 임의의 데이터-셋 그룹의 이름을 기입합니다.
이름을 짓는 것입니다. 이럴 때 자유로운 창작의식을 자극해야 합니다.
물론, 화면에 보이는 것과 같이 이름에는 규칙
``The dataset group name must have 1 to 63 characters. Valid characters: a-z, A-Z, 0-9, and _``
이 있긴 합니다.

그리고 Forecasting domain을 지정합니다.
Forecasting domain은 지금 행하려는 작업의 성격을 규정하는 것입니다.
2020년 5월 20일 기준, 아래와 같이 7가지 도메인에 대한 정의가 가능합니다.

![Choose a forecasting domain](/images/forecast/steps/01-02-domains.png)

도메인(영역)의 선택은 그 분야에서만 관찰되는 특별한 변위를 가하여 예측값을 보다 정교하게 만들어 줍니다.
만약 어느 도메인에도 적합하지 않다고 판단되면 Custom을 선택할 수 있습니다.

* Retail: 소매업의 판매 추이를 예측합니다.
    Amazon Forecast가 가장 잘 하는 분야이기도 합니다. 그리고 태생이 이와 같은
    분야입니다.
* Custom: (위 아래) 어떤 도메인에도 속하지 않는다면 선택할 수 있습니다.
* Inventory Planning: 특정 아이템의 적정 재고를 가늠하는 역할을 합니다. '판매 추이'와는 조금 다른 성격입니다.
* EC2 capacity: EC2의 필요 용량을 예측합니다. Auto Scaling Group으로 훌륭하게 이 부분을
    갈음할 수도 있겠지만, 신경쓰지 않고 지갑을 여는 행위 대신 언제 얼마만큼의 용량이 요구되어서
    내 지갑에 얼마의 돈을 준비해 두어야 하는지에 대한 예측을 하고 싶다면 이 도메인이 쓸모 있겠습니다.
* Work force: 배치 인력의 수를 계획하는 데 참고 자료를 만들 수 있습니다. '고용'과 '노동'이라는 단어가
    매우 민감함 한국에서는 생소한 분야일 수도 있겠지만 ...
    어떤 이벤트가 발생하고 그 이벤트는 항상 '사람'이 필요하다는 가정, 그래서 선재적으로 
    고용(혹은 파견 회사로부터 인력을 공급을 받아야 한다면)을 해야하고 그 고용은
    단기적이며 해고(계약)의 기간을 예측할 수 있다면 인력 자원 활용을 능률적으로 해 낼 수 있겠습니다.
* Web traffic: 웹 사이트에 접속 수 - 트래픽을 예측합니다.
* Metrics: 현금 흐름, 매출, 판매 등을 예측합니다.

위 설명은 개략적인 것이며, 사용자 스스로가 예측할 부분을 적당히[^2] 선택할 수 있습니다.  
이번 실습에서는 '**Custom**'을 선택합니다.

![create dataset group](/images/forecast/steps/01-03-create-dataset-group.png)

그리고 `Next`{style='background-color:#ef6c00; color:white'}를 클릭합니다.

[^2]: 적당(適當)은 '정도에 알맞다'라는 뜻인데, 
    어찌하다 보니 이리 부정한 인식이 이 단어에 씌워져 있습니다. 
    그래서 사람의 습관이라는 것과 그 습관이 굳어졌을 때의 결과는 본래와 반대인 경우가 있습니다.
    '안절부절하다'가 옳은 표현인데, '안절부절못하다'가 표준어가 되는 웃음거리는 주위에 널려 있습니다.