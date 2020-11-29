# SerialDate 리팩터링
https://www.jfree.org/jcommon 라이브러리의 `SerialDate` 클래스(made by 데이비드 길버트) 리팩토링하기

## 돌려보자
단위 테스트를 돌려보면 모든 경우를 점검하지 않는다는 사실이 드러남 (약 50%)

## 고쳐보기
* 변경 이력 지우기(이제는 소스 제어 도구를 사용하자)
* import 문 간결화
* Javadoc 주석의 HTMP 코드 지우기
* 클래스 선언 시 `Serial`이라는 단어가 부적절함 -> `DayDate`로 변경
* `MonthConstants`는 상속하지 말고 enum으로 선언
* `serialVersionUID` 삭제
* 불필요한 주석, 변수, 생성자 등 삭제
* 변수 위치 올바르게 변경
* Base class가 Derivative class를 모르게 하기
* 불필요한 키워드 삭제(public, final 등)
* 한 메서드가 하나의 메서드에서만 호출되는 경우 하나로 합침

## 결론
결과적으로 테스트 커버리지가 증가했고 버그 몇 개를 고쳤으며 코드 크기가 줄었고 코드가 명확해졌다.
