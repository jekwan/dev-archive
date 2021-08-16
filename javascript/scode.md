# Scope
Scope는 식별자(변수 이름, 함수 이름, 클래스 이름 등)의 유효 범위를 의미한다.  
예를 들어, 함수의 매개변수는 함수 내부에서만 참조할 수 있고, 외부에서는 참조할 수 없다. 이는 매개변수를 참조할 수 있는 유효범위, 즉 매개변수의 Scope가 함수의 Body로 한정되기 때문이다.

```javascript
var x = 'global';

function foo() {
  var x = 'local';
  console.log(x); //      (1)
}

foo();

console.log(x); //        (2)
```
자바스크립트 엔진은 (1)과 (2)에서 이름이 같은 두 변수 중 어떤 변수를 참조할 지 결정(identifier resolution)해야 한다.  
자바스크립트 엔진은 코드를 실행할 때, 코드의 문맥(execution context)을 기반으로 실행하게 된다.  
위 코드에서 자바스크립트 엔진이 식별자를 참조할 때, Scope를 통해 어떤 식별자를 참조할 지 결정한다.  
(1)과 (2)가 각각 참조하는 식별자는 이름은 동일하지만, 속해있는 Scope가 다른 별개의 변수이다.
즉, 스코프는 일종의 `네임 스페이스`이다.


# Scope chain

```javascript
var x = 'global x';
var y = 'global y';
var z = 'global z';

function outer() {
  var x = 'outer x';
  var y = 'outer y';
  
  function inner() {
    var x = 'inner x';
    
    console.log(x); //      (1)
    console.log(y); //      (2)
    console.log(z); //      (3)
  }
  
  inner();
}

outer();
```

위 코드를 실행하면,  
```
inner x   (1)의 결과
outer y   (2)의 결과
global z  (3)의 결과
```
가 나타나게 된다. log함수는 모두 inner함수의 내부에서 실행되었지만, 식별자 `y`와 `z`는 각각 outer 스코프와 전역 스코프에서 참조하였다.  
각각의 함수는 하나의 Scope를 구성하는데, 함수가 중첩되는 모양에 따라 Scope도 중첩된 계층 구조를 형성 한다.  
즉, outer 함수의 스코프는 inner 함수의 스코프보다 상위의 스코프이다. 마찬가지로 전역 스코프는 outer 함수 스코프의 상위 스코프이다.  
그리고 자바스크립트 엔진에서 식별자를 참조할 때, 현재 Scope에서 시작하여 전역 Scope까지 순서대로 상위 Scope로 이동하며 식별자를 검색(identifier resolution)한다.  
`inner -> outer -> 전역`의 순서로 스코프를 이동하며 변수를 검색하고, 변수가 찾아지면 더 이상 상위 스코프로 가지 않고 식별자 검색을 마친다.  
이렇듯 현재 스코프에서 연결된 상위 스코프로 이동해가며 식별자를 찾는 것을 `Scope Chain`이라고 한다.  
실제로 ECMA-262는 자바스크립트 엔진 구현체는 코드를 실행하기에 앞서 Lexical Environment라는 자료구조를 생성하고, 식별자의 검색은 이 자료구조 상에서 행해진다고 명시하고있다.
