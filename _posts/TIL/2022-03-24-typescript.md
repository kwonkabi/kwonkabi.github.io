---
title: "자바스크립트와 타입스크립트의 차이"
excerpt: "타입스크립트를 왜, 어떻게 쓰는지 알아보자!"

categories: typescript
tags: [typescript, javascript, ts, tsx]

toc: true
toc_sticky: true

author_profile: true
sidebar: false
---

### 1. 타입스크립트란

타입스크립트란, 자바스크립트의 '타입'을 강제시키는 언어이다. 자바스크립트에서는 변수에 문자열을 할당했다가 숫자열을 재할당 해도 문제가 되지 않는다. 즉 자료형(타입)이 중요하지 않다.

```javascript
let hello = "hello";
hello = 12345;
```

그러나 타입스크립트에서는 변수의 타입을 지정해주기 때문에, 위와 같은 재할당이 불가능하다.

```typescript
let hello: string = "hello";

// hello = 12345 // 에러! 문자열만 재할당 가능.
hello = "12345";
```

문자열(string)이라고 지정하면 죽어도 문자열만 들어와야 한다. 그렇지 않으면 에러가 뜬다. 코드량이 많아지는데도 굳이 타입스크립트를 사용하는 추세가 증가하고 있는 것도 이 때문이다. **팀 단위로 협업할 때 문제를 사전에 차단하기 위해**서이다. 그렇다고 꼭 한 개의 타입만 지정할 수 있는 건 아니니, 타입스크립트의 사용방법에 대해 알아보자.

<br>

---

<br>

### 2. 타입스크립트의 사용 방법

[**타입스크립트 설치 방법**](https://www.typescriptlang.org/download)

<br>

#### 2.1. 타입스크립트 확장자

타입스크립트를 설치해주었다면, 자바스크립트에서 타입스크립트 파일로 확장자를 변경해주어야 한다. 이때 자바스크립트 파일 중 return을 포함하고 있다면(jsx를 포함하고 있다면) 확장자를 .tsx로, return이 없다면 .ts로 변경해주면 된다.

<br>

#### 2.2. 타입스크립트의 타입

- 타입 추론: js파일을 ts 혹은 tsx 파일로 바꾸게 된다고 모든 변수의 타입을 다 지정해주어야 하는 것은 아니다. 타입스크립트가 알아서 눈치껏 추론을 한다.

```typescript
let aaa = "안녕하세요";
aaa = 3;
```

이렇게 입력하면 재할당이 안 된다고 두 번째 aaa에 빨간 줄이 그어진다.

- 타입 명시: ts 파일로 변경했을 때 빨간 줄이 그어지는 부분들은 '어떤 타입인지 모르니 명시해달라'는 뜻이니 찾아서 명시해주면 된다. 위에서 잠깐 봤듯이, 기본 형식은 이렇다. 변수 뒤에 콜론을 붙이고 변수 타입을 지정해주면 된다.

```typescript
let bbb: string = "반가워요";
```

타입의 종류는 자바스크립트와 비슷하다. 자주 사용하는 타입 여섯 가지를 소개할 것이다. 사용법은 조금 생소할 수 있으니 하나하나 보자.

<br>

##### 2.2.1. 문자 타입

```typescript
let ccc: string;
ccc = "반갑습니다~";
ccc = 3;
```

<br>

##### 2.2.2. 숫자 타입

```typescript
let ddd: number = 10;
ddd = "10"; // error
```

<br>

##### 2.2.3. 불린 타입

```typescript
let eee: boolean = true;
eee = false;
eee = "false"; // error
```

불린 타입은 자바스크립트에서 특히 유의해야 하는 타입이기 때문에 타입스크립트의 진가가 여기서 드러난다. 자바스크립트에서는 문자열 "false"는 빈 문자열이 아니기 때문에 불린값으론 true를 반환한다. (혼돈의 카오스,,) 그래서 타입을 불린으로 지정해주면 이러한 혼동을 막을 수 있다. 'false'는 애초에 불린값이 아니라 true나 false를 반환하지도 않고, 에러가 뜬다!

<br>

##### 2.2.4. 배열 타입

```typescript
let fff: number = [1, 2, 3, 4, 5, "안녕하세요"]; // error
let ggg: string = ["철수", "영희", "훈이", 123]; // error
let hhh: number | string = [1, 2, 3, 4, 5, "안녕하세요"];
```

※ 위에서 사용한 바는 '또는'을 의미한다. '숫자나 문자열 중 아무거나 상관 없다'는 뜻! 그래서 엘리먼트로 숫자와 문자열이 같이 왔는데도 위의 두 개와 달리 에러가 뜨지 않는다. 자바스크립트에서는 바를 두 개 써줘야 했지만 타입스크립트에서는 하나만 사용한다. 마찬가지로 자바스크립트에서 '그리고'를 의미하는 &&는 타입스크립트에서 & 이렇게 하나만 사용한다! 배열뿐만 아니라 객체 타입에서도 사용되니 익숙해지자!

<br>

##### 2.2.5. 객체 타입

```typescript
interface IProfile {
  name: string;
  age: string | number;
  school: string;
  hobby?: string; // 선택사항
}
```

객체 타입에서는 낯선 **interface**라는 게 등장했다. 객체 타입을 만들어 줄 때 꼭 써줘야 한다. 그리고 객체 타입 변수명을 정할 때는 관례가 있다. interface의 I를 따오고, 객체를 담은 변수 이름을 대문자로 변경해서 위처럼 IProfile과 같은 식으로 네이밍한다.

※ hobby 뒤에 붙은 **?**는 선택사항을 의미한다. 꼭 값을 받아오지 않아도 무관하다는 의미.

```typescript
let profile: IProfile = {
  name: "철수",
  age: 8,
  school: "다람쥐초등학교",
};
profile.age = "8살";
profile.school = 123; // error
```

재할당 시에, age는 타입을 문자열 혹은 숫자로 지정했기 때문에 '8살'이라는 문자열로 재할당해도 에러가 뜨지 않지만, school에는 문자열 타입을 지정해두었기 때문에 숫자를 넣으면 에러가 뜬다!

<br>

##### 2.2.6. 함수 타입

```typescript
const add = (money1: number, money2: number, unit: string): string => {
  return money1 + money2 + unit;
};
const resutlt = add(1000, 2000, "원");
```

함수 타입을 지정해 줄 때는, 함수를 선언할 때부터 매개변수의 타입을 지정해준다!
