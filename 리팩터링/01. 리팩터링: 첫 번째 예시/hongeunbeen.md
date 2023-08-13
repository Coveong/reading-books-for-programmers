# 01. 리팩터링: 첫 번재 예시

## 1. 시작하기

- 다양한 연극을 외주로 받아 공연하는 극단의 이용 가격 계산 프로그램
    - 연극의 장르와 관객 규모를 기초로 비용을 책정
        - 비극(tragedy), 희극(comedy) 두 장르만 공연함
    - 공연료와 별개로 포인트를 지급하여 다음번 의뢰 시 공연로 할인 가능

```jsx
function statement(invoice, plays){
	let totalAmount = 0;
	let volumeCredits = 0;
	let result = `청구 내역 (고객명: ${invoice.customer})\n`;
	const format = new Intl.NumberFormat("en-US", {
																										style: "currency", 
																										currency: "USD", 
																										minimumFractionDigits: 2
																								}).format;

	for(let perf of invoice.performances) {
		const play = plays[perf.playID];
		let thisAmount = 0;

		switch(play.type){
			case "tragedy": //비극
				thisAmount = 4000;
				if(perf.audience > 30) {
					thisAmount += 1000 * (perf.audience - 30);
				}
				break;
			case "comdey":
				thisAmount = 3000;
				if(perf.audience > 20) {
					thisAmount += 10000 + 500 * (perf.audience - 20);
				}
				thisAmount += 300 * perf.audience;
				break;
			default:
				throw new Error(`알 수 없는 장르: ${play.type}`);
		}

		//포인트를 적립한다.
		volumeCredits += Math.max(perf.audience - 30, 0);
		//희극 관객 5명마다 추가 포인트를 제공한다.
		if("comedy" === play.type) volumeCredits += Math.floor(perf.audience / 5);

		//청구 내역을 출력한다.
		result += `${play.name}: ${format(thisAmount/100)} (${perf.audience}석)\n`;
		totalAmount += thisAmount;
	}

	result += `총액 ${format(totalAmount/100)}\n`;
	result += `적립 포인트: ${volumeCreditws}점\n`;

	return result;
}
```

## 2. 리팩터링 필요한 부분 찾기

> 프로그램이 새로운 기능을 추가하기에 편한 구조가 아니라면, 먼저 기능을 추가하기 쉬운 형태로 리팩터링하고 나서 원하는 기능을 추가한다.
> 
- 청구 내역을 HTML로 출력하는 기능 필요
    - 🤔 청구서 작성 로직을 변경할 때마다 HTML 버전 함수 수정해야 하는 번거로움 있음
- 연극 장르와 공연료 정책이 달라질 가능성 있음

## 3. 리팩터링 첫 단계

> 리팩터링하기 전에 제대로 된 테스트부터 마련한다. 테스트는 반드시 자가진단하도록 만든다.
> 
- 리팩터링할 코드 영역을 검사해줄 테스트 코드 마련 필요
- 성공/실패를 스스로 판단하는 자가진단 테스트로 만들어야 함

## 4. statement() 함수 쪼개기

> 리팩터링은 프로그램 수정을 작은 단계로 나눠 진행한다. 그래서 중간에 실수하더라도 버그를 쉽게 찾을 수 있다.
> 
- statement() 함수 속 swtich 문 역할 = 공연에 대한 요금 계산
    
    ```jsx
    switch(play.type){
    	case "tragedy": //비극
    		thisAmount = 4000;
    		if(perf.audience > 30) {
    			thisAmount += 1000 * (perf.audience - 30);
    		}
    		break;
    	case "comdey":
    		thisAmount = 3000;
    		if(perf.audience > 20) {
    			thisAmount += 10000 + 500 * (perf.audience - 20);
    		}
    		thisAmount += 300 * perf.audience;
    		break;
    	default:
    		throw new Error(`알 수 없는 장르: ${play.type}`);
    }
    ```
    
- 함수 추출
    - 추출한 함수에는 코드가 하는일을 설명하는 이름 지음
    - 함수 추출 시 유효범위를 벗어나는 변수 확인 필요
    
    ```jsx
    function amountFor(perf, play) { //값이 변경되지 않는 변수는 매개변수로 전달
    	
    	let result = 0; //변수 초기화 코드
    
    	switch(play.type){
    		case "tragedy": //비극
    			result = 4000;
    			if(perf.audience > 30) {
    				result += 1000 * (perf.audience - 30);
    			}
    			break;
    		case "comdey":
    			result = 3000;
    			if(perf.audience > 20) {
    				result += 10000 + 500 * (perf.audience - 20);
    			}
    			result += 300 * perf.audience;
    			break;
    		default:
    			throw new Error(`알 수 없는 장르: ${play.type}`);
    	}
    
    	return result;
    }
    ```
    
- 임시 변수를 질의 함수로 바꾸기
    - 임시 변수들로 로컬 범위에 존재하는 이름이 늘어나 추출 작업이 어려워짐
    
    ```jsx
    function playFor(aPerformance) {
    	return plays[aPerformance.playID];
    }
    
    //statement 함수
    function statement(invoice, plays){
    	...
    	for(let perf of invoice.performances) {
    		//const play = playFor(perf);
    		let thisAmount = amountFor(perf, playFor(perf));
    		...
    	}
    	...
    }
    ```
    
- 변수 인라인하기 적용 (play 임시 변수 삭제 가능)
    - 성능이 느려지더라도 이전 코드보다 성능 개선이 수월해짐.
    - 지역 변수를 제거함으로서 유효범위를 신경써야 할 대상 줄어듬
    
    ```jsx
    function amountFor(perf) { 
    
    	let result = 0;
    
    	switch(playFor(aPerformance).type){
    		case "tragedy": //비극
    			result = 4000;
    			if(perf.audience > 30) {
    				result += 1000 * (perf.audience - 30);
    			}
    			break;
    		case "comdey":
    			result = 3000;
    			if(perf.audience > 20) {
    				result += 10000 + 500 * (perf.audience - 20);
    			}
    			result += 300 * perf.audience;
    			break;
    		default:
    			throw new Error(`알 수 없는 장르: ${playFor(aPerformance).type}`);
    	}
    
    	return result;
    }
    
    //statement 함수
    function statement(invoice, plays){
    	...
    	for(let perf of invoice.performances) {
    		//let thisAmount = amountFor(perf);	
    		...
    		//청구 내역을 출력한다.
    		result += `${play.name}: ${format(amountFor(perf)/100)} (${perf.audience}석)\n`;
    		totalAmount += amountFor(perf);
    	}
    	...
    }
    ```
    
- 함수 선언 바꾸기
    - 리팩터링시 이름을 잘 지어야만 효과가 있음
    
    ```jsx
    function usd(aNumber){
     return new Intl.NumberFormat("en-US", {
    																					style: "currency", 
    																					currency: "USD", 
    																					minimumFractionDigits: 2
    																			}).format(aNumber/100); //단위 변환 로직
    }
    	
    //statement 함수
    function statement(invoice, plays){
    	...
    	for(let perf of invoice.performances) {
    	  ...
    		result += `${play.name}: ${usd(amountFor(perf))} (${perf.audience}석)\n`;
    		totalAmount += amountFor(perf);
    	}
    	result += `총액 ${usd(totalAmount)}\n`;
    	...
    }
    ```
    
- 반복문 쪼개기
    - 중복 반복문으로 성능이 걱정된다면 (리팩터링 과정에서 성능 떨어짐) 리팩터링 후 성능 개선하기
    - 리팩터링으로 인한 성능에 대한 조언 ‘특별한 경우가 아니라면 일단 무시하라’
    
    ```jsx
    function totalAmount(){
    	let result = 0l
    	for(let perf of invoice.performances){
    		result += amountFot(perf);
    	}
    	return result;
    }
    
    function totalVolumeCredits(){
    	let result = 0;
    	for(let perf of invoice.performances){
    		result += volumeCreditsFor(perf);
    	}
    	return result;
    }
    
    function statement(invoice, plays){
    	let result = `청구 내역 (고객명: ${invoice.customer})\n`;
    	for(let prerf of invoice.performances){
    		result += `${playFor(perf).name}: ${usd(amountFor(perf)} (${perf.audience}석)\n`;
    	}
    	result += `총액: ${usd(totalAmount)}\n`;
    	result += `적립 포인트: ${totalVolumeCredits()}점\n`;
    	return result;
    }
    ```
    
    - 리팩터링 과정 (잘게 나누기)
        1. 반복문 쪼개기: 변수 값을 누적시키는 부분 분리
        2. 문장 슬라이드하기: 변수 초기화 문장을 변수 값 누적 코드 바로 앞으로 옮기기
        3. 함수 추출하기: 적립 포인트 계산 부분을 별도 함수로 추출하기
        4. 변수 인라인하기: volumeCredits 변수를 제거하기

→ 프로그램의 논리적인 요소 파악하기 쉬도록 코드의 구조 보강 리팩터링!

## 5. 계산 단계와 포맷팅 단계 분리하기

- 기능 변경 리팩터링
    
    ```jsx
    function statement(invoice, plays){
    	return renderPlainText(createStatementData(invoice, plays));
    }
    
    function createStatementData(invoice, plays){
    	consst statementData = {};
    	statementData.customer = invoice.customer;
    	statementData.performances = invoice.performances.map(enrichPerformance);
    	statementData.totalAmount = totalAmount(statementData);
    	statementData.totalVolumeCredits = totalVolumeCredits(statementData);
    
    	return statementData;
    }
    
    function enrichPerformance(aPerformance) {...} 
    function playFor(aPerformance) {...} 
    function amountFor(aPerformance) {...} 
    function volumeCreditsFor(aPerformance) {...} 
    function totalAmount(data) {...} 
    function totalVolumeCredits(data) {...}
    ```
    

## 6. 다형성을 활용해 계산 코드 재구성하기

- 조건부 로직을 다형성으로 바꾸기
    - 조건부 로직을 명확한 구조로 보완하는 방법
        - 상속 계층을 구성하여 희극 서브클래스와 비극 서브 클래스가 각자의 구체적인 계산 로직 정의하기
    - 조건부 코드 한 덩어리를 다형성을 활용하는 방식으로 바꿈
    
    ```jsx
    export default function createStatementData(invoice, plays) { 
    	const result = {}; 
    	result.customer = invoice.customer; 
    	result.performances = invoice.performances.map(enrichPerformance); 
    	result.totalAmount = totalAmount(result); 
    	result.totalVolumeCredits = totalVolumeCredits(result); 
    	return result; 
    	
    	function enrichPerformance(aPerformance) { 
    		const calculator = createPerformanceCalculator(aPerformance, playFor(aPerformance)); 
    		const result = Object.assign({}, aPerformance); 
    		result.play = calculator.play; 
    		result.amount = calculator.amount; 
    		result.volumeCredits = calculator.volumeCredits; return result; 
    	}
    
    	function playFor(aPerformance) { 
    		return plays[aPerformance.playID] 
    	} 
    
    	function totalAmount(data) { 
    		return data.performances 
    			.reduce((total, p) => total + p.amount, 0); 
    	} 
    
    	function totalVolumeCredits(data) { 
    		return data.performances 
    			.reduce((total, p) => total + p.volumeCredits, 0);
    	 } 
    } 
    
    function createPerformanceCalculator(aPerformance, aPlay) { 
    	switch(aPlay.type) { 
    		case "tragedy": return new TragedyCalculator(aPerformance, aPlay); 
    		case "comedy" : return new ComedyCalculator(aPerformance, aPlay); 
    		default: 
    			throw new Error(`알 수 없는 장르: ${aPlay.type}`); 
    	} 
    } 
    
    class PerformanceCalculator { 
    	constructor(aPerformance, aPlay) { 
    		this.performance = aPerformance; 
    		this.play = aPlay; 
    	} 
    
    	get amount() { throw new Error('서브클래스에서 처리하도록 설계되었습니다.'); } 
    	get volumeCredits() { return Math.max(this.performance.audience - 30, 0); } 
    } 
    
    class TragedyCalculator extends PerformanceCalculator {
    	get amount() {
    		let result = 40000; 
    		if (this.performance.audience > 30) { 
    			result += 1000 * (this.performance.audience - 30); 
    		} 
    		return result; 
    	}
     } 
    
    class ComedyCalculator extends PerformanceCalculator { 
    	get amount() { 
    		let result = 30000; 
    		if (this.performance.audience > 20) { 
    			result += 10000 + 500 * (this.performance.audience - 20); 
    		} 
    
    		result += 300 * this.performance.audience; 
    		return result; 
    	}
    	 
    	get volumeCredits() { 
    		return super.volumeCredits + Math.floor(this.performance.audience / 5); 
    	} 
    }
    ```
    
- 리팩터링 과정
    1. 상속 계층 정의(공연료, 적립 포인트 계산 함수 담을 클래스 생성)
    2. 공연료 계산기 만들기 (공연 관련 데이터를 전용 클래스로 옮기기)
    3. 공연료 계산 코드를 계산기 클래스 안으로 복사
    4. 공연료 계산기 다형성 버전 만들기
        - 타입 코드를 서브클래스로 바꾸기 (타입 코드 대신 서브 클래스 사용하도록 변경)
        - 생성자를 팩터리 함수로 바꾸기 (서브 클래스 사용위해 생성자 대신 함수로 호출)

## 7. 정리

- 다양한 리팩터링 기법
    - 함수 추출하기
    - 변수 인라인하기
    - 함수 옮기기
    - 조건부 로직을 다형성으로 바꾸기
- 리팩터링은 대부분 코드가 하는 일을 파악하는데서 시작
    - 코드 읽기 → 개선점 찾기 → 리팩터링 작업으로 코드에 반영
- 리팩터링을 효과적으로 하는 핵심 = 단계를 잘게 나누기
