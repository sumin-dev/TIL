# 항해99 8기 4주차 <Node.js 숙련> TIL_20220801 #
## 주특기 숙련 주차 팀 과제 ##
### Sequelize같은 ORM과 MySQL같은 데이터베이스는 각각 어떠한 역할을 가지고 있을까요? ###
- **ORM(Object-relational Mapping)** 이란 객체와 관계형 데이터베이스의 데이터를 자동으로 매핑(연결)해주는 것을 말한다. 
- 객체 지향 프로그래밍은 클래스를 사용하고, 관계형 데이터베이스는 테이블을 사용한다.
- 여기서 **Sequelize는 자바스크립트 객체와 데이터베이스의 관계를 매핑해주는 유용한 도구.** 자바스크립트 구문을 SQL문으로 변경.
 ![orm](https://user-images.githubusercontent.com/109029407/182054638-c09518dc-d18d-4540-a087-0d671fccac2a.png)

- **MySQL**은 가장 널리 사용되고 있는 **관계형 데이터베이스 관리 시스템(RDBMS: Relational DBMS)**. 사용자와 DB 간 인터페이스 역할.
- 오픈 소스이며, 다중 사용자와 다중 스레드를 지원. 또한, C언어, C++, JAVA, PHP 등 여러 프로그래밍 언어를 위한 다양한 API를 제공
- 유닉스, 리눅스, 윈도우 등 다양한 운영체제에서 사용.
--- 
### Mongoose에서 exec() 함수의 역할 ###
- 기본적으로 몽고DB는 콜백으로 결과값을 반환. 콜백은 간단하지만 중첩되었을 경우 콜백 지옥이 발생할 수 있다는 문제점이 있음.
- 따라서 콜백 대신 프로미스를 보통 많이 사용.프로미스의 장점(코드 중첩 완화, 조건부 쿼리, 에러 한 번에 처리 등)들을 모두 이용할 수 있기 때문에 편리함.

- 몽구스 공식문서에서 find 메소드에 대해 설명하고 있는 예시사진이다.
 ![exec](https://user-images.githubusercontent.com/109029407/182073303-2152f188-8f1a-49b9-a1cb-faece95ccfbb.png)


- find, findOne, findById, findOneAndUpdate 등의 메서드의 리턴값은 Query라고 되어 있다. Mongoose Query는 프로미스가 아니고, then을 사용할 수 있는 일종의 유사 프로미스.
- find, findOne 등의 메서드 뒤에 exec()을 붙이든 안 붙이든 기능은 동일. 대신 exec()을 사용하면 유사 프로미스가 아닌 온전한 프로미스를 반환값으로 얻을 수 있으며, 에러가 났을 때 stack trace에 오류가 발생한 코드의 위치가 포함되기 때문에 공식 문서에서도 exec()을 사용할 것을 권장.
```
const doc = await Band.findOne({ name: "Guns N' Roses" }); // works

const badId = 'this is not a valid id';
try {
  await Band.findOne({ _id: badId });
} catch (err) {
  // Without `exec()`, the stack trace does **not** include the
  // calling code. Below is the stack trace:
  //
  // CastError: Cast to ObjectId failed for value "this is not a valid id" at path "_id" for model "band-promises"
  //   at new CastError (/app/node_modules/mongoose/lib/error/cast.js:29:11)
  //   at model.Query.exec (/app/node_modules/mongoose/lib/query.js:4331:21)
  //   at model.Query.Query.then (/app/node_modules/mongoose/lib/query.js:4423:15)
  //   at process._tickCallback (internal/process/next_tick.js:68:7)
  err.stack;
}

try {
  await Band.findOne({ _id: badId }).exec();
} catch (err) {
  // With `exec()`, the stack trace includes where in your code you
  // called `exec()`. Below is the stack trace:
  //
  // CastError: Cast to ObjectId failed for value "this is not a valid id" at path "_id" for model "band-promises"
  //   at new CastError (/app/node_modules/mongoose/lib/error/cast.js:29:11)
  //   at model.Query.exec (/app/node_modules/mongoose/lib/query.js:4331:21)
  //   at Context.<anonymous> (/app/test/index.test.js:138:42)
  //   at process._tickCallback (internal/process/next_tick.js:68:7)
  err.stack;
}
```

- 몽구스 3버전까지는 쿼리를 프로미스로 만들기 위해서 뒤에 exec()을 필수로 붙여줬지만 4버전부터는 필수는 아니지만 붙이는 것을 추천.
- 객체를 생성하는 메소드인 save()도 자체적으로 promise. (save는 exec을 붙이지 않음)

- [참고1 (몽구스의 promise)](https://mongoosejs.com/docs/promises.html)
- [참고2 (몽구스의 promise)](https://masteringjs.io/tutorials/mongoose/promise)
