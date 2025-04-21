# 이론
## MongoDB 저장 방식 ✔
- Collection: **폴더**와 같은 개념
- Document: **파일 하나**에 해당하며, 자바스크립트 객체와 유사한 형태로 데이터를 저장합니다.
![[Pasted image 20241011151523.png]]
이렇게 MongoDB를 사용하면 자바스크립트 코드와 유사한 형태로 데이터를 쉽게 저장하고 출력할 수 있습니다.


## id와 ObjectId ✔
MongoDB에서 document를 만들 때 `_id`라는 필드가 자동 생성됩니다. `_id`는 각 document를 구분하기 위한 유니크한 값입니다. 

- ObjectId는 **랜덤 문자열**로, 서로 중복되지 않습니다. **순차적으로 생성**됩니다.
- 기본적으로 자동 생성된 `_id`를 사용하는 것이 일반적입니다.


## 서버와 MongoDB 연결


# Spring
## 저장


## 조회


## 업데이트


## 삭제


# node.js 코드
## 저장
데이터를 삽입하기 위해서는 컬렉션을 선택하고, **해당 컬렉션에 데이터를 추가**합니다.
```javascript
async function insertUser(user) {
    const users = db.collection('users'); // 컬렉션 선택
    const result = await users.insertOne(user); // 데이터 삽입
    console.log(`사용자 추가됨: ${result.insertedId}`);
}

// 사용 예시
insertUser({ name: 'Alice', email: 'alice@example.com', age: 25 });
```

## 조회
데이터를 조회하기 위해서는 `find` 메서드를 사용합니다.
▶ 특정 id 사용자 찾기
```javascript
async function findUser(id) {
    const users = db.collection('users');

    const userList = await users.findOne(id); 
    console.log('사용자 목록:', userList);
}

findUser({_id : new ObjectId(req.params.id)});
```

▶ 전체 사용자 찾기
```javascript
async function findUsers() {
    const users = db.collection('users');

    const userList = await users.find().toArray(); // 모든 사용자 조회
    console.log('사용자 목록:', userList);
}

findUsers();
```


## 업데이트
기존 데이터를 업데이트하는 방법입니다.
```javascript
async function updateUser(email, newAge) {
    const users = db.collection('users');

	const result = await users.updateOne({ email: email }, { $set: { age: newAge } }); 
    console.log(`${result.modifiedCount} 개의 사용자가 업데이트되었습니다.`);
}

// 사용 예시
updateUser('alice@example.com', 26);
```


## 삭제
데이터를 삭제하는 방법입니다.
```javascript
async function deleteUser(email) {
    const users = db.collection('users');

    const result = await users.deleteOne({ email: email }); // 데이터 삭제
    console.log(`${result.deletedCount} 개의 사용자가 삭제되었습니다.`);
}

// 사용 예시
deleteUser('alice@example.com');
```


## 연결 종료
작업이 끝난 후에는 MongoDB 연결을 종료하는 것이 좋습니다.
```javascript
async function closeConnection() {
    await client.close();
    console.log('MongoDB 연결이 종료되었습니다.');
}

// 프로그램 종료 시 연결 종료
closeConnection();
```