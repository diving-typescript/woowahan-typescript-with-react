# 6.1 자바스크립트의 런타임과 타입스크립트의 컴파일

## 런타임과 컴파일타임

- **컴파일 타임** : 소스코드가 컴파일 과정을 거쳐 컴퓨터가 인식할 수 있는 기계어코드(바이트 코드)로 변환되어 실행할 수 있는 프로그램이 되는 과정

- **런타임** : 컴파일 과정을 마친 응용 프로그램이 사용자에 의해 실행되는 과정

## 자바스크립트 런타임

대표적인 자바스크립트 런타임으로 크롬이나 사파리 같은 인터넷 브라우저와 Node.js 등이 있다.

자바스크립트 런타임은 다양한 구성 요소로 이루어져 있는데 주요 구성 요소로 자바스크립트 엔진,
웹 API, 콜백 큐, 이벤트 루프, 렌더 큐가 있다.

## 타입스크립트의 컴파일

타입스크립트는 tsc라고 불리는 컴파일러를 통해 자바스크립트 코드로 변환되는데 사실 고수준 언어(타입스크립트)가 또 다른 고수준 언어(자바스크립트)로 변환되는 것이기 때문에 컴파일이 아닌 트랜스파일이라고 부르기도 한다.

또한 이러한 변환 과정은 소스코드를 다른 소스코드로 변환하는 것이기에 타입스크립트 컴파일러를 *소스 대 소스 컴파일러*라고 지칭하기도 한다.

타입스크립트 컴파일러는 소스코드를 해석하여 **AST**(최소 구문 트리)를 만들고 이후 타입확인을 거친 다음에 결과 코드를 생성한다.

#### 프로그램이 실행되기까지의 과정

1. 타입스크립트 소스코드를 타입스크립트 AST로 만든다. (tsc)
2. 타입 검사기가 AST를 확인하여 타입을 확인한다. (tsc)
3. 타입스크립트 AST를 자바스크립트 소스로 변환한다.(tsc)
4. 자바스크립트 소스코드를 자바스크립트 AST로 만든다. (런타임)
5. AST가 바이트 코드로 변환된다. (런타임)
6. 런타임에서 바이트 코드가 평가되어 프로그램이 실행된다. (런타임)

> **AST**(Abstract Syntax Tree)
> 컴파일러가 소스코드를 해석하는 과정에서 생성된 데이터 구조다. 컴파일러는 어휘적 분석과 구문 분석을 통해 소스코드를 노드 단위의 트리 구조로 구성한다.

이때 타입스크립트 소스코드의 타입은 1~2단계에서만 사용되고 3단계부터는 타입을 확인하지 않는다. 즉, 개발자가 작성한 타입 정보는 단지 타입을 확인하는 데만 쓰이며, 최종적으로 만들어지는 프로그램에는 아무런 영향을 주지 않는다.

타입스크립트는 컴파일타임에 타입을 검사하기 때문에 에러가 발생하면 프로그램이 실행되지 않는다. 이러한 특징 때문에 타입스크립트를 컴파일타임에 에러를 발견할 수 있는 *정적 타입 검사기*라고 부른다.

<br>

# 6.2 타입스크립트 컴파일러의 동작

## 코드 검사기로서의 타입스크립트 컴파일러

타입스크립트는 컴파일타임에 문법 에러와 타입 관련 에러를 모두 검출한다.

<details>
  <summary>
    예제
  </summary>

```js
const developer = {
  work() {
    console.log("working...");
  },
};

developer.work(); // working...
developer.sleep(); // TypeError: developer.sleep is not a function
```

이 코드를 자바스크립트로 작성하는 시점에서는 에러가 발생하지 않지만, 실제 실행을 하면 에러가 발생한다.

```ts
const developer = {
  work() {
    console.log("working...");
  },
};

developer.work(); // working...
developer.sleep(); // Property ‘sleep’ does not exist on type ‘{ work(): void;}’
```

타입스크립트는 위와같이 코드를 실행하기 전 사전에 에러를 발견하여 알려준다.

</details>

## 코드 변환기로서의 타입스크립트 컴파일러

타입스크립트 컴파일러는 타입을 검사한 다음 타입스크립트 코드를 각자의 런타임 환경에서 동작할 수 있도록 구버전의 자바스크립트로 트랜스파일한다.

<details>
  <summary>
    예제
  </summary>

```ts
type Fruit = "banana" | "watermelon" | "orange" | "apple" | "kiwi" | "mango";

const fruitBox: Fruit[] = ["banana", "apple", "mango"];

const welcome = (name: string) => {
  console.log(`hi! ${name} :)`);
};
```

타입스크립트 컴파일러의 `target` 옵션을 사용하여 ES5버전의 자바스크립트 소스코드로 컴파일하면 아래와 같은 코드가 생성된다.

```js
"use strict";

var fruitBox = ["banana", "apple", "mango"];

var welcome = function (name) {
  console.log("hi! ".concat(name, " :)"));
};
```

트랜스파일이 완료된 자바스크립트 파일에서 타입 정보가 제거된 것을 볼 수 있다.

</details>

타입스크립트 코드가 자바스크립트 코드로 변환되는 과정은 타입 검사와 독립적으로 동작하기 때문에 타입스크립트 컴파일러는 타입 검사를 수행한 후 코드 변환을 시작한다. 이때, 타입스크립트 코드의 타이핑이 잘못되어 발생하는 에러는 자바스크립트 실행 과정에서 런타임 에러로 처리된다.

자바스크립트는 타입정보를 이해하지 못하기때문에 타입스크립트 소스코드에 타입 에러가 있더라도 자바스크립트로 컴파일되어 타입 정보가 모두 제거된 후에는 타입이 아무런 효력을 발휘하지 못한다.

<details>
  <summary>예제</summary>

```ts
const name: string = "zig";
// Type ‘string’ is not assignable to type ‘number’
const age: number = "zig";
```

`age`변수를 `number`타입으로 선언하고 문자열 "zig"를 할당하면 타입 에러가 발생하지만 자바스크립트로 컴파일할 수는 있다.

컴파일 후의 결과는 아래와 같다.

```ts
const name = "zig";
const age = "zig";
```

</details>

컴파일 된 코드가 실행되고 있는 런타임에서는 타입 검사를 할 수 없기 때문에 주의해야 하는 경우도 있다.

<details>
  <summary>예제</summary>

```ts
interface Square {
  width: number;
}

interface Rectangle extends Square {
  height: number;
}

type Shape = Square | Rectangle;

function calculateArea(shape: Shape) {
  if (shape instanceof Rectangle) {
    // ‘Rectangle’ only refers to a type, but is being used as a value here
    // Property ‘height’ does not exist on type ‘Shape’
    // Property ‘height’ does not exist on type ‘Square’
    return shape.width * shape.height;
  } else {
    return shape.width * shape.width;
  }
}
```

예제에서 `Square`와 `Rectangle`은 인터페이스로, 타입스크립트에서만 존재한다. 이들은 자바스크립트 런타임에서는 실제 객체가 아니며, 따라서 `instanceof` 같은 런타임 체크에 사용될 수 없다.

다시말하자면, `instanceof` 체크는 런타임에 실행되지만 `Rectangle`은 타입이기 때문에 자바스크립트 런타임은 해당 코드를 이해하지 못한다. 타입스크립트 코드가 자바스크립트로 컴파일되는 과정에서 모든 인터페이스, 타입, 타입 구문이 제거되어 버리기 때문에 런타임에서는 타입을 사용할 수 없는 것이다.

</details>

<br>

# 6.3 타입스크립트 컴파일러의 구조

컴파일러는 하나의 프로그램으로 이를 구현한 소스 파일이 존재한다. 타입스크립트 공식 깃허브에서 *compiler*라는 별도의 폴더로 구성된 타입스크립트 컴파일러를 찾아볼 수 있다. 해당 폴더는 타입스크립트 컴파일러가 동작하는 데 중요한 몇 가지 중요한 구성 요소를 가지고 있다.

#### 타입스크립트 컴파일러의 실행 과정

![](https://velog.velcdn.com/images/orodae/post/bf709d6d-1c65-4b30-b855-b20d1a433077/image.png)

_이미지 출처: "우아한 타입스크립트 with 리액트", 우아한형제들, 한빛미디어, 2023, p. 192._

## 프로그램(Program)

타입스크립트 컴파일러는 tsc 명령어로 실행되며, 컴파일러는 `tsconfig.json`에 명시된 컴파일 옵션을 기반으로 컴파일을 수행한다.

먼저 전체적인 컴파일 과정을 관리하는 프로그램 객체(인스턴스)가 생성된다.
이 프로그램 객체는 컴파일할 타입스크립트 소스 파일과 소스 파일 내에서 임포트된 파일을 불러오는데, 가장 최초로 불러온 파일을 기준으로 컴파일 과정이 시작된다.

## 스캐너(Scanner)

스캐너는 타입스크립트 소스코드를 작은 단위로 나누어 의미 있는 토큰으로 변환하는 작업을 수행한다.

```ts
const woowa = "bros";
```

위 코드는 스캐너에 의해 다음과 같이 분석된다.

![](https://velog.velcdn.com/images/orodae/post/e4cb315d-025d-4799-8b23-acb5d2e10621/image.png)

_이미지 출처: "우아한 타입스크립트 with 리액트", 우아한형제들, 한빛미디어, 2023, p. 193._

## 파서(Parser)

스캐너가 소스 파일을 토큰으로 나눠주면 파서는 그 토큰 정보를 이용하여 AST를 생성한다.

AST는 컴파일러가 동작하는 데 핵심 기반이 되는 자료 구조로, 소스코드의 구조를 트리 형태로 표현하다. AST의 최상위 노드는 타입스크립트 소스 파일이며, 최하위 노드는 파일의 끝 지점으로 구성된다.

스캐너가 어휘적 분석을 통해 토큰 단위로 소스코드를 나눈다면, 파서는 이렇게 생성된 토큰 목록을 활용하여 구문적 분석을 수행한다고 볼 수 있으며 이를 통해 코드의 실질적인 구조를 노드 단위의 트리 형태로 표현하는 것이다.

각각의 노드는 코드상의 위치, 구문 종류, 코드 내용과 같은 정보를 담고있다.

예를 들어 ( )에 해당하는 토큰이 있을 때 파서가 AST를 생성하는 과정에서 이 토큰이 실질적으로 함수의 호출인지, 함수의 인자인지 또는 그룹 연산자인지가 결정된다.

<details>
  <summary>예제</summary>

```ts
function normalFunction() {
  console.log("normalFunction");
}

normalFunction();
```

![](https://velog.velcdn.com/images/orodae/post/9be70617-2a23-4f08-a126-edfb1c0e6dae/image.png)

_이미지 출처: "우아한 타입스크립트 with 리액트", 우아한형제들, 한빛미디어, 2023, p. 194._

</details>

## 바인더(Binder)

바인더의 주요 역할은 체커 단계에서 타입 검사를 할 수 있도록 기반을 마련하는 것이다. 바인더는 타입 검사를 위해 심볼이라는 데이터 구조를 생성한다. 심볼은 이전 단계의 AST에서 선언된 타입의 노드 정보를 저장한다.

심볼의 인터페이스 일부는 다음과 같이 구성된다.

<details>
  <summary>예제</summary>

```ts
export interface Symbol {
  flags: SymbolFlags; // Symbol flags

  escapedName: string; // Name of symbol

  declarations?: Declaration[]; // Declarations associated with this symbol

  // 이하 생략...
}
```

`flag` 필드는 AST에서 선언된 타입의 노드 정보를 저장하는 식별자이다. 심볼을 구분하는 식별자 목록은 다음과 같다.

```ts
// src/compiler/types.ts
export const enum SymbolFlags {
  None = 0,
  FunctionScopedVariable = 1 << 0, // Variable (var) or parameter
  BlockScopedVariable = 1 << 1, // A block-scoped variable (let or const)
  Property = 1 << 2, // Property or enum member
  EnumMember = 1 << 3, // Enum member
  Function = 1 << 4, // Function
  Class = 1 << 5, // Class
  Interface = 1 << 6, // Interface
  // ...
}
```

심볼 인터페이스의 `declarations`필드는 AST 노드의 배열 형태를 보인다.

</details>

결과적으로 바인더는 심볼을 생성하고 해당 심볼과 그에 대응하는 AST 노드를 연결하는 역할을 수행한다.

다음은 여러 가지 선언 요소에 대한 각각의 심볼 결과이다.

<details>
  <summary>예제</summary>

```ts
type SomeType = string | number;

interface SomeInterface {
  name: string;
  age?: number;
}

let foo: string = "LET";

const obj = {
  name: "이름",
  age: 10,
};
class MyClass {
  name;
  age;
  constructor(name: string, age?: number) {
    this.name = name;
    this.age = age ?? 0;
  }
}

const arrowFunction = () => {};

function normalFunction() {}

arrowFunction();

normalFunction();

const colin = new MyClass("colin");
```

</details>

#### 여러 선언 요소에 대한 심볼

![](https://velog.velcdn.com/images/orodae/post/72abcbeb-be21-49f4-a007-8868e33cedb7/image.png)

_이미지 출처: "우아한 타입스크립트 with 리액트", 우아한형제들, 한빛미디어, 2023, p. 197._

## 체커(Checker)와 이미터(Emitter)

### 체커(Checker)

체커는 파서가 생성한 AST와 바인더가 생성한 심볼을 활용하여 타입 검사를 수행한다. 이 단계에서 체커의 소스 크기는 파서의 소스 크기보다 매우 크며, 전체 컴파일 과정에서 타입 검사가 차지하는 비중이 크다는 것을 짐작할 수 있다.

체커의 주요 역할은 AST의 노드를 탐색하면서 심볼 정보를 불러와 주어진 소스 파일에 대해 타입 검사를 진행하는 것이다.

체커의 타입 검사는 다음 컴파일 단계인 이미터에서 실행된다. `checker.ts`의 `getDiagnostics()`함수를 사용해서 타입을 검증하고 타입 에러에 대한 정보를 보여줄 에러 메시지를 저장한다.

### 이미터(Emitter)

이미터는 타입스크립트 소스를 자바스크립트(js) 파일과 타입 선언 파일(d.ts)로 생성한다.

이미터는 타입스크립트 소스 파일을 변환하는 과정에서 개발자가 설정한 타입스크립트 설정 파일을 읽어오고, 체커를 통해 코드에 대한 타입 검증 정보를 가져온다. 그리고 `emitter.ts` 소스 파일 내부의 `emitFiles()`함수를 사용하여 타입스크립트 소스 변환을 진행한다.

지금까지의 내용을 정리하면 다음과 같다.

> **타입스크립트의 컴파일 과정**

1. tsc 명령어를 실행하여 프로그램 객체가 컴파일 과정을 시작한다.
2. 스캐너는 소스 파일을 토큰 단위로 분리한다.
3. 파서는 토큰을 이용하여 AST를 생성한다.
4. 바인더는 AST의 각 노드에 대응하는 심볼을 생성한다. 심볼은 선언된 타입의 노드 정보를 담고 있다.
5. 체커는 AST를 탐색하면서 심볼 정보를 활용하여 타입 검사를 수행한다.
6. 타입 검사 결과 에러가 없다면 이미터를 사용해서 자바스크립트 소스 파일로 변환한다.
