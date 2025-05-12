# dio
HTTP 요청을 쉽게 처리할 수 있는 라이브러리이다.          
REST API와의 통신을 구현할때 사용된다.
## 다양한 HTTP 메서드를 사용할 수 있다.
다양한 요청을 처리하기 위해         
- GET
- POST
- DELET        

등과 같은 여러 HTTP 메서드를 사용할 수 있다.
## 인터셉터 (Interceptors)
요청, 반응, 에러 등을 가로채서 전처리/후처리할 수 있다.         
예) 토큰 인증 로그인, 에러 핸들링 등           

토큰 인증 로그인을 예로 들어서 설명하면           
클라이언트가 Access Token을 담아 서버에 api 요청을 보내면       
Access Token이 만료될경우 인터셉터인 onError로 가로채서 토큰 검증을 하는 것이다.

## FormData로 파일 업로드

파일 업로드나 ```multipart/form-data``` 요청을 쉽게 보낼 수 있다.        
```dart
FormData formData = FormData.fromMap({
  'name': 'test',
  'file': await MultipartFile.fromFile('path/to/file.jpg', filename: 'file.jpg'),
});
await dio.post('/upload', data: formData);
```
## 요청 옵션 설정 가능
헤더, 쿼리 파라미터,타임아웃 등을 설정할 수 있다

```dart
Response response = await dio.get(
  'https://api.example.com/users',
  options: Options(headers: {'Authorization': 'Bearer token'}),
  queryParameters: {'page': 1},
);
```
헤더, 쿼리 파라미터 예제
```dart
Dio dio = Dio(BaseOptions(
  connectTimeout: const Duration(seconds: 5),
  receiveTimeout: const Duration(seconds: 10),
));
```
타임아웃 에제