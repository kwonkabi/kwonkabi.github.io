---
title: "디스트럭처링 할당"
excerpt: "객체와 배열 구조분해할당 개많이 썼잖아!"

categories: javascript
tags: [javascript, destructuring-assignment]

toc: true
toc_sticky: true

author_profile: true
sidebar: false
---

> 디스트럭처링 할당(구조 분해 할당, destructuring assignment)은 구조화된 배열과 같은 이터러블 또는 객체를 비구조화(구조 파괴)하여 1개 이상의 변수에 개별적으로 할당하는 것을 말한다.

배열과 같은 이터러블 또는 객체 리터럴에서 **필요한 값만 추출하여 변수에 할당**할 때 유용하다.

---

### 1. 배열 디스트럭처링 할당

- **ES5**에서 구조화된 배열을 디스트럭처링하여 변수에 할당하는 방법

```js
// ES5
var arr = [1, 2, 3];

var one = arr[0];
var two = arr[1];
var three = arr[2];

console.log(one, two, three); // 1 2 3
```

<br>

- **ES6**의 배열 디스트럭처링 할당

  - 배열의 각 요소를 배열로부터 추출하여 1개 이상의 변수에 할당한다.
  - 배열 디스트럭처링 할당의 대상(할당문의 우변)은 이터러블이어야 한다.
  - 순서대로 할당된다.

```js
const arr = [1, 2, 3];

// ES6 배열 디스트럭처링 할당
// 변수 one, two, three를 선언하고 배열 arr을 디스트럭처링하여 할당한다.
// 이때 할당 기준은 배열의 인덱스다.
const [one, two, three] = arr;

console.log(one, two, three); // 1 2 3
```

<br>

- 배열 디스트럭처링 할당을 위해서는 **할당 연산자 왼쪽에 값을 할당받을 변수를 배열 리터럴 형태로 선언**해야 한다.

```js
const [x, y] = [1, 2];
```

<br>

- 우변에 이터러블을 할당하지 않으면 에러가 발생한다.

```js
const [x, y]; // SyntaxError: Missing initializer in destructuring declaration

const [a, b] = {}; // TypeError: {} is not iterable
```

<br>

- 배열 디스트럭처링 할당의 변수 선언문은 선언과 할당을 분리할 수 있지만, 그 경우 const 키워드를 사용할 수 없기 때문에 권장하지 않는다.

```js
let x, y;
[x, y] = [1, 2];
```

<br>

- 배열 디스트럭처링 할당의 기준은 배열의 인덱스다. 즉, **순서대로 할당**된다. 이때 변수의 개수와 이터러블의 요소 개수가 반드시 일치할 필요는 없다.

```js
const [a, b] = [1, 2];
console.log(a, b); // 1 2

const [c, d] = [1];
console.log(c, d); // 1 undefined

const [e, f] = [1, 2, 3];
console.log(e, f); // 1 2

const [g, , h] = [1, 2, 3];
console.log(g, h); // 1 3
```

<br>

- 배열 디스트럭처링 할당을 위해 변수에 **기본값**을 설정할 수 있다.

```js
// 기본값
const [a, b, c = 3] = [1, 2];
console.log(a, b, c); // 1 2 3

// 기본값보다 할당된 값이 우선한다.
const [e, f = 10, g = 3] = [1, 2];
console.log(e, f, g); // 1 2 3
```

<br>

- 배열 디스트럭처링 할당은 배열과 같은 **이터러블에서 필요한 요소만 추출하여 변수에 할당하고 싶을 때** 유용하다.

- 다음 예제는 URL을 파싱하여 protocol, host, path 프로퍼티를 갖는 객체를 생성해 반환한다.

```js
// url을 파싱하여 protocol, host, path 프로퍼티를 갖는 객체를 생성해 반환한다.
function parseURL(url = "") {
  // '://' 앞의 문자열(protocol)과 '/' 이전의 '/'으로 시작하지 않는 문자열(host)과 '/' 이후의 문자열(path)을 검색한다.
  const parsedURL = url.match(/^(\w+):\/\/([^/]+)\/(.*)$/);
  console.log(parsedURL);
  /*
  [
    'https://developer.mozilla.org/ko/docs/Web/JavaScript',
    'https',
    'developer.mozilla.org',
    'ko/docs/Web/JavaScript',
    index: 0,
    input: 'https://developer.mozilla.org/ko/docs/Web/JavaScript',
    groups: undefined
  ]
  */

  if (!parsedURL) return {};

  // 배열 디스트럭처링 할당을 사용하여 이터러블에서 필요한 요소만 추출한다.
  const [, protocol, host, path] = parsedURL;
  return { protocol, host, path };
}

const parsedURL = parseURL(
  "https://developer.mozilla.org/ko/docs/Web/JavaScript"
);
console.log(parsedURL);
/*
{
  protocol: 'https',
  host: 'developer.mozilla.org',
  path: 'ko/docs/Web/JavaScript'
}
*/
```

<br>

- 배열 디스트럭처링 할당을 위한 변수에 Rest 파라미터와 유사하게 Rest 요소 `...`을 사용할 수 있다.

```js
// Rest 요소
const [x, ...y] = [1, 2, 3];
console.log(x, y); // 1 [ 2, 3 ]
```

<br>

---

### 2. 객체 디스트럭처링 할당

- **ES5**에서 객체의 각 프로퍼티를 객체로부터 디스트럭처링하여 변수에 할당하기 위해서는 프로퍼티 키를 사용해야 한다.

```js
// ES5
var user = { firstName: "Ungmo", lastName: "Lee" };

var firstName = user.firstName;
var lastName = user.lastName;

console.log(firstName, lastName); // Ungmo Lee
```

<br>

- **ES6**의 객체 디스트럭처링 할당은 객체의 각 프로퍼티를 객체로부터 추출하여 1개 이상의 변수에 할당한다.

  - 객체 디스트럭처링 할당의 대상(할당문의 우변)은 객채여야 하며, **할당 기준은 프로퍼티 키다**.
  - 즉, 배열과 달리 순서는 의미가 없으며 선어된 변수 이름과 프로퍼티 키가 일치하면 할당된다.

```js
const user = { firstName: "Ungmo", lastName: "Lee" };

// ES6 객체 디스트럭처링 할당
// 변수 lastName, firstName을 선언하고 user 객체를 디스트럭처링하여 할당한다.
// 이때 프로퍼티 키를 기준으로 디스트럭처링 할당이 이루어진다. 순서는 의미가 없다.
const { lastName, firstName } = user;

console.log(firstName, lastName); // Ungmo Lee
```

<br>

- 할당 연산자 왼쪽에프로퍼티 값을 할당받을 변수를 객체 리터럴 형태로 선언해야 한다.

```js
const { lastName, firstName } = { firstName: "Ungmo", lastName: "Lee" };
```

<br>

- 우변에 객체 또는 객체로 평가될 수 있는 표현식을 할당하지 않으면 에러가 발생한다.

```js
const { lastName, firstName };
// SyntaxError: Missing initializer in destructuring declaration

const { lastName, firstName } = null;
// TypeError: Cannot destructure property 'lastName' of 'null' as it is null.
```

<br>

- lastName, firstName은 **프로퍼티 축약 표현**을 통해 선언한 객체 리터럴 형태의 변수이다.

```js
const { lastName, firstName } = user;
// 위와 아래는 동치다.
const { lastName: lastName, firstName: firstName } = user;
```

<br>

- 객체의 프로퍼티 키와 다른 변수 이름으로 프로퍼티 값을 할당받으려면 다음과 같이 변수를 선언한다.

```js
const user = { firstName: "Ungmo", lastName: "Lee" };

// 프로퍼티 키를 기준으로 디스트럭처링 할당이 이루어진다.
// 프로퍼티 키가 lastName인 프로퍼티 값을 ln에 할당하고,
// 프로퍼티 키가 firstName인 프로퍼티 값을 fn에 할당한다.
const { lastName: ln, firstName: fn } = user;

console.log(fn, ln); // Ungmo Lee
```

<br>

- 객체 디스트럭처링 할당을 위한 변수에 **기본값**을 설정할 수 있다.

```js
const { firstName = "Ungmo", lastName } = { lastName: "Lee" };
console.log(firstName, lastName); // Ungmo Lee

const { firstName: fn = "Ungmo", lastName: ln } = { lastName: "Lee" };
console.log(fn, ln); // Ungmo Lee
```

<br>

- 객체 디스트럭처링 할당은 **객체에서 프로퍼티 키로 필요한 프로퍼티 값만 추출하여 변수에 할당하고 싶을 때** 유용하다.

```js
const str = "Hello";
// String 래퍼 객체로부터 length 프로퍼티만 추출한다.
const { length } = str;
console.log(length); // 5

const todo = { id: 1, content: "HTML", completed: true };
// todo 객체로부터 id 프로퍼티만 추출한다.
const { id } = todo;
console.log(id); // 1
```

<br>

- 객체 디스트럭처링 할당은 객체를 인수로 전달 받는 함수의 **매개변수**에도 사용할 수 있다.

```js
function printTodo(todo) {
  console.log(
    `할일 ${todo.content}은 ${todo.completed ? "완료" : "비완료"} 상태입니다.`
  );
}

printTodo({ id: 1, content: "HTML", completed: true });
// 할일 HTML은 완료 상태입니다.
```

<br>

- 위 예제에서 객체를 인수로 전달받는 매개변수 todo에 객체 디스트럭처링 할당을 사용하면 좀 더 간단하고 가독성 좋게 표현할 수 있다.

```js
function printTodo({ content, completed }) {
  console.log(`할일 ${content}은 ${completed ? "완료" : "비완료"} 상태입니다.`);
}

printTodo({ id: 1, content: "HTML", completed: true });
// 할일 HTML은 완료 상태입니다.
```

<br>

- **배열의 요소가 객체인 경우 배열 디스트럭처링 할당과 객체 디스트럭처링 할당을 혼용**할 수 있다.

```js
const todos = [
  { id: 1, content: "HTML", completed: true },
  { id: 2, content: "CSS", completed: false },
  { id: 3, content: "JS", completed: false },
];

// todos 배열의 두 번째 요소인 객체로부터 id 프로퍼티만 추출한다.
const [, { id }] = todos;
console.log(id); // 2
```

<br>

- **중첩 객체**의 경우 다음과 같이 사용한다.

```js
const user = {
  name: "Lee",
  address: {
    zipCode: "03068",
    city: "Seoul",
  },
};

// address 프로퍼티 키로 객체를 추출하고 이 객체의 city 프로퍼티 키로 값을 추출한다.
const {
  address: { city },
} = user;
console.log(city); // 'Seoul'
```

<br>

- 객체 디스트럭처링 할당을 위한 변수에 Rest 파라미터나 Rest 요소와 유사하게 Rest 프로퍼티 `...`을 사용할 수 있다.

```js
// Rest 프로퍼티
const { x, ...rest } = { x: 1, y: 2, z: 3 };
console.log(x, rest); // 1 { y: 2, z: 3 }
```
