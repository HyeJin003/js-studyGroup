 # Immutability(변경불가성) 핵심원리!!!
**한 번 생성된 데이터는 변경할 수 없는 디자인 패턴**

 객체는 **참조 형태**로 전달하고 전달 받는다.
 참조를 통해 공유 되어 있으면 > **참조를 가지고 있는 어떤 장소에서 객체를 변경하면 참조를 공유하는 모든 장소에서 그 영향을 받기 때문에 주의가 필요**하다.
┌─────────────┐
│  변수 a     │──────┐
└─────────────┘      │
                     ▼
               ┌──────────────┐
               │  객체 { x: 1 } │
               └──────────────┘
                     ▲
┌─────────────┐      │
│  변수 b     │──────┘
└─────────────┘
![alt text](image.png)

혜진이의 설명 포인트
== 변수 안에는 실제 데이터가 들어 있는 게 아니라
“이 데이터가 저장된 곳이 어디인지 가리키는 주소(pointer)”가 들어가 있다!! ==
---


## 장점
불변 객체를 사용하면 **복제나 비교를 위한 조작을 단순화**할 수 있고, **성능 개선에도 도움**이 된다.

1. **예측 가능성** — 데이터가 갑자기 바뀌지 않으니 버그 감소  
2. **디버깅 쉬움** — 언제, 어디서 바뀌었는지 추적할 필요 없음  
3. **비교·복제 용이** — 참조 비교로 변경 여부를 쉽게 판단 가능  

---

##  단점
1. **데이터가 큰 객체일수록** 매번 복사하기 때문에 **메모리 사용 증가**  
2. **변경이 잦은 경우**에는 **성능 저하** 가능  
3. **객체가 변경 가능한 데이터를 많이 가지고 있는 경우** 오히려 비효율적일 수 있음  

---

##  ES6 이후 불변성 구현이 쉬워진 이유
ES6부터는 **불변 데이터 패턴(Immutable Data Pattern)** 을 쉽게 구현할 수 있는 기능이 추가되었다.

| 방법 | 설명 |
|------|------|
| `Object.assign()` | 객체 복사 및 일부 속성 변경 |
| 스프레드 문법 (`...`) | 간결하게 복사 가능 |
| `map`, `filter`, `reduce` | 배열을 새로 만들어 리턴 |
| `const` | 변수 재할당 방지 (완전한 불변은 아님) |

### # 불변 데이터 패턴 (Immutable Data Pattern)

객체를 **불변(immutable)**으로 만들어 프로퍼티 변경을 방지하는 패턴입니다.  
객체의 값을 변경해야 할 때는 **참조가 아닌 새로운 객체를 생성**하여 변경합니다.  
이때 기존 객체는 그대로 유지되므로 안전하게 데이터를 관리할 수 있습니다.

---

## 1. 방어적 복사 (Defensive Copy)

객체를 직접 수정하지 않고, **새로운 객체를 만들어 변경**합니다.

### 예시: `Object.assign`

- 소스 객체의 프로퍼티로 타깃 객체의 동일한 프로퍼티를 덮어씀
- 리턴값: 수정된 타깃 객체
- ES6 이후 지원, Internet Explorer는 미지원

```js
const original = { name: 'Lee', age: 25 };
const copy = Object.assign({}, original); // original을 복사
copy.name = 'Kim';
console.log(original.name); // Lee
console.log(copy.name);     // Kim
```

#### - Object.assign 
 객체의 프로퍼티와 동일한 프로퍼티를 가진 타켓 객체의 프로퍼티들은 소스 객체의 프로퍼티로 덮어쓰기된다. 리턴값으로 타킷 객체를 반환한다. 
 ES6에서 추가된 메소드이며 Internet Explorer는 지원하지 않는다.


 #### - Object.freeze
 - 객체를 동결하여 프로퍼티 변경을 막음
 - 얕은(Shallow) 동결만 지원
 - 중첩 객체는 별도로 동결 필요

```js 
const user = { name: 'Lee', address: { city: 'Seoul' } };
Object.freeze(user);

user.name = 'Kim';              // 변경 안됨
user.address.city = 'Busan';    // 중첩 객체는 여전히 변경 가능

console.log(user.name);        // Lee
console.log(user.address.city); // Busan
```
## Immutable.js (Facebook 제공)  
- ==List==, ==Stack==, ==Map==, ==OrderedMap==, ==Set==, ==OrderedSet==, ==Record== 등 *영구 불변 데이터 구조(Persistent Immutable Data Structures) * 제공
- ==Object.assign==과 ==Object.freeze== 방식보다 편리하고 성능 효율적
- 대규모 애플리케이션에서 상태 관리에 유용

``` js
import { Map } from 'immutable';

const user = Map({ name: 'Lee', address: Map({ city: 'Seoul' }) });

// 불변 객체 변경
const updatedUser = user.set('name', 'Kim');
const updatedAddress = updatedUser.setIn(['address', 'city'], 'Busan');

console.log(user.get('name'));                     // Lee (원본 변경 안됨)
console.log(updatedUser.get('name'));              // Kim
console.log(user.getIn(['address', 'city']));     // Seoul
console.log(updatedAddress.getIn(['address', 'city'])); // Busan
```


# Object.freeze vs Immutable.js 비교

| 항목 | Object.freeze | Immutable.js |
|------|---------------|--------------|
| 변경 가능 여부 | 얕은 동결만 가능 (Shallow Freeze) | 완전 불변 (Deep Immutable) |
| 중첩 객체 | 중첩 객체는 여전히 변경 가능 | 중첩 객체까지 모두 불변 |
| 성능 | 객체 크기가 크거나 깊으면 복사 비용이 발생 | 효율적인 구조로 최적화, 큰 데이터에도 효율적 |
| 문법/사용성 | 내장 메서드 (`Object.freeze`) 사용 | 별도 라이브러리 설치 필요 (`Immutable.List`, `Map` 등) |
| 장점 | 간단하고 내장 기능 | 대규모 애플리케이션에서 안전한 상태 관리, 편리한 API |
| 단점 | 깊은 불변 처리 번거로움 | 외부 라이브러리 의존, 러닝 커브 있음 |

###  요약

- **작은 객체**나 **간단한 불변 처리**: `Object.freeze` 적합  
- **복잡한 상태 관리**나 **중첩 객체가 많은 대규모 앱**: `Immutable.js` 추천
