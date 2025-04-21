# 글 작성 기능 (Create) 
## 글 작성 요청 어떻게 구현해야 될까?
1) 글 작성 페이지에서 글 작성하고 서버로 보냄
2) 서버에서는 유효성 검사하고 DB에 보냄
3) 이후, 글 목록 페이지로 리다이액트함.
![[Pasted image 20241011165527.png]]


## 글 작성 페이지 GET 구현
1) 글 작성 페이지 요청
```html
<a href="/add"> 글 작성 </a>
```

2) 페이지 get  라우팅
요청을 보내기전에 `write.ejs`에 들어가야 됩니다.
```javascript
app.get('/add', (req, res) => {
  res.render('write.ejs');
});
```


2) EJS 파일 생성
`write.ejs`으로 파일을 만들어서 구현합니다.
```html
<form class="form-box" action="/add" method="POST">
	<h4>글쓰기</h4>
	<input name="title" placeholder="제목을 입력하세요">
	<input name="content" placeholder="내용을 입력하세요">
	<button type="submit">전송</button>
</form>
```
- `<form>` 태그의 `action` 속성에 `/add` URL을, `method` 속성에 `POST`를 설정하여 글 작성 정보를 서버로 전송합니다. 
- 각 `<input>` 태그에는 `name` 속성을 설정하여 데이터를 서버에 전달할 수 있습니다.


## 글 생성 POST 요청
1) 글 생성 post 요청
위의 작성한 페이지에서 post요청 구현했습니다.

2) 글 생성 post 라우팅
`/add` URL로 POST 요청이 들어오면 요청 **본문을 `req.body`를 통해 쉽게 접근**할 수 있습니다.
```javascript
app.post('/add', (req, res) => {
	// 비즈니스 로직 작성
});
```

2) API 구조 작성하기
```javascript
app.post('/add', (req, res) => {
	try{
		if(제목이 비어있다면?){
			오류처리
		}else{
			db 저장하기
		}
		페이지 리다이렉트
	} catch(e){
		오류페이지로 이동
	}
});
```

3) 코드로 구현하기(유효성 검사, db 전송, 에러 처리)
```javascript
app.post('/add', async (req, res) => {
	try{
		if(req.body.title == ''){
			res.rediect('/add?error')
		}else{
			await db.collection('post').insertOne({
				title: req.body.title,
				content: req.body.content
			});
		}
		res.rediect('/list')
	} catch(e){
		console.log(e);
		res.rediect('/error')
	}
});
```


# 글 상세페이지 조회(Read)
## 상세페이지 조회 어떻게 구현해야 될까?
1. 사용자가 `/detail/어쩌구` URL에 접속하면
2. 해당 `_id`를 가진 글을 DB에서 찾아와서
3. EJS 페이지에 글 내용을 삽입하여 사용자에게 보여줍니다.
![[Pasted image 20241011165551.png]]


## 글 상세페이지 GET 구현
1) 상세 페이지로 요청
- 사용자가 특정 글의 상세 페이지로 쉽게 이동할 수 있도록 하려면, 링크를 생성해야 합니다.
- 예를 들어, `/detail/글_id` 형태의 링크를 만들어 사용자가 **클릭하면 해당 글의 상세 페이지로** 이동할 수 있습니다.

```c
<div class="white-bg">
  <% for (let i = 0; i < 글목록.length; i++){ %>
	<div class="list-box">
	  <h4> <a href="/detail/<%= 글목록[i]._id %>"> <%= 글목록[i].title %> </a>
	  </h4>
	  <p>글내용임</p>
	</div>
  <% } %>
</div>
```

2) 글 상세페이지 GET 라우팅
Express.js에서 URL 파라미터를 사용하여 동적인 URL을 처리할 수 있습니다.
```javascript
app.get('/detail/:id', (req, res) => {
	// 비즈니스 로직 작성
});
```

3) API 구조 작성하기
```javascript
app.get('/detail/:id', (req, res) => {
	try{
		DB에서 해당 id로 글 찾기
		if(id 값이 존재하지 않으면?){
			페이지 오류
		}
		글 상세 페이지로 리다이렉트 + 데이터 전달
	} catch(e){
		DB 요청 오류
	}
});
```

4) API 구현하기
```javascript
app.get('/detail/:id', (req, res) => {
	try{
		const data = await db.collection('post')
			.findOne({_id : new ObjectId(req.parmas.id)});
		if(data = null){
			res.rediect('/list?error')
		}
		res.render('detail.ejs', { data : data });
	} catch(e){
		res.rediect('/error')
	}
});
```

5) detail 페이지 구현
`detail.ejs` 상세페이지 레이아웃을 작성합니다.
```html
<!DOCTYPE html>
<html lang="ko">
<head>
	<meta charset="UTF-8">
	<title>글 상세보기</title>
</head>
<body>
	<h1><%= 글.title %></h1>
	<p><%= 글.content %></p>
	<a href="/list">목록으로 돌아가기</a>
</body>
</html>
```


# 수정

## 수정 요청 어떻게 구현해야 될까?
1. 글마다 있는 수정버튼 누르면 **글수정할 수 있는 페이지**로 이동
2. 그 페이지엔 **글의 제목과 내용이 이미 폼에 채워져있어야함**
![[Pasted image 20241011094724.png]]
3. 전송누르면 그걸로 기존에 있던 **document를 수정**해줌

![[Pasted image 20241011170440.png]]


## 글 수정 GET 요청
1) 글 수정 페이지 GET 요청
`list.ejs` 파일에 **수정 버튼을 추가**하여 사용자가 클릭하면 **해당 글의 수정 페이지(`/edit/글_id`)로 이동**하도록 합니다.
```javascript
<div class="white-bg">
	<% for (var i = 0; i < 글목록.length; i++) { %>
		<div class="list-box">
			<a href="/detail/<%= 글목록[i]._id %>">
			  <h4><%= 글목록[i].title %></h4>
			</a>
			<a href="/edit/<%= 글목록[i]._id %>">✏️</a>
			<p><%= 글목록[i].content %></p>
		</div>
	<% } %>
</div>
```

2) 라우팅
```javascript
app.get('/edit/:id', (req, res) => {
	//비즈니스 로직
});
```

3) API 구조 작성
```javascript
app.get('/edit/:id', (req, res) => {
	try{
		DB에서 해당 id로 글 찾기
		if(id 값이 존재하지 않으면?){
			페이지 오류
		}
		글 상세 페이지로 리다이렉트 + 데이터 전달
	} catch(e){
		DB 요청 오류
	}
});
```

4) API 구현
```javascript
app.get('/detail/:id', (req, res) => {
	try{
		const result = await db.collection('post')
			.findOne({_id : new ObjectId(req.parmas.id)});
		if(data = null){
			res.rediect('/list?error')
		}
		res.render('edit.ejs', { result : result });
	} catch(e){
		res.rediect('/error')
	}
});
```

5) 수정 페이지 만들기
```html
<form action="/edit/<%= result._id %>" method="POST">
	<input name="title" value="<%= result.title %>">
	<input name="content" value="<%= result.content %>">
	<button type="submit">수정하기</button>
</form>
```
- `/edit/<%= result._id %>` : 어떤 글을 수정할지 모르니깐 동적ID로 요청해줍니다.
- 사용자가 수정한 제목과 내용을 서버로 전송합니다. 전송 시 해당 글의 ID도 함께 전송됩니다.


## 글 수정 POST 요청
1) 글 수정 post 요청
위의 작성한 페이지에서 post요청 구현했습니다.

2) post 라우팅
```javascript
app.post('/edit/:id', async (req, res) => {
	//비즈니스 로직
});
```

3) API 구조 작성
```javascript
app.post('/edit/:id', (req, res) => {
	try{
		if(제목이 비어있다면?){
			오류처리
		}else{
			db 수정하기
		}
		페이지 리다이렉트 
	} catch(e){
		오류페이지로 이동
	}
});
```

4) API 구현
```javascript
app.post('/edit/:id', async (req, res) => {
	try{
		if(req.body.title == ''){
			res.rediect('/add?error')
		}else{
			await db.collection('post').updateOne({
				{ _id: new ObjectId(req.params.id)},
				{ $set: { title: req.body.title, content: req.body.content }}
			);
		}
		res.rediect('/list')
	} catch(e){
		console.log(e);
		res.rediect('/error')
	}
});
```


# 삭제


# 로그인