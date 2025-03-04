1. 액션, 계산, 데이터 구분하기
   - 암묵적 입출력 (인자외 다른입력+ 전역변수 읽기 , 러턴값외 다른출력 + 전역 배열 바꾸기)
     - o (액션)
     - x (계산)
2. 액션에서 계산 빼기

### 빼는 이유

- 테스트 를 쉽게 하기 위해서
- 재사용성을 위해서

### 단점?

- 코드가 길어짐
  - 테스트 코드를 하는데 쉬어짐

## 느낀점

1. 고정적 관념인 코드가 길어지면 안좋다 생각 바꾸기
2. 테스트 코드는 줄어든다라는 의미 기억하기
<!-- 다시 작성하는 4장 -->

## 액션에서 계산 빼기

### 목표

1. 테스트 하기 쉽게 만들기
2. 재사용 하기 쉽게 만들기
3. 액션과 계산 , 데이터를 구분하기

- 함수에 암묵적 ( 입,출력)이있으면 액션이 된다

### 제시 되는조건 (더좋은 코드 만드는 법)

1. DOM 업데이트와 비즈니스 규칙의 분리
2. 전역변수의 제거 , 전역변수의 의존 x
3. DOM 사용가능한곳에서 실행 된다고 가정 x
4. 함수가 결과값을 리턴 (암묵적인것이아닌 명시적인값)

### 이제 액션 함수 에서 계산 빼기

- 서브루틴 출력하기
  - 기존 코드 (액션 , 계산)-> 암묵적(입력 , 출력) => 새로운 함수로 변경(입력 - 인자 , 출력 - 리턴값) + 계산
    - 함수를 호출하는 쪽에서 전역변수의 값을 할당하도록 함 - 호출하는 곳에서 리턴값을 전역 변수에 할당
    - 새로운 함수쪽에서는 지역변수 ,지역변수 리턴으로 수정

### 또다른 액션을 계산으로 빼는 법 (코드적으로 설명)

```js
function add_item_to_cart(name, price) {
  shopping_cart.push({
    name: name,
    price: price,
  });
  calc_cart_total();
}
```

- 위와같은 코드를 새함수 로 분리를 하면

```js
function add_item_to_cart(name, price) {
  add_item(name, price);
  calc_cart_total();
}
function add_item(name, price) {
  shopping_cart.push({
    // push()함수로 배열을 바꾸고 있다 , 전역 함수인 shopping_cart를 읽고있다
    name: name,
    price: price,
  });
} //새함수
```

- 암묵적 입력과 출력을 인자와 리턴값으로 변경

```js
function add_item_to_cart(name, price) {
  add_item(shopping_cart, name, price); // 전역변수를 인자로 넘김
  calc_cart_total();
}
function add_item(cart, name, price) {
  cart.push({
    // 전역변수 대신 인자를 사용
    name: name,
    price: price,
  });
}
```

- 위와같이 암묵적 입력을 인자로 변경하며 명시적인 입력으로 변경

```js
function add_item_to_cart(name, price) {
  shopping_cart = add_item(shopping_cart, name, price);
  // 원래함수에서는 리턴값을 받아 전역 변수에 할당하는 식으로 변경
  calc_cart_total();
}
function add_item(cart, name, price) {
  const new_cart = cart.slice();
  // 복사본을 만들어 지역변수에 할당합니다
  new_cart.push({
    // 복사본으로 변경
    name: name,
    price: price,
  });
  return new_cart;
}
```

- 마지막으로 암묵적 출력을 복사본을 만들어 리턴값으로 변경하는 과정으로 add_item()에서는 암묵적 입출력이 없는 계산이 완성되게 됩니다
