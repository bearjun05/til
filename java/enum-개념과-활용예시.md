## 0. Overview

---

### 이 글을 읽으면 알게 되는 것들

- [ ]  왜 열거형(Enum) 이 만들어졌는지
- [ ]  열거형(Enum) 은 상수에 비해 어떤 장점을 가지고 있는지
- [ ]  로또 프로젝트 일부 기능 구현을 위한 열거형(Enum) 활용 사례

### 필요한 사전 지식

- 상수(Constant) 에 대한 기본적인 개념

### 참조 했거나 하면 좋을 것들

1. [생활코딩](https://www.opentutorials.org/module/516/6091) 
2. [뱀귤님 티스토리](https://bcp0109.tistory.com/334)
3. [이동욱님의 포스팅](https://techblog.woowahan.com/2527/)

## 1. 배경

---

Enum 은 열거형(enumerated type)이라고 부른다. 열거형은 **서로 연관된 상수들의 집합**이라고 할 수 있다. 
여기서 주목 해야 할 것은 **상수** 라는 것이다.

enum 과 상수는 깊은 연관이 있다.
요약 하자면, **enum 은 상수의 진화형**이라고 할 수 있다. (물론 각자의 장 단점이 있음)

우선 아래 코드를 보고 상수와 enum 과의 연관성을 느껴보자([생활코딩](https://www.opentutorials.org/module/516/6091) 참조)

```java
enum Fruit{
    APPLE, PEACH, BANANA;
}
```

위의 코드는 아래 코드와 사실상 같다.

```java
class Fruit{
    public static final Fruit APPLE  = new Fruit();
    public static final Fruit PEACH  = new Fruit();
    public static final Fruit BANANA = new Fruit();
    private Fruit(){}
}
```

enum은 많은 곳에서 사용하던 상수 디자인 패턴을 언어가 채택해서 문법적인 요소로 **단순화시킨 것**이라고 할 수 있다.

그럼 **우리가 상수 대신 enum을 쓸때 어떤 장점을 얻을 수 있을까?**

우선,

1. **코드가 간결해진다.**
    - 위 코드 2개만 비교해 보더라도 알 수 있다. enum 쪽이 가독성이 높고,
    가독성이 높으면 활용과 유지보수에 용이하다.
2. **enum 자체가 클래스이다.**
    - 열거형 자체가 클래스이기 때문에 열거형 내부에 생성자, 필드, 메소드를 가질 수 있어서
    단순히 상수가 아니라 더 많은 역할을 할 수 있다.
    
    아래 코드를 보자  이렇게 생성자와 필드를 통해 색깔과 가격을 추가시켜 준 모습이다.
    
    ```java
    enum Fruit{
        APPLE("빨강", 2000), PEACH("분홍", 2400), BANANA("노랑", 3000);
    
    		private int 색깔;
        private int 가격;
    
        Fruit(String 색깔, int 가격) {
            this.색깔 = 색깔;
            this.가격 = 가격;
        }
    }
    ```
    

1. **구현부 코드에 의도가 열거임을 분명하게 나타낼 수 있다.**
    - 아래 구현 코드를 보자
    누가 보더라도 Fruit에 연관되어있는 APPLE, PEACH, BANANA 라는 것이 한 눈에 들어오고,
    메소드 접근을 통해서 기존에 매핑 시켜 놓았던 다른 값들도 쉽게 얻을 수 있다.
    
    ```java
   // Fruit.APPLE
   // Fruit.PEACH.색깔 //빨강
   // Fruit.BANANA.가격 //3000
    ```
    

한 줄로 요약해보면,
**enum 은 상수의 성격을 지니면서도 가독성이 뛰어나고 활용이 용이하며, 다양한 기능을 클래스 처럼 구현하여 쓸 수 있는 상수의 진화형이다.** 

혹시, 위 코드 사례와 내용이 이해되지 않는다면 상수에 대한 개념이 부족하지 않는지 확인해 보면 좋을 것이다.

## 2. 활용 예시 (Lotto 기능 구현을 통한)

---

지금부터 보게 될 코드는 Lotto Project의 일부분 코드 이다.

### Lotto project 간략 소개

> 로또 구입 금액을 입력하면 1장 당 로또 금액으로 나눈 숫자 만큼 로또를 발행해준다.
6개의 중복 없는 랜덤 숫자와 1개의 보너스 숫자로 이루어진 각각의 로또 번호들이 있고,
당첨 번호를 입력 받아 발행 받은 로또 번호와 비교하고, 총 당첨금과 각 당첨 유형 개수를 비교한다.
> 

```

구입금액을 입력해 주세요.
8000

8개를 구매했습니다.
[8, 21, 23, 41, 42, 43]
[3, 5, 11, 16, 32, 38]
[7, 11, 16, 35, 36, 44]
[1, 8, 11, 31, 41, 42]
[13, 14, 16, 38, 42, 45]
[7, 11, 30, 40, 42, 43]
[2, 13, 22, 32, 38, 45]
[1, 3, 5, 14, 22, 45]

당첨 번호를 입력해 주세요.
1,2,3,4,5,6

보너스 번호를 입력해 주세요.
7

**당첨 통계
---
3개 일치 (5,000원) - 1개
4개 일치 (50,000원) - 0개
5개 일치 (1,500,000원) - 0개
5개 일치, 보너스 볼 일치 (30,000,000원) - 0개
6개 일치 (2,000,000,000원) - 0개
총 수익률은 62.5%입니다.**
```

여기서 우리가 코드로 구현해야 할 부분은 **당첨 통계** 부분이다.
당첨 유형은 총 5개가 있다.

- 3개 일치
- 4개 일치
- 5개 일치
- 5개 일치 + 보너스 일치
- 6개 일치

그리고 각각 당첨 유형은 상금이 있다
또한 내가 산 여러 장의 로또 중 몇 개가 당첨 됐는지 해당 유형에 대한 개수도 세야 한다.
그렇게 개수와 상금을 곱하고 총 상금을 계산해서 내가 산 로또 복권 금액과 비교해 수익률도 나타내야 한다.

정리해보면,
당첨 유형은 **서로 연관이 있고** 하나의 당첨 유형에 **여러 값들을 동시에 매핑** 시키고,
이를 **자유롭게 이용**하길 원하는 상황이다.

enum 으로 이 문제를 해결해보자,

```java
public enum CorrectNum {
THREE(0, 5000),
FOUR(0, 50000),
FIVE(0, 1500000),
FIVE_BONUS(0, 30000000),
SIX(0, 2000000000),
NOTHING(0, 0);

    private int cnt;
    private int prizeMoney;

    CorrectNum(int cnt, int prizeMoney) {
        this.cnt = cnt;
        this.prizeMoney = prizeMoney;
    }

    public int Cnt() {
        return cnt;
    }

    public int PrizeMoney() {
        return prizeMoney;
    }

    public void setCnt(int cnt) {
        this.cnt = cnt;
    }
}
```

CorrectNum 이라고 하는 enum 클래스를 만들었다.
그리고 각각 당첨 유형을 개수로 세기 위해서 cnt 변수와
상금을 설정하기 위해 prizeMoney 변수를 주었다.

아래 코드는 구현 코드이다.

```java
//System.out.println("당첨 통계");
//System.out.println("---");
//
//for (int i = 0; i < lottos.size(); i++) {
//    List<Integer> lotto = lottos.get(i).getNumbers();
//    CorrectNum correctNumType = (CorrectNum) Draw.findCorrectType(lotto, winNum,    bonusNum);
//    correctNumType.setCnt(correctNumType.Cnt() + 1);
//
//}
//System.out.println("3개 일치 (5,000원) - " + CorrectNum.THREE.Cnt() + "개");
//System.out.println("4개 일치 (50,000원) - " + CorrectNum.FOUR.Cnt() + "개");
//System.out.println("5개 일치 (1,500,000원) - " + CorrectNum.FIVE.Cnt() + "개");
//System.out.println("5개 일치, 보너스 볼 일치 (30,000,000원) - " + CorrectNum.FIVE_BONUS.Cnt() + "개");
//System.out.println("6개 일치 (2,000,000,000원) - " + CorrectNum.SIX.Cnt() + "개");
//System.out.println(String.format("%,.2f%%입니다.", RateOfProfit.calculate(amount)));
```

밑에 코드를 묶어 좀 더 간략하게 정리해 보았다.

```java
//CorrectNum.THREE // 당첨 번호가 3개 있다 라는 것이 쉽게 읽힌다. (가독성 용이)
//
//CorrectNum.FOUR.Cnt() // 당첨 번호 4개 유형의 개수를 출력 (사용하기 쉬움)
//
//CorrectNum correctNumType = 
//	(CorrectNum) Draw.findCorrectType(lotto, winNum, bonusNum);
//// enum은 클래스이기 때문에 데이터 타입으로 하나의 변수에 값을 담아 처리 할 수 있다.
//
//correctNumType.setCnt(correctNumType.Cnt() + 1);
//// 매핑 값을 변경하거나 값을 불러오는 것을 메소드로 쉽게 할 수 있다. 
```

위와 같은 사례 처럼 다양한 값을 각각의 유형에 매핑 시키고 그 값을 변경하거나 사용해야 하는 다양한 상황에서
enum을 활용할 수 있음을 알게 된다.