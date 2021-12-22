# JWT
![image](https://user-images.githubusercontent.com/20410193/147110978-b755d2fb-8875-433b-8286-26cfcee29494.png)


- JWT는 헤더(header), 정보(payload), 서명(signature) 구조로 이루어져 있음

  - **header**

    JWT 토큰을 어떻게 해석해야 하는지, 즉 **암호화 방식**을 명시한 부분. **타입**과 **알고리즘**이 담김

    이 헤더를 열어보면 Payload와 Signature를 어떻게 해석할지를 알 수 있음

    ```json
    { 
      "typ":"JWT",
      "alg":"HS256"
    }
    ```

    이를 **base64**로 **인코딩**해서 헤더를 생성.

  - **payload**

    실제 토큰의 **바디에 포함**할 내용.

    JWT는 토큰을 **디코딩**해서 바로 **값을 확인**할 수 있는 구조로 되어 있어서 데이터베이스 조회할 필요없이 사용자 아이디 등을 여기에 담아서 바로 사용할 수 있음. 그 외에도 보통 발급 시간(iat), 만료기간(exp) 등이 객체형으로 담김

    ```json
    {
      "iss":"John Doe",
      "exp":1434290400000,
      "username":"john",
      "age":25,
      "iat":1434286842654
    }
    ```

    헤더와 같게 **base64 인코딩**

  - **signature**

    **무결성**을 보장하는 부분. **header, payload**를 인코딩 한 값을 합친뒤 **SECRET_KEY로 해쉬**

    JWT 같은 경우 header와 payload 를 누구나 까볼 수 있음. payload 를 까본 다음, 원래의 user id 를 변경한다거나 새로운 정보를 추가해서 정보를 수정하고 다시 base64 로 인코딩 하는 식으로 기존의 payload 위변조 가능.

    따라서 헤더와 페이로드의 위변조 여부를 검증할 수 있어야하는데 이 수단이 바로 signiture

    **헤더와 페이로드**를 base64로 인코딩해서 만든 두 값을 마침표(.)로 이어 붙이고 **헤더**에서 alg로 지정한 **알고리즘** (HS256) 으로 **암호화**를 통해 시그니처 생성해서 JWT 의 맨 마지막에 붙이는 것

    ```javascript
    var secretKey = 'secret';
    var header = 'aaa';
    var payload = 'bbb';
    var headerAndPayload = header + '.' + payload;
    
    crypto.createHmac('sha256', secretKey).update(headerAndPayload).digest('base64')
    ```

    만약 유저가 보낸 토큰의 header, payload 를 통해 자체적으로 생성한 시그니처 값과, 유저가 보낸 토큰의 signiture 값이 불일치한다면 변조된 것임을 알 수 있음

    이때 공격자가 시그니처를 위변조할 수 없도록 비밀키를 사용해서 암호화를 하는데, 당연히 이 비밀키는 외부에 노출되면 안됨.

- **header**와 **payload**는 암호화를 한 것이 아니라 **단순히 JSON문자열을 base64로 인코딩**한 것 뿐이므로 **누구나** 이 값을 다시 **디코딩**하면 JSON에 어떤 **내용**이 들어있는지 **확인**할 수 있음. 따라서 **민감한 내용**을 넣어서는 **안됨**
- **signature** 를 통해 해당 **토큰 변조 여부** 확인가능. 
  1. 클라 -> 서버로 토큰 보냄
  2. 서버는 토큰에서 header 와 payload 값을 합친 뒤 SECRET_KEY로 해쉬
  3. 클라에서 받은 토큰의 signature 값과, 2번에서 자체적으로 생성한 해쉬 값 비교. 값이 다르면 변조된 것임을 알 수 있음
  4. 따라서 이 secret key가 노출되면 JWT 토큰을 위조할 수 있으므로 비밀키를 철저히 숨겨야 함
- 토큰만 가지고 유효성 검사나 사용자 식별까지 할 수 있으므로 서버를 **stateless**로 유지할 수 있음
- 하지만 앞에서 보았듯이 **페이로드**은 암호화하지 않기 때문에 서명 없이도 **누구나 열어볼 수** 있음. 따라서 여기에는 **보안이 중요한 데이터는 넣으면 안 됨**. base64로 인코딩해서 사용하다 보면 이 부분을 간과하기 쉬운데 필요한 최소한의 정보만 페이로드에 담아야 함
- 인코딩 특성상 페이로드의 내용이 많아지면 토큰의 길이도 같이 길어져 **용량이 커짐**. 앞에서 정의된 페이로드의 key를 전부 약자를 사용하는 이유도 JWT를 최대한 짧게 만들기 위함.
- 토큰을 **강제로 만료**시킬 **방법이 없**음. 클라이언트가 로그아웃하더라도 해당 토큰을 클라이언트가 제거하는 것뿐이지 토큰 자체가 만료되는 것은 아님. 만약 이때 누군가 토큰을 **탈취**한다면 해당 토큰을 **만료시간까지는 유효**.



# 출처

- [JWT 토큰 인증](https://velog.io/@haebin/JWT-%ED%86%A0%ED%81%B0-%EC%9D%B8%EC%A6%9D)
- [JWT(JSON Web Token)에 대한 이해, 로그인의 활용](https://velog.io/@jguuun/JWTBasic)
- [JWT](https://jwt.io/)
- - [JWT(JSON Web Token)에 대해서...](https://blog.outsider.ne.kr/1160)

