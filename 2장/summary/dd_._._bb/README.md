# 2. 타입

## 2-1. 타입이란

### 2-1-1. 자료형으로서의 타입

- 변수는 값을 저장할 수 있는 **메모리 공간**이며, 값을 가리키는 **상징적인 이름**이다.
- 개발자는 변수를 선언하고, 특정 값을 변수에 할당한다.
- 컴퓨터 메모리는 한정적이므로, 값을 효율적으로 저장하려면 **값의 크기**를 알아야 한다.
- 데이터 타입은 값의 크기를 명시하여, 컴퓨터가 값을 참조할 때 **적절한 메모리 크기**를 읽도록 돕는다.
- 데이터 타입 체계는 모든 데이터를 해석하거나 처리할 때 활용된다.
- 변수에 저장할 수 있는 값의 크기와 데이터 타입 체계는 **프로그래밍 언어마다 다르다**.
- 자바스크립트는 7가지 데이터 타입을 제공하며, 이를 통해 값의 종류를 명시할 수 있다.
  - **자바스크립트의 7가지 데이터 타입**: `string`, `number`, `boolean`, `null`, `undefined`, `symbol`, `object`.
  - 타입스크립트는 자바스크립트에 타입 시스템을 추가하여, 코드의 안정성과 가독성을 높인다.

```js
// 2.1.1-2.js
// 문자열(string) 타입의 데이터를 변수 name에 할당
const name = "zig"; // name 변수는 "zig"라는 문자열 값을 저장

// 숫자(number) 타입의 데이터를 변수 year에 할당
const year = 2022; // year 변수는 2022라는 숫자 값을 저장
```

<br>

### 2-1-2. 집합으로서의 타입

- 타입은 수학에서의 집합과 유사한 개념으로, **값이 가질 수 있는 유효 범위(가능한 값의 집합)**를 정의합니다.
- **타입 시스템은 코드에서 사용되는 값의 유효 범위를 제한**하여, 잘못된 값이 사용되는 것을 방지하고 런타임 오류를 줄이는 데 도움을 줍니다.
- TypeScript와 같은 정적 타입 언어에서는 함수 인자나 변수에 타입을 명시적으로 정의할 수 있습니다.  
  - TypeScript는 정적 타입 시스템을 통해 컴파일 단계에서 오류를 잡아줍니다.
  - JavaScript는 동적 타입 언어로, 타입을 명시하지 않기 때문에 런타임 오류가 발생할 가능성이 더 높습니다.

**[타입을 정의한 경우]**

```ts
// 2.1.2-1.ts
// 변수에 타입을 명시적으로 정의
const num: number = 123; // 숫자 타입
const str: string = "abc"; // 문자열 타입

// 함수의 매개변수에 타입을 정의
function func(n: number) {
  // n은 반드시 숫자 타입이어야 함
  console.log(n);
}

// 올바른 호출
func(num); // 정상 작동

// 잘못된 호출
func(str); 
// 🚨 오류: 'string' 타입은 'number' 타입의 매개변수에 할당할 수 없습니다.
```

**[타입을 정의하지 않은 경우]**

```js
// 2.1.2-2.js
// 함수의 매개변수에 타입을 정의하지 않음
function double(n) {
  return n * 2; // n이 숫자가 아니면 NaN 반환 가능
}

// 올바른 호출
console.log(double(2)); // 4

// 잘못된 호출
console.log(double("z")); 
// ❌ NaN: 문자열을 곱셈 연산에 사용했기 때문
```

**[타입을 정의한 경우]**

```ts
// 2.1.2-3.ts
// 함수의 매개변수에 타입을 정의
function double(n: number): number {
  return n * 2; // n은 반드시 숫자 타입이어야 함
}

// 올바른 호출
console.log(double(2)); // 4

// 잘못된 호출
console.log(double("z")); 
// 🚨 오류: 'string' 타입은 'number' 타입의 매개변수에 할당할 수 없습니다. (에러 코드: 2345)
```

<br>

### 2-1-3. 정적타입과 동적타입

- 타입을 결정하는 시점에 따라 프로그래밍 언어는 **정적 타입**과 **동적 타입**으로 구분됩니다.
- 1. **정적 타입(Static Typing)**:
  - **특징**:
    - 모든 변수의 타입이 **컴파일 단계**에서 결정됩니다.
    - 변수 선언 시 타입을 명시해야 하므로 초기 개발 속도가 느릴 수 있습니다.
    - 컴파일 타임에 타입 에러를 발견할 수 있어, 프로그램의 안정성을 높일 수 있습니다.
  - **관련 프로그래밍 언어**:
    - Java, C, TypeScript 등
- 2. **동적 타입 (Dynamic Typing)**:
  - **특징**:
    - 변수의 타입이 **런타임**에 결정됩니다.
    - 개발자가 변수의 타입을 명시할 필요가 없습니다.
    - 런타임 에러가 발생할 가능성이 높아질 수 있습니다.
  - **관련 프로그래밍 언어**:
    - JavaScript, Python 등
  
```js
// 2.1.3-1.js
// ㄴ 동적 타입 언어에서 런타임 에러가 발생할 수 있는 사례
function multiplyByThree(number) { // `multiplyByThree` 함수는 매개변수 `number`의 타입을 명시하지 않았습니다.
  return number * 3;
}

console.log(multiplyByThree(10)); // 30 (정상 동작)
console.log(multiplyByThree("f")); // NaN (문자열과 숫자 연산으로 인해 Not-a-Number 발생)
```

<br>

### 2-1-4. 강타입과 약타입

1. **모든 프로그래밍 언어에는 값의 타입이 존재**하며, 타입은 명시적이거나 암묵적으로 결정됩니다.
2. **암묵적 타입 변환** 여부에 따라 타입 시스템을 **강타입(Strong Typing)**과 **약타입(Weak Typing)**으로 분류할 수 있습니다.
   - **강타입(Strong Typing)**: 서로 다른 타입의 값 간 연산 시 오류가 발생.
   - **약타입(Weak Typing)**: 서로 다른 타입의 값 간 연산 시 암묵적 타입 변환을 통해 연산이 수행됨.
3. **타입 시스템**은 프로그램에서 타입을 할당하는 규칙 집합으로, 크게 두 가지로 나뉩니다:
   - **명시적 타입 시스템**: 개발자가 타입을 명시적으로 지정.
   - **(암묵적) 타입 추론 시스템**: 컴파일러 또는 인터프리터가 타입을 자동으로 추론.

**[강타입 vs 약타입 언어] 코드 비교**

- 강타입 언어 (예: Python, Java)
  
```python
# Python (강타입)
a = "10"  # 문자열
b = 5     # 정수

# 서로 다른 타입의 값 연산 시 오류 발생
result = a + b  # 🚨 TypeError: can only concatenate str (not "int") to str
```

```java
// Java (강타입)
String a = "10"; // 문자열
int b = 5;       // 정수

// 서로 다른 타입의 값 연산 시 컴파일 오류 발생
int result = a + b; // 🚨 Error: incompatible types
```

- 약타입 언어 (예: JavaScript, PHP)

```javascript
// JavaScript (약타입)
let a = "10"; // 문자열
let b = 5;    // 정수

// 암묵적 타입 변환 후 연산 수행
let result = a + b; // 결과: "105" (문자열로 변환)
```

```php
// PHP (약타입)
$a = "10"; // 문자열
$b = 5;    // 정수

// 암묵적 타입 변환 후 연산 수행
$result = $a + $b; // 결과: 15 (정수로 변환)
```

**[요약 비교]**

| **특징**            | **강타입 언어**                     | **약타입 언어**                     |
|---------------------|------------------------------------|------------------------------------|
| **타입 검사 시점**   | 컴파일 또는 런타임 시 엄격히 검사      | 암묵적 변환을 통해 유연하게 처리       |
| **타입 변환**        | 명시적 변환 필요                     | 암묵적 변환 가능                     |
| **오류 발생 여부**   | 타입 불일치 시 오류 발생               | 타입 불일치 시 오류 없이 변환 수행       |
| **예시 언어**        | Python, Java, C#                  | JavaScript, PHP, Perl             |


<br>

### 2-1-5. 컴파일 방식

1. **컴파일의 정의**:  
   - 컴파일은 고수준 언어(사람이 이해하기 쉬운 언어)로 작성된 코드를 저수준 언어(컴퓨터가 이해할 수 있는 언어)로 변환하는 과정입니다.  
   - 변환 결과는 바이너리 코드(0과 1로 이루어진 코드)입니다.

2. **타입스크립트의 특징**:  
   - 타입스크립트는 일반적인 컴파일러와 다르게, 컴파일 결과가 사람이 읽을 수 있는 자바스크립트 파일로 변환됩니다.  
   - 이는 타입스크립트가 자바스크립트의 런타임 에러를 사전에 방지하기 위해 설계되었기 때문입니다.

<br>

## 2-2. 타입스크립트의 타입 시스템


### 2-2-1. 타입 어노테이션 방식

```ts
let isDone: boolean = false;
let decimal: number = 6;
let color: string = "blue";
let list: number[] = [1, 2, 3];
let x: [string, number]; // tuple
```

1. **타입 어노테이션(Type Annotation) 개념**: 변수, 상수, 함수의 인자 또는 반환 값에 대해 타입을 명시적으로 선언하여 컴파일러에 저장될 값의 타입을 알려주는 문법.
2. **타입스크립트의 특징**:
   1. **타입 어노테이션(Type Annotation)의 방식**: 변수 뒤에 `: type` 구문을 붙여 데이터 타입을 명시.
   2. 기존 자바스크립트 코드에 점진적으로 타입을 적용할 수 있음.
   3. 언어마다 타입을 명시하는 방식이 다름.
      - 타입 어노테이션 방식은 언어마다 다르지만, 공통적으로 코드의 가독성과 안정성을 높이는 데 기여합니다.

**[언어별 타입 어노테이션 비교]**

**타입스크립트 (TypeScript)**

```ts
let age: number = 25; // 변수 뒤에 : 타입을 명시
function greet(name: string): string {
  return `Hello, ${name}`;
}
```

**파이썬 (Python, 타입 힌팅 사용)**

```python
age: int = 25  # 변수 뒤에 : 타입을 명시
def greet(name: str) -> str:
    return f"Hello, {name}"
```

<br>

### 2-2-2. 구조적 타이핑(Structural Typing)

1. **명목적 타입 시스템**:
   - 대부분의 언어에서 값이나 객체는 하나의 구체적인 타입을 가짐.
   - 타입은 이름으로 구분되며, 런타임 컴파일 이후에도 타입 정보가 유지됨.
   - 명확한 상속 관계나 공통 인터페이스가 없으면 타입 간 호환이 불가능.

2. **타입스크립트의 구조적 타이핑**:
   - 타입스크립트는 이름이 아닌 **구조**로 타입을 구분.
   - 객체의 속성과 구조가 동일하다면, 서로 다른 타입이라도 호환 가능.
   - 이를 **구조적 타이핑(Structural Typing)**이라 부름.


```ts
interface Developer {
  faceValue: number;
}

interface BankNote {
  faceValue: number;
}

let developer: Developer = { faceValue: 52 };
let bankNote: BankNote = { faceValue: 10000 };

developer = bankNote; // ✅ OK
bankNote = developer; // ✅ OK
// Developer`와 `BankNote`라는 두 인터페이스가 동일한 구조(`faceValue: number`)를 가지므로, 서로 다른 타입임에도 값 할당이 가능.
```

```ts
interface Rectangle {
  width: number;
  height: number;
}

interface Box {
  width: number;
  height: number;
}

let rect: Rectangle = { width: 10, height: 20 };
let box: Box = { width: 15, height: 25 };

rect = box; // ✅ OK
box = rect; // ✅ OK
// `Rectangle`과 `Box`는 이름이 다르지만, 구조가 동일하므로 서로 할당이 가능.
```

```ts
interface Circle {
  radius: number;
}

let circle: Circle = { radius: 10 };

// rect = circle; // 🚨 Error: 구조가 다르므로 호환되지 않음
// circle = rect; // 🚨 Error: 구조가 다르므로 호환되지 않음
// `Rectangle`과 `Circle`은 구조가 다르기 때문에 서로 할당이 불가능.
```

```ts
type Add = (a: number, b: number) => number;
type Subtract = (x: number, y: number) => number;

let add: Add = (a, b) => a + b;
let subtract: Subtract = (x, y) => x - y;

add = subtract; // ✅ OK
subtract = add; // ✅ OK
// 함수의 매개변수와 반환 타입이 동일하다면, 이름이 달라도 할당이 가능.
```

<br>

### 2-2-3. 구조적 서브타이핑 (Structural Subtyping)

- 구조적 서브타이핑은 객체의 **구조(속성)**를 기준으로 타입을 판단하는 방식입니다.  
- 이름이 다른 타입이라도 **구조가 동일**하다면, 서로 호환 가능한 타입으로 간주됩니다.
- **타입 계층 구조(상속 관계)에 얽매이지 않고, 객체의 속성만으로 판단합니다.**
- 타입 간의 호환성은 **타입 이름**이 아닌 **구조**에 의해 결정됩니다.  

**예시 코드**

- **속성 기반 타입 호환성**:
  ```ts
  interface Pet {
    name: string;
  }

  interface Cat {
    name: string;
    age: number;
  }

  let pet: Pet;
  let cat: Cat = { name: "Zag", age: 2 };

  // ✅ OK: Cat의 구조가 Pet의 구조를 포함하므로 호환 가능
  pet = cat;
  ```

- **함수 매개변수에서의 구조적 타이핑**:
  ```ts
  interface Pet {
    name: string;
  }

  let cat = { name: "Zag", age: 2 };

  function greet(pet: Pet) {
    console.log(`Hello, ${pet.name}`);
  }

  // ✅ OK: cat의 구조가 Pet의 구조를 포함하므로 함수 호출 가능
  greet(cat);
  ```

  ```ts
  interface Rectangle {
    width: number;
    height: number;
  }

  interface Square {
    width: number;
    height: number;
    color: string;
  }

  let rect: Rectangle;
  let square: Square = { width: 10, height: 10, color: "blue" };

  // ✅ OK: Square의 구조가 Rectangle의 구조를 포함하므로 호환 가능
  rect = square;

  function calculateArea(shape: Rectangle) {
    return shape.width * shape.height;
  }

  // ✅ OK: Square의 구조가 Rectangle의 구조를 포함하므로 함수 호출 가능
  console.log(calculateArea(square)); // 100
  ```

- **클래스 간의 구조적 서브타이핑**:
  ```ts
  class Person {
    name: string;
    age: number;

    constructor(name: string, age: number) {
      this.name = name;
      this.age = age;
    }
  }

  class Developer {
    name: string;
    age: number;
    sleepTime: number;

    constructor(name: string, age: number, sleepTime: number) {
      this.name = name;
      this.age = age;
      this.sleepTime = sleepTime;
    }
  }

  function greet(p: Person) {
    console.log(`Hello, I'm ${p.name}`);
  }

  const developer = new Developer("zig", 20, 7);

  // ✅ OK: Developer의 구조가 Person의 구조를 포함하므로 호환 가능
  greet(developer); // Hello, I'm zig
  ```

<br>

### 2-2-4. 자바스크립트를 닮은 타입스크립트

1. **타입스크립트의 구조적 타이핑**: 
   - 타입스크립트는 객체나 함수의 **구조**를 기반으로 타입을 검사합니다. 이는 자바스크립트의 동작(덕 타이핑)을 모델링한 결과입니다.
   - 타입스크립트는 자바스크립트의 덕 타이핑 개념을 모델링하면서도 정적 타이핑의 장점을 제공합니다.
2. **덕 타이핑**: "어떤 값이 올바르게 동작한다면, 그 값이 어떻게 만들어졌는지는 중요하지 않다"는 개념으로, 런타임에 타입을 검사합니다.
3. **구조적 타이핑 vs 덕 타이핑**:
   - **구조적 타이핑**: 컴파일 타임에 타입을 검사 (정적 타이핑).
   - **덕 타이핑**: 런타임에 타입을 검사 (동적 타이핑).
4. **공통점**: 두 방식 모두 객체의 **속성**을 기반으로 타입을 검사합니다.

**자바스크립트의 덕 타이핑 예제**:

```js
function printName(obj) {
  console.log(obj.name);
}

const person = { name: "Alice", age: 25 };
printName(person); // 출력: Alice
```

- `printName` 함수는 `obj`가 `name` 속성을 가지고 있다면 문제없이 동작합니다.
- `obj`가 어떤 타입인지, `person` 객체가 어떻게 정의되었는지는 중요하지 않습니다. 런타임에 `name` 속성이 있는지만 확인합니다.

**타입스크립트의 구조적 타이핑 예제**:

```ts
type Person = { name: string; age: number };

function printName(obj: Person) {
  console.log(obj.name);
}

const person = { name: "Alice", age: 25 };
printName(person); // 출력: Alice
```

- 타입스크립트는 `Person` 타입을 정의하고, `printName` 함수의 매개변수로 이를 요구합니다.
- 컴파일 타임에 `person` 객체가 `Person` 타입과 구조적으로 일치하는지 검사합니다.

**구조적 타이핑의 유연성**:

```ts
type Person = { name: string };

const person = { name: "Alice", age: 25 }; // 추가 속성이 있어도 문제없음
const anotherPerson = { name: "Bob" };

function printName(obj: Person) {
  console.log(obj.name);
}

printName(person); // 출력: Alice
printName(anotherPerson); // 출력: Bob
```

- `person` 객체는 `Person` 타입에 요구되지 않는 `age` 속성을 추가로 가지고 있지만, 타입스크립트는 이를 허용합니다. 중요한 것은 `name` 속성이 존재하고 타입이 맞는지입니다.

<br>

### 2-2-5. 구조적 타이핑의 예상치 못한 결과

- 구조적 타이핑은 객체의 형태(구조)에 따라 타입을 판단하는 방식으로, 추가 속성이 있는 객체도 허용될 수 있습니다. 이로 인해 타입 안정성이 떨어질 수 있는 상황이 발생할 수 있습니다.

```ts
interface Cube {
  width: number;
  height: number;
  depth: number;
}

function addLines(c: Cube) {
  let total = 0;
  for (const axis of Object.keys(c)) {
    // `Object.keys()`로 얻은 키는 문자열 배열(`string`)로 간주되며, 이는 타입스크립트가 `Cube` 타입의 키로 보장하지 않음. 따라서 `c[axis]` 접근 시 타입 에러가 발생.
    const length = c[axis]; // 🚨 타입 에러 발생
    total += length;
  }
}

const namedCube = {
  width: 6,
  height: 5,
  depth: 4,
  name: "SweetCube", // 추가 속성
};

// 추가 속성이 있는 객체(`namedCube`)를 함수에 전달해도 타입스크립트는 이를 허용함.
// 이는 구조적 타이핑의 유연성으로 인해 발생.
addLines(namedCube); // ✅ OK (구조적 타이핑으로 인해 허용)
```

```ts
type Cube = {
  kind: "cube"; // 식별 가능한 속성 추가
  width: number;
  height: number;
  depth: number;
};

function addLines(c: Cube) {
  let total = 0;
  for (const axis of ["width", "height", "depth"] as const) {
    const length = c[axis]; // ✅ 타입 안전
    total += length;
  }
}

const namedCube = {
  kind: "cube",
  width: 6,
  height: 5,
  depth: 4,
  name: "SweetCube", // 추가 속성
};

addLines(namedCube); // 🚨 타입 에러 (추가 속성 허용 안 됨)
```

- 이러한 한계를 극복하고자 타입스크립트에 명목적 타이핑 언어의 특징을 가미한 식별할 수 있는 유니온 과 같은 방법이 생겼다.

<br>

### 2-2-6. 타입스크립트의 점진적 타입 확인

<br>

### 2-2-7. 자바스크립트 슈퍼셋으로서의 타입스크립트

<br>

### 2-2-8. 값 vs 타입

<br>

### 2-2-9. 타입을 확인하는 방법

<br>


## 2-3. 원시 타입


### 2-3-1. `boolean`

<br>

### 2-3-2. `undefined`

<br>

### 2-3-3. `null`

<br>

### 2-3-4. `number`

<br>

### 2-3-5. `bigInt`

<br>

### 2-3-6. `string`

<br>

### 2-3-7. `symbol`

<br>

## 2-4. 객체 타입

### 2-4-1. `object`

<br>

### 2-4-2. `{}`

<br>

### 2-4-3. `array`

<br>

### 2-4-4. `type`과 `interface` 키워드

<br>

### 2-4-5. `function`

<br>