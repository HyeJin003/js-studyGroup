# 제어문(Control Flow Statement)

제어문은 코드의 실행 순서를 조건에 따라 제어하거나 반복 실행할 때 사용한다. 일반적으로 코드는 위에서 아래로 순차적으로 실행된다.

---

## 1. 블록문(Block Statement)

- 0개 이상의 문을 중괄호 `{}`로 묶은 코드 블록
- 단독으로 사용 가능하지만, 주로 제어문이나 함수 선언 등에서 사용
- 끝에 세미콜론(;)을 붙이지 않는다

```
// 블록문
{
  var foo = 10;
  console.log(foo);
}

// 제어문
var x = 0;
while (x < 10) {
  x++;
}
console.log(x); // 10

// 함수 선언문
function sum(x, y) {
  return x + y;
}
console.log(sum(1, 2)); // 3
```

## 2. 조건문 (Conditional Statement)

### 2.1 if…else 문

- 조건식이 참이면 `if` 블록 실행, 거짓이면 `else` 블록 실행
- 조건식이 여러 개라면 `else if` 사용 가능
- 블록 내 문이 하나뿐이면 중괄호 `{}` 생략 가능
- 삼항 조건 연산자로 대체 가능

```
var num = 2;
var kind;

// if…else if…else 문
if (num > 0)
  kind = '양수';
else if (num < 0)
  kind = '음수';
else
  kind = '영';

console.log(kind); // 양수

// 삼항 조건 연산자로 대체 가능
var kind2 = num ? (num > 0 ? '양수' : '음수') : '영';
console.log(kind2); // 양수
```

### 2.2 switch 문

- `switch` 문은 표현식의 평가 결과와 일치하는 `case` 문으로 실행 순서를 이동
- `break` 문으로 폴스루(fall through) 방지
- `default` 문은 선택 사항이며 보통 마지막에 위치
- 여러 `case` 문을 중첩하여 사용할 수도 있음 (예: 윤년 계산)

```
// 월을 영어로 변환
var month = 11;
var monthName;

switch (month) {
  case 1: monthName = 'January'; break;
  case 2: monthName = 'February'; break;
  case 3: monthName = 'March'; break;
  case 4: monthName = 'April'; break;
  case 5: monthName = 'May'; break;
  case 6: monthName = 'June'; break;
  case 7: monthName = 'July'; break;
  case 8: monthName = 'August'; break;
  case 9: monthName = 'September'; break;
  case 10: monthName = 'October'; break;
  case 11: monthName = 'November'; break;
  case 12: monthName = 'December'; break;
  default: monthName = 'Invalid month';
}

console.log(monthName); // November
```

여러 case 문 중첩 (윤년 계산 예시)

```
var year = 2000, month = 2, days = 0;

switch (month) {
case 1: case 3: case 5: case 7: case 8: case 10: case 12:
days = 31; break;
case 4: case 6: case 9: case 11:
days = 30; break;
case 2:
days = ((year % 4 === 0 && year % 100 !== 0) || year % 400 === 0) ? 29 : 28;
break;
default:
console.log('Invalid month');
}

console.log(days); // 29

```

### 3. 반복문 (Loop Statement)

#### 3.1 for 문

- 초기화식, 조건식, 증감식을 이용하여 반복 수행
- 조건식이 거짓이 될 때까지 반복
- 중첩 사용 가능

기본 for 문

```
for (var i = 0; i < 2; i++) {
  console.log(i); // 0 1
}
```

역순 반복

```
for (var i = 1; i >= 0; i--) {
  console.log(i); // 1 0
}
```

중첩 for 문: 두 주사위 합이 6인 경우 출력

```
for (var i = 1; i <= 6; i++) {
  for (var j = 1; j <= 6; j++) {
    if (i + j === 6) console.log(`[${i}, ${j}]`);
  }
}
/* 출력 결과
[1, 5]
[2, 4]
[3, 3]
[4, 2]
[5, 1]
*/
```

### 3.2 while 문

- 조건식이 **참인 동안 반복 실행**
- 조건식이 **거짓이면 종료**
- 무한루프 가능, **break**로 탈출 가능

```
var count = 0;
```

기본 while 문

```
while (count < 3) {
  console.log(count); // 0 1 2
  count++;
}
```

무한 루프 + break

```
count = 0;
while (true) {
  if (count === 3) break; // 탈출 조건
  console.log(count); // 0 1 2
  count++;
}
```

### 3.3 do…while 문

- 코드 블록을 **먼저 실행**하고 조건식 평가
- 최소 **1회 실행 보장**

```
var count = 0;

do {
  console.log(count); // 0 1 2
  count++;
} while (count < 3);
```

### 4. break 문

- 반복문, `switch`, 레이블 블록을 **즉시 탈출**
- 레이블 지정 가능 (중첩 반복문 탈출 시 유용)

레이블 사용 예

```
outer: for (var i = 0; i < 3; i++) {
 for (var j = 0; j < 3; j++) {
   if (i + j === 3) break outer;
 }
}
console.log('Done!'); // Done!
```

```
var string = 'Hello World.', index;
for (var i = 0; i < string.length; i++) {
  if (string[i] === 'l') {
    index = i;
    break;
  }
}
console.log(index); // 2
```

### 5. continue 문

- 반복문의 **현재 반복**을 중단하고, **증감식으로 이동**
- 반복문을 **탈출하지는 않음**

```
var string = 'Hello World.', count = 0;

for (var i = 0; i < string.length; i++) {
  // 'l'이 아니면 건너뛰고 다음 반복으로
  if (string[i] !== 'l') continue;
  count++; // 'l'일 경우에만 실행
}

console.log(count); // 3
```
