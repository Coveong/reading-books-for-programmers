# 1장. 리팩터링: 첫 번째 예시

# 1.2 예시 프로그램을 본 소감

- 설계가 나쁜 시스템은 수정하기 어렵다.
    - 원하는 동작을 수행하도록 하기위해 수정해야 할 부분을 찾고, 기존 코드와 잘 맞물려 작동하게 할 방법을 강구하기가 어렵기 때문이다.
    - 무엇을 수정할지 찾기 어렵다면 실수를 저질러서 버그가 생길 가능성도 높아진다.
- **프로그램이 새로운 기능을 추가하기에 편한 구조가 아니라면, 먼저 기능을 추가하기 쉬운 형태로 리팩터링하고 나서 원하는 기능을 추가한다.**

# 1.3 리팩터링의 첫 단계

- 리팩터링할 코드 영역을 꼼꼼하게 검사해줄 테스트 코드들부터 마련해야 한다.
- **리팩터링하기 전에 제대로 된 테스트부터 마련한다. 테스트는 반드시 자가진단하도록 만든다.**

# 1.4 statement() 함수 쪼개기

- 아무리 간단한 수정이라도 리팩터링 후에는 항상 테스트하는 습관을 들여야 한다.
- 조금씩 변경하고 매번 테스트하라
- 리팩터링은 프로그램 수정을 작은 단계로 나눠 진행한다. 그래서 중간에 실수하더라도 버그를 쉽게 찾을 수 있다.
- 몇몇 리팩터링 기법 적용
    - 함수 추출하기(6.1절)
    - 추출 후에는 지금보다 명확하게 표현할 수 있는 간단한 방법은 없는지 검토한다.
        - 이름 짓기는 중요하면서 쉽지 않다.
        - 긴 함수를 작게 쪼개는 리팩터링은 이름을 잘 지어야만 효과가 있다.
    - 인라인으로 선언하게 변경하기
    - 반복문 쪼개기(8.7절)
    - 문장 슬라이드하기 (8.6절)
    - 임시 변수를 질의함수로 바꾸기 (7.4절)

# 1.5 중간 점검: 난무하는 중첩함수

## Before

```jsx
export function statement(invoice, plays) {
  let totalAmount = 0;
  let volumeCredits = 0;
  let result = `청구내역 (고객명: ${invoice.customer})\n`;
  const format = new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD', maximumFractionDigits: 2 })
    .format;

  for (let perf of invoice.performances) {
    const play = plays[perf.playID];
    let thisAmount = 0;

    switch (play.type) {
      case 'tragedy':
        thisAmount = 40_000;

        if (perf.audience > 30) {
          thisAmount += 1_000 * (perf.audience - 30);
        }
        break;
      case 'comedy':
        thisAmount = 30_000;

        if (perf.audience > 20) {
          thisAmount += 10_000 + 500 * (perf.audience - 20);
        }
        thisAmount += 300 * perf.audience;
        break;

      default:
        throw new Error(`알 수 없는 장르: ${play.type}`);
    }

    // 포인트를 적립한다.
    volumeCredits += Math.max(perf.audience - 30, 0);

    // 희극 관객 5명마다 추가 포인트를 제공한다.
    if ('comedy' === play.type) {
      volumeCredits += Math.floor(perf.audience / 5);
    }

    // 청구 내역을 출력한다.
    result += `${play.name}: ${format(thisAmount / 100)} ${perf.audience}석\n`;
    totalAmount += thisAmount;
  }
  result += `총액 ${format(totalAmount / 100)}\n`;
  result += `적립 포인트 ${volumeCredits}점\n`;

  return result;
}
```

## After

```jsx
export function statement(invoice, plays) {
  let result = `청구내역 (고객명: ${invoice.customer})\n`;

  function usd(aNumber) {
    return new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD', maximumFractionDigits: 2 }).format(
      aNumber / 100
    );
  }

  function playFor(aPerformance) {
    return plays[aPerformance.playID];
  }

  function amountFor(aPerformance) {
    let result = 0;

    switch (playFor(aPerformance).type) {
      case 'tragedy':
        result = 40_000;

        if (aPerformance.audience > 30) {
          result += 1_000 * (aPerformance.audience - 30);
        }
        break;
      case 'comedy':
        result = 30_000;

        if (aPerformance.audience > 20) {
          result += 10_000 + 500 * (aPerformance.audience - 20);
        }
        result += 300 * aPerformance.audience;
        break;

      default:
        throw new Error(`알 수 없는 장르: ${playFor(aPerformance).type}`);
    }
    return result;
  }

  function volumeCreditsFor(aPerformance) {
    let result = 0;

    result += Math.max(aPerformance.audience - 30, 0);

    if ('comedy' === playFor(aPerformance).type) {
      result += Math.floor(aPerformance.audience / 5);
    }

    return result;
  }

  function totalVolumeCredits() {
    let result = 0;

    for (let perf of invoice.performances) {
      result += volumeCreditsFor(perf);
    }
    return result;
  }

  function totalAmount() {
    let result = 0;

    for (let perf of invoice.performances) {
      result += amountFor(perf);
    }
    return result;
  }

  for (let perf of invoice.performances) {
    // 청구 내역을 출력한다.
    result += `${playFor(perf).name}: ${usd(amountFor(perf))} ${perf.audience}석\n`;
  }

  result += `총액 ${usd(totalAmount())}\n`;
  result += `적립 포인트 ${totalVolumeCredits()}점\n`;

  return result;
}
```

- 최상위의 `statement()`는 7줄이 되었으며, 출력할 문장을 생성하는 일만 한다.
- 계산 로직은 모두 여러 개의 보주 함수로 빼냈다.

→ 결과적으로 각 계산 과정은 물론 전체 흐름을 이해하기가 훨씬 쉬워졌다.

# 1.6 계산 단계와 포맷팅 단계 분리하기

> 만약 여기에서 포맷팅 단계가 String이 아니라 HTML 형태로 받고 싶다면?
> 

→ 이러한 변경요청사항에 유연하게 대응할 수 있는 구조로 만들자!

개선을 위해 사용할 방법은 “단계 쪼개기”

즉, `statement()`의 로직을 두 단계로 나누는 것

1. `statement()` 에 필요한 데이터를 처리한다.
2. 앞서 처리한 결과를 텍스트나 HTML로 표현한다.

→ 첫 번째 단계에서는 두 번째 단계로 전달할 중간 데이터 구조 생성

이를 위해 두 단계 사이의 중간 데이터 구조 역할을 할 객체를 만들어서 `renderPlainText()`의 인수로 전달한다.

```jsx
export function statement(invoice, plays) {
	const statementData = {}; // 중간 데이터 구조를 인수로 전달
  return renderPlainText(createStatementData(statementData, invoice, plays));
}

function renderPlainText(data, invoice, plays) { // 중간 데이터 파라미터로 전달받음
  let result = `청구내역 (고객명: ${data.customer})\n`;

  for (let perf of data.performances) {
    result += `${perf.play.name}: ${usd(perf.amount)} ${perf.audience}석\n`;
  }

  result += `총액 ${usd(data.totalAmount)}\n`;
  result += `적립 포인트 ${data.totalVolumeCredits}점\n`;

  return result;
}
```

그 이후 다른 인수들 (invoice, plays)는 방금 만든 중간 데이터 구조로 옮기면

계산 관련 코드는 `statement()` 함수로, `renderPlainText()`는 data 매개변수로 전달된 데이터만 처리할 수 있다.

```jsx
export function statement(invoice, plays) {
	const statementData = {};
	statementData.customer = invoice.customer; // 이렇게!
	statementData.performances = invoice.performances;
  return renderPlainText(createStatementData(statementData, plays));
}

```

가변 데이터는 금방 상하기 때문에 데이터를 최대한 불변처럼 취급한다.

그 이후에는 중간 데이터를 사용하도록 바꾸고, 반복문을 파이프라인으로 바꾼다.

최종적으로는 두 단계가 명확히 분리됐으니 각 코드를 별도 파일에 저장한다.

# 1.7 중간 점검: 두 파일(과 두 단계)로 분리됨

statement.js

```jsx
import { createStatementData } from './createStatementData.js';

export function statement(invoice, plays) {
  return renderPlainText(createStatementData(invoice, plays));
}

function renderPlainText(data) {
  let result = `청구내역 (고객명: ${data.customer})\n`;

  for (let perf of data.performances) {
    result += `${perf.play.name}: ${usd(perf.amount)} ${perf.audience}석\n`;
  }

  result += `총액 ${usd(data.totalAmount)}\n`;
  result += `적립 포인트 ${data.totalVolumeCredits}점\n`;

  return result;
}

function htmlStatement(invoice, plays) {
  return renderHtml(createStatementData(invoice, plays));
}

function renderHtml(data) {
  let result = `<h1>청구 내역 ${data.customer}</h1>\n`;

  result += '<table>\n';
  result += '<tr><th>연극</th><th>좌석 수</th><th>금액</th></tr>';

  for (let perf of data.performances) {
    result += `  <tr><td>${perf.play.name}</td><td>${perf.audience}석</td>`;
    result += `<td>${usd(perf.amount)}</td></tr>\n`;
  }
  result += '</table>\n';
  result += `<p>총액: <em>${usd(data.totalAmount)}</em></p>\n`;
  result += `<p>적립포인트: <em>${data.totalVolumeCredits}</em>점</p>\n`;

  return result;
}

function usd(aNumber) {
  return new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD', maximumFractionDigits: 2 }).format(
    aNumber / 100
  );
}
```

createStatementData.js

```jsx
export function createStatementData(invoice, plays) {
  const statementData = {};

  statementData.customer = invoice.customer;
  statementData.performances = invoice.performances.map(enrichPerformance);
  statementData.totalAmount = totalAmount(statementData);
  statementData.totalVolumeCredits = totalVolumeCredits(statementData);

  return statementData;

  function enrichPerformance(aPerformance) {
    const result = Object.assign({}, aPerformance);

    result.play = playFor(result);
    result.amount = amountFor(result);
    result.volumeCredits = volumeCreditsFor(result);

    return result;
  }

  function playFor(aPerformance) {
    return plays[aPerformance.playID];
  }

  function amountFor(aPerformance) {
    let result = 0;

    switch (aPerformance.play.type) {
      case 'tragedy':
        result = 40_000;

        if (aPerformance.audience > 30) {
          result += 1_000 * (aPerformance.audience - 30);
        }
        break;
      case 'comedy':
        result = 30_000;

        if (aPerformance.audience > 20) {
          result += 10_000 + 500 * (aPerformance.audience - 20);
        }
        result += 300 * aPerformance.audience;
        break;

      default:
        throw new Error(`알 수 없는 장르: ${aPerformance.play.type}`);
    }
    return result;
  }

  function volumeCreditsFor(aPerformance) {
    let result = 0;

    result += Math.max(aPerformance.audience - 30, 0);

    if ('comedy' === aPerformance.play.type) {
      result += Math.floor(aPerformance.audience / 5);
    }

    return result;
  }

  function totalAmount(data) {
    return data.performances.reduce((acc, cur) => acc + cur.amount, 0);
  }

  function totalVolumeCredits(data) {
    return data.performances.reduce((acc, cur) => acc + cur.volumeCredits, 0);
  }
}
```

이렇게 모듈화하면 각 부분이 하는 일과 그 부분들이 맞물려 돌아가는 과정을 파악하기 쉬워진다.

모듈화한 덕분에 계산 코드를 중복하지 않고도 HTML 버전을 만들 수 있다.

# 1.8 다형성을 활용해 계산 코드 재구성하기

연극 장르를 추가하고 장르마다 공연료와 적립 포인트 계산법을 다르게 지정하도록 기능을 수정한다.

이번 작업의 목표는 상속 계층을 구성해서 희극 서브클래스와 비극 서브클래스가 각자의 구체적인 계산 로직을 정의하는 것이다.

이 리팩토링 기법의 핵심은 조건부 로직을 다형성으로 바꾸기(10.4절)이다.

그러기 위해서는 상속 계층부터 정의한다. (= 공연료와 적립 포인트 계산 함수를 담을 클래스가 필요!)

공연료 계산기를 만들고 타입 코드를 서브 클래스로 바꾼다. 그 이후에는 생성자를 팩터리 함수로 바꾼다.

```jsx
// 생성자 대신 팩터리 함수 이용
function enrichPerformance(aPerformance) {
  const calculator = createPerformanceCalculator(aPerformance, playFor(aPerformance));
  const result = Object.assign({}, aPerformance);

  result.play = calculator.play;
  result.amount = calculator.amount;
  result.volumeCredits = calculator.volumeCredits;

  return result;
}

function createPerformanceCalculator(aPerformance, aPlay) {
  switch (aPlay.type) {
    case 'tragedy':
      return new TragedyCalculator(aPerformance, aPlay);
    case 'comedy':
      return new ComedyCalculator(aPerformance, aPlay);

    default:
      throw new Error(`알 수 없는 장리: ${aPlay.type}`);
  }
}
```

포인트를 계산하는 로직은 오버라이드하게 만들어서 포인트 계산 방식이 조금 다른 희극 처리 로직을 해당 서브클래스로 내린다.

# 1.9 상태 점검: 다형성을 활용하여 데이터 생성하기

**createStatementData.js**

```jsx
export function createStatementData(invoice, plays) {
  const statementData = {};

  statementData.customer = invoice.customer;
  statementData.performances = invoice.performances.map(enrichPerformance);
  statementData.totalAmount = totalAmount(statementData);
  statementData.totalVolumeCredits = totalVolumeCredits(statementData);

  return statementData;

  function enrichPerformance(aPerformance) {
    const calculator = createPerformanceCalculator(aPerformance, playFor(aPerformance));
    const result = Object.assign({}, aPerformance);

    result.play = calculator.play;
    result.amount = calculator.amount;
    result.volumeCredits = calculator.volumeCredits;

    return result;
  }

  function playFor(aPerformance) {
    return plays[aPerformance.playID];
  }

  function totalAmount(data) {
    return data.performances.reduce((acc, cur) => acc + cur.amount, 0);
  }

  function totalVolumeCredits(data) {
    return data.performances.reduce((acc, cur) => acc + cur.volumeCredits, 0);
  }
}

function createPerformanceCalculator(aPerformance, aPlay) {
  switch (aPlay.type) {
    case 'tragedy':
      return new TragedyCalculator(aPerformance, aPlay);
    case 'comedy':
      return new ComedyCalculator(aPerformance, aPlay);

    default:
      throw new Error(`알 수 없는 장리: ${aPlay.type}`);
  }
}

class PerformanceCalculator {
  constructor(aPerformance, aPlay) {
    this.performance = aPerformance;
    this.play = aPlay;
  }

  get amount() {
    throw new Error('서브클래스에서 처리하도록 설계되었습니다.');
  }

  get volumeCredits() {
    return Math.max(this.performance.audience - 30, 0);
  }
}

/* 서브클래스 */

class TragedyCalculator extends PerformanceCalculator {
  get amount() {
    let result = 40_000;

    if (this.performance.audience > 30) {
      result += 1_000 * (this.performance.audience - 30);
    }

    return result;
  }
}
class ComedyCalculator extends PerformanceCalculator {
  get amount() {
    let result = 30_000;

    if (this.performance.audience > 20) {
      result += 10_000 + 500 * (this.performance.audience - 20);
    }
    result += 300 * this.performance.audience;

    return result;
  }

  get volumeCredits() {
    return super.volumeCredits + Math.floor(this.performance.audience / 5);
  }
}
```

연극 장르별 계산 코드를 함께 묶어두었다.

새로운 장르를 추가하려면 해당 장르의 서브 클래스를 작성하고 생성함수인 `createPerformanceCalculator()`에 추가하기만 하면 된다.