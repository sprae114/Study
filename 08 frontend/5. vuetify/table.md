[Material Component Framework — Vuetify.js](https://v15.vuetifyjs.com/ko/components/data-tables/)
[Data table component — Vuetify](https://vuetifyjs.com/en/components/data-tables/basics/)
[API — Vuetify](https://vuetifyjs.com/en/api/v-data-table/)

---

## 종류


## Props

### 제목
```html
<v-data-table :headers="headers"></v-data-table>
```

```js
data() {
	return {
		headers: [
			{ title: 'ID', value: 'id', align: 'start' },
			{ title: '카테고리', value: 'category', align: 'center', sortable: true },
			{ title: '제목', value: 'title', align: 'center', sortable: true },
			{ title: '작성자', value: 'author', align: 'center', sortable: true },
		],
	};
},
...
```

![[Pasted image 20250121195420.png]]

### 데이터
```html
<v-data-table :headers="headers" :items="posts"></v-data-table>
```

```js
data: () => ({
	headers: [
		{ title: 'ID', value: 'id', align: 'start' },
		{ title: '카테고리', value: 'category', align: 'center', sortable: true },
		{ title: '제목', value: 'title', align: 'center', sortable: true },
		{ title: '작성자', value: 'author', align: 'center', sortable: true },
	],
	posts : [
		{ id: 1, title: '여름의 추억', author: '홍길동', date: '2023-06-15', category: '여행' },
		{ id: 2, title: '겨울철 여행 추천지', author: '김영희', date: '2023-12-01', category: '여행' },
		...
		{ id: 20, title: '모바일 앱 개발', author: '홍길동', date: '2023-09-30', category: '기술' },
		{ id: 21, title: '모바일 앱 개발', author: '홍길동', date: '2023-09-30', category: '기술' },
	],
}),
```

![[Pasted image 20250121200124.png]]

# 슬롯
## 상단 및 하단

### top
**테이블의 상단**에 사용자 정의 콘텐츠를 추가할 수 있는 슬롯입니다. 보통 **검색 필드나 버튼** 등을 추가하는 데 사용됩니다.
```vue
<template v-slot:top>
  <v-btn @click="addItem">새 항목 추가</v-btn>
  <v-text-field v-model="search" label="검색"></v-text-field>
</template>
```


### footer
**테이블의 하단에 사용자 정의 콘텐츠를 추가**할 수 있는 슬롯입니다. **페이지네이션**이나 추가적인 정보를 표시하는 데 사용됩니다.

```vue
<template v-slot:footer>
  <v-pagination v-model="page" :length="pageCount"></v-pagination>
</template>
```


## 행

### item
**각 데이터 항목에 대해 사용자 정의 콘텐츠**를 추가할 수 있는 슬롯입니다. 기본적으로 테이블의 각 행을 구성하는 데 사용됩니다.

```vue
<template v-slot:item="{ item }">
  <tr>
    <td>{{ item.name }}</td>
    <td>{{ item.location }}</td>
    <td>{{ item.height }}</td>
    <td>
      <v-btn @click="editItem(item)">편집</v-btn>
      <v-btn @click="deleteItem(item)">삭제</v-btn>
    </td>
  </tr>
</template>
```


### item.actions
**각 항목의 액션 버튼**을 정의할 수 있는 슬롯입니다. 일반적으로 편집, 삭제 등의 버튼을 추가하는 데 사용됩니다.
```vue
<template v-slot:item.actions="{ item }">
  <v-btn @click="editItem(item)">편집</v-btn>
  <v-btn @click="deleteItem(item)">삭제</v-btn>
</template>
```


### item.{{ columnName }}
각 열의 데이터를 사용자 정의 형식으로 표시할 수 있는 슬롯입니다. 특정 열의 데이터를 커스터마이징할 수 있습니다.

```vue
<template v-slot:item.name="{ item }">
  <strong>{{ item.name }}</strong> <!-- 이름을 강조하여 표시 -->
</template>
```

### select
행 선택 기능을 커스터마이징할 수 있는 슬롯입니다. 체크박스를 사용자 정의할 수 있습니다.

```vue
<template v-slot:select="{ item }">
  <v-checkbox v-model="selectedItems" :value="item.id"></v-checkbox>
</template>
```

## 옵션
### no-data
데이터가 없을 때 표시할 내용을 정의하는 슬롯입니다. 사용자에게 데이터가 없음을 알리는 메시지 등을 추가할 수 있습니다.
```vue
<template v-slot:no-data>
  <v-alert type="info">표시할 데이터가 없습니다.</v-alert>
</template>
```

