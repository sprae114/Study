아래는 기존 `$.ajax` 코드를 `axios`를 사용한 `await` 방식으로 변환한 결과입니다. 각 예제는 `GET`과 `POST` 요청에 맞춰 비동기 처리를 간결하게 정리했습니다.

---

## Get 요청

### 1) 기본 요청 (정적 URL)

```html
<body>  
    <button onclick="ex01Fn()">ex01 함수 호출</button>  
</body>  
<script>  
  const ex01Fn = async () => {  
    try {  
      const res = await axios.get("/ex01");  
      console.log("성공", res.data);  
    } catch (error) {  
      console.log("실패", error);  
    }  
  };  
</script>
```

---

### 2) @RequestParam

```html
<body>  
    <button onclick="ex02Fn()">ex02 함수 호출</button>  
</body>  
<script>  
  const ex02Fn = async () => {  
    const val1 = "Hello SpringBoot!!";  
    const params = {  
      param1: val1,  
      param2: "안녕"  
    };  

    try {  
      const res = await axios.get("/get/ex02", { params });  
      console.log("성공", res.data);  
    } catch (error) {  
      console.log("실패", error);  
    }  
  };  
</script>
```

---

### 3) @ModelAttribute

```html
<body>  
    <button onclick="ex03Fn()">ex03 함수 호출</button>  
</body>  
<script>  
  const ex03Fn = async () => {  
    const val1 = "Hello SpringBoot!!";  
    const params = {  
      param1: val1,  
      param2: "안녕"  
    };  

    try {  
      const res = await axios.get("/get/ex03", { params });  
      console.log("성공", res.data);  
    } catch (error) {  
      console.log("실패", error);  
    }  
  };  
</script>
```

---

### 4) @PathVariable

```html
<body>  
    <button onclick="ex04_2Fn()">ex04_2 함수 호출</button>  
</body>  
<script>  
  const ex04_2Fn = async () => {  
    const id = "user1";  

    try {  
      const res = await axios.get(`/get/ex04/${id}`);  
      console.log("성공", res.data);  
    } catch (error) {  
      console.log("실패", error);  
    }  
  };  
</script>
```

---

## Post 요청

### 1) 기본 요청

```html
<body>  
    <button onclick="ex01Fn()">ex01 함수 호출</button>  
</body>  
<script>  
  const ex01Fn = async () => {  
    try {  
      const res = await axios.post("/post/ex01");  
      console.log("성공", res.data);  
    } catch (error) {  
      console.log("실패", error);  
    }  
  };  
</script>
```

---

### 2) @RequestParam

```html
<body>  
    <button onclick="ex02Fn()">ex02 함수 호출</button>  
</body>  
<script>  
  const ex02Fn = async () => {  
    const val1 = "Hello SpringBoot!!";  
    const params = {  
      param1: val1,  
      param2: "안녕"  
    };  

    try {  
      const res = await axios.post("/post/ex02", params);  
      console.log("성공", res.data);  
    } catch (error) {  
      console.log("실패", error);  
    }  
  };  
</script>
```

---

### 3) @ModelAttribute

```html
<body>  
    <button onclick="ex03Fn()">ex03 함수 호출</button>  
</body>  
<script>  
  const ex03Fn = async () => {  
    const val1 = "Hello SpringBoot!!";  
    const params = {  
      param1: val1,  
      param2: "안녕"  
    };  

    try {  
      const res = await axios.post("/post/ex03", params);  
      console.log("성공", res.data);  
    } catch (error) {  
      console.log("실패", error);  
    }  
  };  
</script>
```

---

### 4) @RequestBody

```html
<body>  
    <button onclick="ex04Fn()">ex04 함수 호출</button>  
</body>  
<script>  
  const ex04Fn = async () => {  
    const val1 = "Hello SpringBoot!!";  
    const params = {  
      param1: val1,  
      param2: "안녕"  
    };  

    try {  
      const res = await axios.post("/post/ex04", params, {  
        headers: { "Content-Type": "application/json" }  
      });  
      console.log("성공", res.data);  
      console.log("param1", res.data.param1);  
    } catch (error) {  
      console.log("실패", error);  
    }  
  };  
</script>
```

---

### 5) @RequestBody & List 요청

```html
<body>  
    <button onclick="ex04_1Fn()">ex04_1 함수 호출</button>  
</body>  
<script>  
  const ex04_1Fn = async () => {  
    const params = [  
      { param1: "username1", param2: 20 },  
      { param1: "username2", param2: 20 }  
    ];  

    try {  
      const res = await axios.post("/post/ex04-1", params, {  
        headers: { "Content-Type": "application/json" }  
      });  
      console.log("성공", res.data);  
      console.log("list 0", res.data[0]);  
    } catch (error) {  
      console.log("실패", error);  
    }  
  };  
</script>
```

---

### 6) @RequestBody & ResponseEntity 리턴

```html
<body>  
    <button onclick="ex04_2Fn()">ex04_2 함수 호출</button>  
</body>  
<script>  
  const ex04_2Fn = async () => {  
    const val1 = "Hello SpringBoot!!";  
    const params = {  
      param1: val1,  
      param2: "안녕"  
    };  

    try {  
      const res = await axios.post("/post/ex04-2", params, {  
        headers: { "Content-Type": "application/json" }  
      });  
      console.log("성공", res.data);  
      console.log("param1", res.data.param1);  
    } catch (error) {  
      console.log("실패", error);  
    }  
  };  
</script>
```

---

### 7) @PathVariable

```html
<body>  
    <button onclick="ex05Fn()">ex05 함수 호출</button>  
</body>  
<script>  
  const ex05Fn = async () => {  
    const val1 = "Hello SpringBoot!!";  
    const params = {  
      param1: val1,  
      param2: "안녕"  
    };  
    const id = 3;  

    try {  
      const res = await axios.post(`/post/ex05/${id}`, params, {  
        headers: { "Content-Type": "application/json" }  
      });  
      console.log("성공", res.data);  
    } catch (error) {  
      console.log("실패", error);  
    }  
  };  
</script>
```
