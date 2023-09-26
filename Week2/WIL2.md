# 모던 자바스크립트 입문 2주차

## 8. 함수

### 함수 정의 방법

1. 함수 선언문

   ```javascript
   function add(a, b) {
     return a + b;
   }
   ```

   - 함수 선언문은 함수 이름을 생략할 수 없다.
2. 함수 리터럴

   ```javascript
   var add = function(a, b) {
     return a + b;
   };
   ```

   - 함수 리터럴은 함수 이름을 생략할 수 있다.
   - 함수 리터럴은 함수 이름을 생략할 수 있기 때문에 익명 함수로도 불린다.
   - 함수 리터럴은 함수 이름을 생략할 수 있기 때문에 함수 표현식이라고도 불린다.
   - 함수 리터럴은 함수 이름을 생략할 수 있기 때문에 함수 선언문이 아닌 문법적인 표현이다.
3. `Function` 생성자로 정의

   ```javascript
   var add = new Function('a', 'b', 'return a + b');
   ```

   - `Function` 생성자로 정의하는 방법은 사용하지 않는 것이 좋다.
   - `Function` 생성자로 정의하는 방법은 함수 리터럴로 정의하는 것보다 느리다.
   - `Function` 생성자로 정의하는 방법은 함수 리터럴로 정의하는 것보다 가독성이 떨어진다.
4. 화살표 함수

   ```javascript
   var add = (a, b) => a + b;
   ```

   - 화살표 함수는 함수 리터럴을 축약한 것이다.
   - 화살표 함수는 익명 함수로만 정의할 수 있다.
   - 화살표 함수는 함수 리터럴보다 가독성이 좋다.


### 중첩 함수

- 함수 안에 함수를 정의할 수 있다.
- 중첩 함수는 외부 함수의 지역변수에 저장되므로...
    - 자신을 감싼 외부 함수 안에서만 호출할 수 있다.
    - 자신을 감싼 외부 함수의 인수와 지역변수에 접근할 수 있다.
- 외부 함수의 변수 스코프가 중첩 함수에도 적용된다는 것은 클로저의 핵심 원리이다.


### 즉시 실행 함수

- 즉시 실행 함수는 함수를 정의하고 즉시 호출하는 것이다.
- 익명 함수는 다음과 같이 정의하고 할당했다.
```javascript
var add = function(a, b) {
  return a + b;
};

console.log(add(2, 3)); // 5
```
- 이를 즉시 호출하는 것은 다음과 같다.

```javascript
(function(a, b) {
  return a + b;
})(2, 3); // 5
```
- 구체적인 원리는 다음과 같다.
    - 익명 함수를 괄호로 감싸면 함수 정의식이 평가되어 함수 객체(의 참조값)이 된다.
- 즉시 실행 함수는 IIFE(Immediately Invoked Function Expression)라고도 불린다.
- 즉시 실행 함수는 비공개 변수를 만들 때 사용한다. 즉 전역 스코프를 오염시키지 않는 네임스페이스를 만들 때 사용한다.

### 