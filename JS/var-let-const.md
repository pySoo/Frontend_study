# var vs let vs const

ES5까지 **var**은 변수를 선언할 수 있는 **유일한** 키워드였지만, 그 단점을 보완하기 위해 ***ES6에서 let, const라는 키워드가 등장***하였다. <br>
ES6는 ES5 기반 위에 새로운 기능을 추가한 것이기 때문에 [var](https://github.com/pySoo/Frontend_study/blob/main/JS/var.md)을 정확하게 이해하면 let과 const의 특징도 이해하기 쉽다.

### **✔ 목표: 각 특징과 차이점에 대해 알아보고 let과 const가 var의 단점을 어떻게 개선했는지 알아보자.**
<br>

## var에 무슨 문제가 있었길래..

> 이전 챕터에서 학습한 내용이다.
- 선언 후 자동으로 undefined로 초기화 →😢 undefined 참조 에러
- 호이스팅 →😢 변수 흐름 파악 어려움으로 코드 가독성 저하
- 함수 레벨 스코프 → 😢 전역 변수 중복 선언
<br>

## 선언과 초기화, 그리고 
var은 너무 자유로운 영혼이다. 🤸 선언 이후 임의의 값으로 초기화를 안 해도 되고, 중복 선언도 가능하다.<br>
그에 비해 const는 많이 보수적인 편이다. 🙅 let은 그 사이라고 생각하면 이해하기 편하다. <br>
- var: 중복 선언 가능 / 재할당 가능 / 선언시 undefined로 초기화
- let: 중복 선언 불가능 / 재할당 가능 / 선언시 undefined로 초기화
- const: 중복 선언 불가능 / 재할당 불가능 / 선언시 반드시 임의의 값으로 초기화
<br>

코드를 보면 이해하는데 도움이 될 것이다.
```javascript
// var 중복 선언 🤸
var a = 10;
var a = 10; // No problem

// let, const 중복 선언 (const도 동일해서 생략) 🙅
let a = 10;
let a = 20; // SyntaxError: Identifier 'a' has already been declared
  
// var, let 재할당 🤸
var a = 10;
a = 20;
console.log(a); // 20

// const 재할당 🙅
const a = 111;
a = 222; // TypeError: Assignment to constant variable.

// var, let 초기화 🤸
var a; 
let b; // 문제 없음

// const 초기화 🙅
const b; // SyntaxError: Missing initializer in const declaration
```
<br>

## 그래서 let이랑 const를 언제 쓰면 좋은데?
변수 선언에는 **기본적으로 const를 사용**하고, **재할당이 필요한 경우에 한정해 let을 사용**하는 것이 좋다. <br>
> ES6를 사용한다면 var 키워드는 사용하지 않는 것을 권장한다.

<br>

### 😯 var의 단점을 어떻게 개선하였나요?
const를 사용하면 의도치 않은 재할당을 방지하고, 선언시 반드시 값을 할당하기 때문에 안전하다. <br>
let, const로 중복 선언 문제를 해결하였다.

<br>

## 스코프 규칙
스코프란 유효한 참조 범위를 뜻한다. <br>
예를 들어, 함수 내부에서 선언된 변수는 함수 내부에서만 참조가 가능하다. 이 경우 변수의 스코프는 함수 내부이다. <br>
- **var: 함수 레벨 스코프** <br>
오로지 함수 내부에 선언된 변수만 지역 변수로 한정한다. <br>
따라서 함수 외부에서 선언한 변수는 모두 전역 변수가 된다.
```javascript
-- 함수와 지역 스코프 예시 --
function example(){
  var i = 10;
  console.log(i);
}
example(); // 10 (함수 내부에서 선언된 i변수는 함수 내부에서만 참조가 가능)
console.log(i); //ReferenceError: i is not defined (함수 외부에서 참조시 에러 발생)

-- 전역 변수 예시 --
var i = 10;
// for 문은 함수가 아니므로 이미 선언된 전역변수 i를 중복 선언한 것이다.
for (var i = 0; i < 5; i++) {
  console.log(i); // 0, 1, 2, 3, 4
 }
// 의도치 않게 i 변수 값이 변경되었다. 🤸
console.log(i); // 5 
```
<br>

- **let, const: 블록 레벨 스코프** <br>
모든 코드 블록(함수, if, for 문 등)에서 선언된 변수는 지역 변수로 인정한다. <br>
```javascript
let foo = 1; // 전역 변수
{
  let foo = 2; // 지역 변수
  const bar = 3; // 지역 변수
}
console.log(foo) // 1 (전역 변수값을 리턴)
console.log(bar) // ReferenceError: bar is not defined (bar이 선언된 코드 블록이 아니므로 참조 불가) 🙅
```
<br>

### 😯 var의 단점을 어떻게 개선하였나요?
블록 레벨 스코프로 전역 변수와 지역 변수의 스코프가 명확해져서, 전역 변수 재할당을 막을 수 있음

<br>

## 호이스팅
Q. var은 호이스팅 때문에 문제가 일어나니까 let과 const는 호이스팅을 막아놨죠?🤔 <br>
A. let, const: 아뇨.. 저희도 해요. 근데 다른 방식으로 합니다.🤗

- var의 호이스팅 <br>
선언과 초기화가 한 번에 진행된다. (undefined로 초기화~) <br>

- let, const의 호이스팅 <br>
선언과 초기화가 분리되어 진행된다. <br>
 1. 런타임 이전에 선언 단계가 실행
 2. 초기화는 변수 선언문을 만났을 때 수행

<br>

```javascript
// 런타임 이전에 선언 단계 실행, 초기화 되지는 않았음
console.log(foo); // ReferenceError: foo is not defined
// Q. 오잉? 선언 단계가 실행 됐는데 왜 참조가 안 되나요?
// A. 스코프 시작부터 초기화 시작 지점까지 변수를 참조할 수 없습니다. 우리는 이것을 일시적 사각지대(TDZ)라고 부르기로 했어요.

let foo; // 변수 선언문에서 초기화가 수행된다.
console.log(foo); undefined

foo = 1; // 할당 단계
console.log(foo); // 1
```

### 일시적 사각지대(TDZ): 변수의 선언과 초기화 사이에 일시적으로 변수 값을 참조할 수 없는 구간
- let/const는 일시적 사각지대로 인해 호이스팅이 발생하지 않는 것처럼 동작한다.

<br>

### 다시 정리해봅시다!
- var: 선언과 초기화가 한 번에 진행 <br>
- let, const: 선언과 초기화가 분리됨. 그러므로 변수 선언과 초기화 사이에 일시적 사각지대가 생기는데, 이 때문에 호이스팅이 발생하지 않는 것처럼 동작한다.<br>
<br>

### 😯 var의 단점을 어떻게 개선하였나요?
var의 호이스팅은 에러를 발생시키지는 않지만 변수 선언 이전에도 참조가 가능하여 코드 가독성을 낮춘다. <br>
let, const는 선언과 초기화를 분리하여 변수 선언 이후부터 변수를 참조할 수 있기 때문에, 변수의 흐름을 파악하는데 용이하다.<br>

<br>

## 전역 객체 프로퍼티
- var: var로 선언된 변수는 전역 객체(브라우저 환경의 경우 window)의 프로퍼티이다.
- let, const: let/const 로 선언된 변수는 전역 객체 프로퍼티가 아니다.

```javascript
// 브라우저 환경에서 실행시, var로 선언한 변수 a는 브라우저 전역객체인 window의 프로퍼티로 할당된다.
var a = 10;

console.log(window.a);  // 10
console.log(a);  // 10
 
// let, const: 전역 객체의 프로퍼티로 할당되지 않는다.
let a = 10;
console.log(window.a); // undefined
console.log(a); // 10
```
### 😯 var의 단점을 어떻게 개선하였나요?
단점을 개선하였다기 보다는 차이점에 가깝습니다. <br>
<br>

## 최종 정리
1. 선언과 초기화, 재할당
2. 스코프 규칙
3. 호이스팅
4. 전역 프로퍼티
<br>
목차를 보면서 차이점을 떠올려 봅시다! 백지 공부법..! 💪
<br><br>

## 구글의 JS 스타일 가이드로 글을 마무리하려 합니다.
```
Use const and let
Declare all local variables with either const or let.

Use const by default, unless a variable needs to be reassigned.

The var keyword must not be used.

1. const와 let을 이용해서 변수를 선언하라.

2. 값을 재할당하는 경우가 아니라면, const를 디폴트로 사용하라.

3. var는 절대로 사용하지 말라
```

### ✔ 변수의 특성과 차이점에 대해 알아보면서, ES6에서 let, const로 ES5의 var의 단점을 어떻게 개선하였는지 알 수 있었습니다. <br> <br>

## 글을 쓰는 목적
기초적인 내용이지만 웹 개발의 근간이 되는 JS의 동작 원리를 정확하게 파악하지 않은 상태로 <br>
좋은 코드, 좋은 성능의 웹을 만들고 싶다는 목표를 가지는 것은 부끄러운 욕심이라는 생각이 들어 JS 정리를 시작하게 되었습니다. <br>
앞으로도 꾸준히 글을 올려보려고 합니다. <br>
이 글이 누군가가 학습하는데 도움이 된다면 더욱 기쁠 것 같습니다! <br>
(개념 오류가 없도록 "모던 자바스크립트 Deep Dive" 책을 참조하여 기술하고 있습니다.) 
 
