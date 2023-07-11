# 클로저(Closure)

## Q1. 클로저는 무엇인가요?

클로저는 함수와 함수가 선언된 렉시컬 환경의 조합입니다. 다시 말해, 내부함수가 외부 함수의 변수들에 접근할 수 있는데, 이러한 내부 함수가 외부로 전달되어도 그 접근 권한을 유지하는 특성을 말합니다. 클로저를 사용하면, 함수의 실행이 끝난 뒤에도 해당 함수의 지역 변수에 접근하는 것이 가능합니다.

간편답변: 함수와 함수가 선언된 렉시컬 환경의 조합으로, 내부 함수가 외부 함수의 변수에 접근할 수 있는 기능입니다.

## Q2. 어떻게 클로저를 사용하나요?

기본적으로 함수 내부에 다른 함수를 선언하고, 내부 함수가 외부 함수의 변수에 접근하는 경우에 클로저를 사용하게 됩니다.

## Q3. 왜 클로저를 사용하나요?

클로저를 사용하는 가장 큰 이유는 데이터 은닉입니다. javascript에는 기본적으로 private 또는 protected와 같은 접근 제한자가 없어서 변수에 언제든지 외부에서 접근하고 수정할 수 있습니다. 하지만 이는 버그를 발생시킬 수 있는 요소입니다. 클로저를 사용하면, 함수의 스코프를 이용하여 변수에 대한 접근을 제한할 수 있습니다. 이 방법으로 내부 상태를 보호하고 외부에서의 무분별한 변경을 막아 코드의 안정성을 높일 수 있습니다.

간편답변: 클로저를 사용하여 데이터 은닉과 변수 접근 제한을 통해 코드의 안정성을 높일 수 있습니다.