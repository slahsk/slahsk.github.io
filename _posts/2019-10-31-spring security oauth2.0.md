
https://github.com/spring-cloud-samples/authserver


spring cloud security authserver
authorization_code


GET
http://localhost:8080/uaa/oauth/authorize?response_type=code&client_id=acme&redirect_uri=http://localhost:8080&scope=openid

로그인 화면 나온다..

시큐리티 사용자 로그인

user/password



로그인하면 스코프 사용 할꺼냐 하는 페이지 가 나오면서 승인 하면

redirect_uri 뒤에 쿼리 파리터에 ?code=?????  가 추가 되어 나온다.

이 코드는 1회성 이다.


이코드 가지고 토큰을 발행 받아야 한다.

POST
http://localhost:8080/uaa/oauth/token

헤더

Authorization : Basic YWNtZTphY21lc2VjcmV0
Content-Type : application/x-www-form-urlencoded


	Authorization 에서 Basic 뒤에 있는 값은 



	acme:acmesecret base64 인코딩 하면 된다.

	client_id:client_secret


	https://www.base64encode.net/



	code : tcyPhq
	redirect_uri : http://localhost:8080
	grant_type : authorization_code


실행하면 토큰값을 얻을 수있다.



