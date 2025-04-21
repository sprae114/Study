# Controller
`map<String, ShortenURL>`

1. 단축 URL 생성 API (post? get?)
```
1) service : 단축키 알고리즘을 사용해 key 8글자 만들기
2) map[shortenURLKey] = ShortenURL 으로 저장
3) return shortenURL;
```

2. 단축 URL 리다이랙트 API (get)
```
1) ShortenURL.getCount() += 1 이후 다시 저장
2) ShortenURL.getoriginalUrl()으로 원래 주소 가져오기
3) return originalURL;
```

3. 단축 URL 정보 조회 API (post)
```
1) map[shortenURL] 리스트 정보 가져오기
3) ResponseDTO 만들기
```

```json
{
 shortenURL : "",
 originalURL : "",
 count : 0
}
```