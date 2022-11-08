## PRG Post/Redirect/Get 가 왜 필요할까?
- [ ] PRG Post/Redirect/Get 가 필요한 이유는?
	- 상품 등록 후, 웹 브라우저의 새로고침을 하면 상품이 계속 등록되는 문제점(POST 멱등문제)
	- ![](https://api.transno.com/v3/document_image/78fcc54a-c441-4b02-9b7e-4b315b2b4d00-10826299.jpg)  
        -   코드로 보기
        - ![](https://api.transno.com/v3/document_image/941f78f8-67cc-4d47-b710-79fe754bbeed-10826299.jpg)  
            
    
    -   해결법  
        
        -   상품 저장 후에 뷰 템플릿으로 이동하는 것이 아니라, 상품 상세 화면으로 리다이렉트를 호출해주면 된다. 이러면 상품 저장 후에 실제 상품 상세 화면으로 다시 이동![](https://api.transno.com/v3/document_image/b414f6a8-4cca-4340-b03e-1119a6d7b3e9-10826299.jpg)  
            
        
        -   코드로 보기![](https://api.transno.com/v3/document_image/ccc65944-bacd-4e59-afd8-b7ed9a2f5afa-10826299.jpg) 
- [ ] RedirectAttributes  
    -   상품 상세 화면에 "저장되었습니다"라는 메시지를 보여달라는 요구사항  
    -   redirectAttributes 담고 뷰 템플릿에 메시지 추가하기  
        -   언제 어떤 Attribute에 담는 것일까?