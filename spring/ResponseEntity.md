ResponseEntity는 공식에서 이렇게 설명한다.

**Extension of HttpEntity that adds an HttpStatusCode status code.** 

말 그대로 해석해보자면 HttpStatusCode 상태 코드를 추가하는 HttpEntity의 확장이라는 소리다.

이때 HttpEntity란 무엇이냐!!

HttpEntity : 

header와 body로 구성된 HTTP요청, 혹은 응답엔티티를 표현한다.

보면 ResponseEntity는 HttpEntity를 상속받았음을 알 수 있다.

[##_Image|kage@bP5Ktq/btslaJx30p4/TGC3vq9JFZofnbtMiEA3lk/img.png|CDM|1.3|{"originWidth":2030,"originHeight":950,"style":"alignCenter"}_##]

HttpStatusCode : HTTP 상태코드의 열겨형이다. (enum)

인터넷을 하다보면 가끔 404 에러를 본 적이 있을텐데, 이런 http 상태에 대한 코드들이 열겨되어있는 enum이 HttpStatusCode이다.

보통 많이 사용하는건 

HttpStatus.OK (200)

HttpStatus.NOT\_FOUND(404) 인것 같다. 

[##_Image|kage@bIFO6a/btslneXlCcp/BvbDtiqz3Hw24gDp45VQ10/img.png|CDM|1.3|{"originWidth":1210,"originHeight":632,"style":"alignCenter"}_##]

나는 코드에서 아래와 같이 사용하였다.

"success" 라는 값과, HttpStatus.OK라는 상태코드 값을 전해준다.

```
@DeleteMapping("/{rno}")
public ResponseEntity<String> remove(@PathVariable("rno") Long rno){
    log.info("RNO : " + rno);
    replyService.remove(rno);
    return new ResponseEntity<>("success",HttpStatus.OK);
}
```

이를 받는 쪽에서는 

```
success: function(result){
  console.log("result: " + result);
```

이런식으로 result 값으로 가져와 콘솔창에 띄워보면

result : success 라고 나오는걸 확인할 수 있다.