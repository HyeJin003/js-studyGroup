# JavaScript 객체(Object)

## 1. 객체란?

객체(Object) = 여러 개의 데이터(프로퍼티) 와 동작(메소드) 을 하나로 묶은 것

쉽게 말해 “사물의 특징과 행동을 코드로 표현한 것”  
예: 자동차 객체 → { 색상: "빨강", 속도: 100, 달린다: function() {} }

## 2. 객체를 만드는 방법

(1) 객체 리터럴

가장 간단한 방법. 중괄호 {} 안에 키:값 쌍을 나열.
```
const person = {
  name: "은별",
  age: 25,
  sayHi: function() {
    console.log("안녕!");
  }
};
```
👉 필요한 순간 바로 객체를 만들 때 가장 많이 쓰는 방식.

(2) new Object() 생성자 함수
```
const person = new Object();
person.name = "은별";
person.age = 25;
```
👉 실제로는 잘 안 쓰이고, 내부적으로는 객체 리터럴도 결국 이 방식을 기반으로 동작함.

(3) 사용자 정의 생성자 함수
비슷한 구조의 객체를 여러 개 찍어낼 때 사용.
```
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.sayHi = function() {
    console.log("안녕!");
  };
}

const p1 = new Person("은별", 25);
const p2 = new Person("지민", 30);
```
👉 new 키워드를 붙여야 새로운 객체가 생성됨.  
👉 this 는 새로 만들어질 그 객체를 가리킴.
   ex.  p1.name = "은별"
        p1.age = 25

## 3. 프로퍼티 다루기
(1) 프로퍼티 키

- 무조건 문자열(혹은 Symbol)
- 숫자, 불리언을 키로 써도 자동으로 문자열로 바뀜
- 식별자 규칙에 맞으면 따옴표 없이 쓰고, 아니면 따옴표 필요.
```
const obj = {
  name: "은별",      // OK
  "my-name": "고은별" // 따옴표 필요
};
```

(2) 값 읽기

- 점 표기법 → obj.name
- 대괄호 표기법 → obj["name"] (변수를 키로 쓸 수 있음)

```
const key = "age";
console.log(obj[key]); // obj.age 와 같음
```

(3) 값 추가 & 수정
```
obj.job = "개발자";   // 새 프로퍼티 추가
obj.age = 26;        // 값 갱신
```

(4) 값 삭제
- 프로퍼티만 삭제 가능. 객체 자체는 삭제 불가능.
```
delete obj.age;
```

(5) 프로퍼티 열거
```
for (let key in obj) {
  console.log(key, obj[key]);
}
```

👉 모든 열거 가능한 키를 순회 (순서 보장 안 됨).  
👉 배열에는 잘 안 씀 (추가 프로퍼티까지 돌아서 예기치 않은 결과 가능).

## for...in

- 객체의 속성 이름(키)을 반복함.  
- 배열에서는 인덱스를 반환함.

```
javascriptconst obj = { a: 1, b: 2, c: 3 };
for (let key in obj) {
  console.log(key); // "a", "b", "c" (속성 이름)
}

const arr = ['사과', '바나나', '오렌지'];
for (let index in arr) {
  console.log(index); // "0", "1", "2" (인덱스, 문자열)
}
```

- 객체로서 어떻게 사용하는지에 따라 모든 속성을 전부 꺼낼 때 사용.(수업 내용! 01-27)
```
const arr = [10, 20, 30];
arr.name = '배열핑';

console.log('for...in');
for (var prop in arr) {
  console.log(prop, arr[prop])
}
```

## for...of

- 값 자체를 반복함.  
- 배열, 문자열, Set, Map 등 반복 가능한(iterable) 객체에서 사용

```
javascriptconst arr = ['사과', '바나나', '오렌지'];
for (let value of arr) {
  console.log(value); // "사과", "바나나", "오렌지" (실제 값)
}

const str = 'hello';
for (let char of str) {
  console.log(char); // "h", "e", "l", "l", "o"
}
```
## 4. 값 vs 참조 (중요!)

- **값 복사(pass-by-value)**
- 원시 타입(숫자, 문자열 등) 은 값 자체를 복사.

```
let a = 1;
let b = a;
a = 10;
console.log(b); // 1 (영향 없음)
```

- **참조 전달(pass-by-reference)**
- 객체 타입 은 참조(주소)를 복사.(원본까지 수정 됨.)

```
let o1 = { x: 1 };
let o2 = o1;
o2.x = 2;
console.log(o1.x); // 2 (같은 객체를 바라봄)
```

## 5. 객체의 종류

1. 표준 내장 객체 (Built-in)

- 자바스크립트 엔진이 기본 제공
- Object, Array, Date, Math, String, RegExp 등

2. 호스트 객체 (Host Objects) - 화면에 띄워주는 것들.

- 실행 환경(브라우저, Node.js 등)에서 제공
- 브라우저: window, document
- Node.js: process, require

3. 사용자 정의 객체

- 우리가 직접 만드는 객체