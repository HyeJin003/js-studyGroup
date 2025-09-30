# 5.10 Object (객체)

## 1. 객체(Object)란?
자바스크립트는 객체(Object) 기반의 스크립트 언어이며, 자바스크립트를 이루고 있는 거의 **모든 것**이 객체이다.  
원시 타입(Primitives)을 제외한 나머지 값들(함수, 배열, 정규표현식 등)은 모두 객체이다.

객체는 **키(key)와 값(value)**으로 구성된 **프로퍼티(Property)**들의 집합이다.  
프로퍼티 값으로 자바스크립트에서 사용 가능한 모든 값을 사용할 수 있으며, 프로퍼티 값이 함수일 경우 이를 **메소드(method)**라 부른다.

> 객체 = 데이터(프로퍼티) + 동작(메소드)  
> 객체는 데이터를 참조하고 조작할 수 있는 동작을 포함하므로 데이터와 동작을 하나의 단위로 구조화할 수 있다.

자바스크립트 객체는 객체지향 상속을 위해 **프로토타입(prototype)**을 통해 프로퍼티와 메소드를 상속받을 수 있다.

---

### 1.1 프로퍼티
- **프로퍼티 키**: 문자열 또는 `symbol` 값
- **프로퍼티 값**: 모든 값 가능
- 문자열이나 `symbol` 외의 값을 키로 지정하면 **암묵적으로 문자열로 변환**  
- 중복 키 선언 시, **나중 선언한 프로퍼티가 덮어씀**  
- 객체는 프로퍼티 순서를 보장하지 않음

### 1.2 메소드
- 프로퍼티 값이 함수인 경우 **메소드(method)**라 부름
- 객체에 한정된 함수임을 의미

---

## 2. 객체 생성 방법
자바스크립트는 **프로토타입 기반 객체지향 언어**이므로 클래스 없이 객체를 생성할 수 있다.  
ES6에서 클래스가 도입되었지만, 이는 기존 프로토타입 기반의 **문법적 설탕(Syntactic sugar)**이다.
“기존 기능을 더 쉽게/보기 좋게 표현하는 문법”

### 2.1 객체 리터럴
```javascript
var emptyObject = {};
console.log(typeof emptyObject); // object

var person = {
  name: 'Lee',
  gender: 'male',
  sayHello: function () {
    console.log('Hi! My name is ' + this.name);
  }
};
```
person.sayHello(); // Hi! My name is Lee
## 2.3 생성자 함수

- 객체 생성 **템플릿**으로 활용 가능  
- `this`로 인스턴스 **프로퍼티/메소드 지정**  
- 함수 내부 일반 변수는 **private**, `this`에 바인딩된 값은 **public**

```javascript
function Person(name, gender) {
  var married = true;         // private
  this.name = name;           // public
  this.gender = gender;       // public
  this.sayHello = function(){ // public
    console.log('Hi! My name is ' + this.name);
  };
}
 
var person = new Person('Lee', 'male');

console.log(person.gender);  // 'male'
console.log(person.married); // undefined
person.sayHello();           // Hi! My name is Lee
```
만약에 일반 변수를 외부에 쓰고 싶다면 함수 호출 하면 된다 .
```javascript 
 this.getMarried = function() {
    return married;           // 내부 지역 변수 리턴
  };
  ```

## 3. 객체 프로퍼티 접근

### 3.1 프로퍼티 키
- 유효한 자바스크립트 이름이면 **따옴표 생략 가능**  
- 유효하지 않거나 예약어면 **대괄호 표기법** 사용  
- 예약어 사용 가능하지만 권장하지 않음

### 3.2 프로퍼티 값 읽기
```javascript
var person = { 'first-name': 'Ung-mo', gender: 'male' };

console.log(person['first-name']); // 'Ung-mo'
console.log(person.gender);        // 'male'
```
### 프로퍼티 값 갱신 예제

```javascript
var person = { 'first-name': 'Ung-mo', gender: 'male' };

// 프로퍼티 값 갱신
person['first-name'] = 'Kim';
console.log(person['first-name']); // 'Kim'
```
### 3.4 프로퍼티 동적 생성

```javascript
var person = { 'first-name': 'Kim', gender: 'male' };

// 새로운 프로퍼티 동적 생성
person.age = 20;
console.log(person.age); // 20
```
### 3.5 프로퍼티 삭제

```javascript
var person = { 'first-name': 'Kim', gender: 'male', age: 20 };

// 프로퍼티 삭제
delete person.gender;
console.log(person.gender); // undefined
```
### 3.6 for-in 문

```javascript
for (var prop in person) {
  console.log(prop + ': ' + person[prop]);
}
> - 객체 프로퍼티 **순서는 보장되지 않음**  
> - 배열에는 사용을 **권장하지 않음**  
> - 배열 요소만 순회하려면 ES6의 **for-of** 사용
```
## 4. Pass-by-reference

- 객체 타입(참조 타입)은 **참조값**으로 전달됨  
- 변경 가능 (**mutable**)  
- 메모리: **힙 영역 (Heap Segment)**

```javascript
var foo = { val: 10 };
var bar = foo;

bar.val = 20;
console.log(foo.val, bar.val); // 20 20
console.log(foo === bar);      // true
```
### 객체 참조 예시 (Pass-by-reference)

![객체 참조 예시](./49b22e84-e5ef-44f4-a644-f650f9916865.png)

- 변수 `foo`는 객체 `{ val: 10 }`을 참조
- 변수 `bar`와 `baz`가 같은 객체를 참조할 수도 있음
- 참조되는 객체의 값 변경 시, 모든 참조가 영향을 받음

## 5. Pass-by-value

- 원시 타입은 **값(value)**으로 전달됨  
- 변경 불가 (**immutable**)  
- 메모리: **스택 영역 (Stack Segment)**

```javascript
var a = 1;
var b = a;

a = 10;
console.log(a, b);    // 10 1
console.log(a === b); // false
```