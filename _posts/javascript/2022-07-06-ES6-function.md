---
title: "ES6 함수의 추가 기능"
excerpt: "2015년에 무슨 일이!"

categories: javascript
tags: [javascript, function, ES6]

toc: true
toc_sticky: true

author_profile: true
sidebar: false
---

### 1. 함수의 구분

#### ES6 이전

- ES6 이전에는 자바스크립트의 함수를 사용 목적에 따라 구분하지 않았다.
- 하나의 함수는 '일반 함수', '생성자 함수', '메서드'든 무엇으로든 호출할 수 있었다.
- 즉, ES6 이전의 함수는 callable인 동시에 constructor 였다.
- 생성자 함수로 호출하지 않아도 프로토타입 객체를 생성한다.
- 이러한 실수를 유발할 가능성이 있고 성능에도 좋지 않다.

<br>

#### ES6

- ES6에서는 함수를 사용 목적에 따라 명확하게 구분한다.

![](/assets/images/js/functions.png)

<br>

---

### 2. 메서드

- ES6에서 메서드는 `메서드 축약 표현`으로 정의된 함수를 의미한다.
- 메서드는 내부 슬롯 [[HomeObject]]를 갖는다.
- 메서드는 super 키워드를 사용할 수 있다.

<br>

---

### 3. 화살표 함수

#### 화살표 함수 정의

- 화살표 함수는 항상 함수 표현식으로 정의해야한다.

```js
const multiply = (x, y) => {
  return x * y;
};
```

<br>

- 화살표 함수의 매개변수가 하나일 경우에만 소괄호를 생략할 수 있다.

```js
const arrow = x => { ... };

// 매개변수가 여러 개이거나 없는 경우엔 생략해서는 안된다.
const arrow = (x, y) => { ... };
const arrow = () => { ... };
```

<br>

- 함수의 몸체가 하나의 표현식이라면 중괄호를 생략할 수 있다. 이때 해당 표현식을 평가한 값이 암묵적으로 반환된다.

```js
// concise body
const power = (x) => x ** 2;
power(2); // -> 4

// 위 표현은 다음과 동일하다.
// block body
const power = (x) => {
  return x ** 2;
};
```

<br>

- 객체 리터럴을 반환하는 경우 소괄호로 감싸 주어야 한다.

```js
const create = (id, content) => ({ id, content });
create(1, "JavaScript"); // -> {id: 1, content: "JavaScript"}

// 위 표현은 다음과 동일하다.
const create = (id, content) => {
  return { id, content };
};
```

<br>

#### 화살표 함수와 일반 함수의 차이

- 1. 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor다.
- 2. 중복된 이름의 매개변수를 선언할 수 없다.
- 3. 화살표 함수는 this, arguments, super, new.target을 갖지 않는다.
  - 따라서 화살표 함수 내에서 위 키워드를 참조하면 현재 스코프를 제외한 상위 스코프 체인에서 해당 키워드를 참조한다.

<br>

#### this

- 화살표 함수와 일반 함수가 구별되는 가장 큰 특징은 `this`이다.
- 화살표 함수에서의 this는 `콜백 함수 내부의 this문제`를 해결하기위해 의도적으로 설계된 것이다.
- 화살표 함수는 함수 자체의 this바인딩을 갖지 않는다.
  - 따라서, 화살표 함수 냉부에서 this를 참조하면 상위 스코프의 this를 그대로 참조한다. 이를 `lexical this`라고 한다.
    - 이는 렉시컬 스코프와 같이 화살표 함수의 this가 함수가 정의된 위치에 의해 결정된다는 것을 의미한다.

<br>

#### super

- 화살표 함수는 함수 자체의 super 바인딩을 갖지 않는다.
- 따라서, super를 참조하면 this와 마찬가지로 상위 스코프의 super를 참조한다.

<br>

#### arguments

- 화살표 함수는 함수 자체의 arguments 바인딩을 갖지 않는다.
- 따라서, arguments 참조하면 this와 마찬가지로 상위 스코프의 arguments를 참조한다.
- arguments 객체는 함수를 정의할 때 매개변수의 개수를 확정할 수 없는 가변 인자 함수를 구현할 때 유용하지만 화살표 함수에서는 arguments 객체를 사용할 수 없다.

<br>

---

### 4. Rest 파라미터

#### 기본 문법

- Rest 파라미터는 매개변수의 이름 앞에 `...`을 붙여 정의한다.
- Rest 파라미터는 함수에 전달된 인수들의 목록을 **배열로 전달**받는다.

```js
function foo(...rest) {
  // 매개변수 rest는 인수들의 목록을 배열로 전달받는 Rest 파라미터다.
  console.log(rest); // [ 1, 2, 3, 4, 5 ]
}

foo(1, 2, 3, 4, 5);
```

- Rest 파라미터는 다른 인자와 함께 사용될 수 있다.
- 이때 함수에 전달된 인수들은 매개변수와 Rest 파라미터에 순차적으로 할당된다.
- 그렇기때문에 **Rest 파라미터는 반드시 마지막 파라미터로 사용**해야한다.
- 먼저 선언된 매개변수에 순차적으로 할당되고 남은 인수들은 모두 Rest 파라미터에 배열로 할당된다.
- Rest 파라미터는 `단 하나만 선언`할 수 있다.

```js
function bar(param1, param2, ...rest) {
  console.log(param1); // 1
  console.log(param2); // 2
  console.log(rest); // [ 3, 4, 5 ]
}

bar(1, 2, 3, 4, 5);
```

<br>

#### Rest 파라미터와 arguments 객체

- arguments 객체를 갖지 않는 화살표 함수에서 가변 인자를 구현하기 위해서는 Rest 파라미터를 사용해야한다.

<br>

---

### 5. 매개변수 기본값

- ES6에서는 함수의 인자에 기본값을 지정할 수 있다.
- Rest 파라미터에는 기본값을 지정할 수 없다.

```js
const foo = (x = 0, y = 0) => x + y;

foo(); // 0
// 인자를 전달하지 않았으므로 x, y는 각각 지정한 기본값이 된다.
```
