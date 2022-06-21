# 싱글톤 패턴

> 하나의 클래스에 오직 하나의 인스턴스만

싱글톤 패턴은 다양한 `생성 패턴`의 일종입니다. 하나의 인스턴스를 만들어 놓고 그 인스턴스를 다른 여러 모듈들이 공유하며 사용하는 방식입니다.

# 장점

- 하나의 인스턴스만 생성하여 사용하기 때문에 생성 비용이 낮습니다.
- 하나의 인스턴스만 존재하기 때문에 `메모리`를 적게 사용할 수 있다.
- 다른 모듈간 데이터 공유가 쉬워진다.

# 단점

- 멀티쓰레딩 환경에서 동시성 문제를 해결해야 한다.
- 테스트하기 어렵다.
  - `TDD`방식으로 개발을 할 때, 독립적인 환경에서 `단위 테스트`를 진행해야 하는데 `싱글톤 객체`는 독립적인 환경을 만들 때 걸림돌이 됩니다. 같은 `싱글톤 객체`에 의존하는 여러 테스트가 있다면 테스트 순서에 따라 결과가 달라질 수 있기 때문에 매 번 테스트마다 `싱글톤 객체`를 초기화 해줘야 하는 번거로움이 생깁니다.

# 예시

```javascript
const obj1 = { a : 27 };
const obj2 = { a : 27 };

console.log(obj === obj2);  >> false
```

`obj1`과 `obj2`는 같은 형태의 객체이지만 다른 인스턴스 입니다.

```javascript
class Singleton {
  constructor() {
    if (!Singleton.instance) {
      Singleton.instance = this;
    }
    return Singleton.instance;
  }
  getInstance() {
    return this.instance;
  }
}

const obj1 = new Singleton();
const obj2 = new Singleton();

console.log(obj1 === obj2);  >> true
```

`obj1`과 `obj2`는 같은 인스턴스입니다.

```javascript
const URL = 'mongodb://localhost:27017/myapp';
const createConnection = url => ({url : url});
class DB {
  constructor(url) {
    if (!DB.instance) {
      DB.instance = createConnection(url);
    }
    return DB.instance;
  }

  connect() {
    return this.instance;
  }
}

const connection1 = new DB(URL);
const connection2 = new DB(URL);

console.log(connection1 === connection2); >> true
```

DB에 대한 연결을 `싱글톤`으로 사용할 수 있습니다.

```javascript
Mongoose.prototype.connect = function(uri, options, callback) {
  const _mongoose = this instanceof Mongoose ? this : _mongoose;
  const conn = _mongoose.connection;
  return _mongoose._promiseOrCallback((callback, cb) => {
    conn.openUri((uri, options, err) => {
      if (err != null) {
        return cb(err);
      }
      return cb(null, _mongoose);
    } ;
  });
};
```

`Mongoose`라는 라이브러리의 실제 `connect` 함수는 위와 같습니다.
