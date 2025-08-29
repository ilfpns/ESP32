# Multipart

⇒ HTTP에서 한 번의 요청/응답 안에 여러 개의 파트를 나누어 담는 방식

### 사용 이유

⇒ 보통 HTTP POST 요청은 본문(body)에 데이터를 하나만 넣을 수 있다

- 예: `Content-Type: application/json` → JSON 문자열 하나
- 예: `Content-Type: text/plain` → 텍스트 하나

그런데 파일 업로드처럼 텍스트 필드 + 바이너리 파일을 같이 보낼 때 합치기 어려움

## 구조

```python
POST /upload HTTP/1.1
Host: example.com
Content-Type: multipart/form-data; boundary=----MyBoundary123
Content-Length: ...

------MyBoundary123
Content-Disposition: form-data; name="username"

yeoms
------MyBoundary123
Content-Disposition: form-data; name="image"; filename="photo.jpg"
Content-Type: image/jpeg

<여기에 JPEG 파일 바이너리 데이터>
------MyBoundary123--
```

여기서 boundary(경계 문자열) 가 파트를 구분함

- `-----MyBoundary123` → 시작
- `-----MyBoundary123--` → 끝
