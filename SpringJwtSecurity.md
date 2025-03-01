# 쿠키, 세션, 토큰

## 쿠키 인증 방식
### 특징
- Cookie 인증
- Key-Value 문자열 형식
- 클라이언트의 브라우저에 설치되는 기록 정보 파일
### 인증방식
  1. 클라이언트가 서버에 요청(접속)을 보냄
  2. 서버는 응답할 때 클라이언트에 저장하고 싶은 정보를 응답 헤더의 Set-Cookie에 담음.
  3. 이후 클라이언트는 요청을 보낼 때마다 요청 헤더에 Cookie에 담아 보냄.
  4. 서버는 해당 쿠키에 담긴 정보로 주체를 식별함.
### 단점
  - 보안 취약
  - 용량 제한
  - 브라우저 간 차이 때문에 쿠키 공유 X
  - 쿠키 사이즈가 많아지면 서버 부하량이 커짐

## 세션 인증 방식
### 특징
- 쿠키의 보안 이슈 때문에 클라이언트의 민감한 정보를 서버측에서 저장하고 관리함.
- 세션 객체 형태
- Key : SESSION ID 
  Value : 세션 생성 시간, 마지막 접근 시간, User가 저장한 속성
  (Map 형태)
### 인증방식
  1. 유저 로그인 시 세션이 서버 메모리 또는 DB에 저장함(세션 식별을 위해 Session Id를 기준으로 저장함).
  2. 서버에서 브라우저의 쿠키에 Session Id 저장함.
  3. 쿠키에 정보가 있기 때문에 브라우저는 해당 사이트에 대한 모든 요청에 Session Id를 쿠키에 담아 전송함.
  4. 서버는 클라이언트가 보낸 Session Id와 서버 메모리로 관리하고 있는 Session Id를 비교하여 인증함.
### 단점
  - Session Id 자체는 큰 정보가 없지만 탈취 당하면 큰일
  - 요청이 많아지면 서버 부하량이 커짐.

## 토큰 인증 방식
### 특징
- 클라이언트가 서버에 접속 시 서버가 클라이언트에게 유일한 토큰을 부여함.
### 인증방식
  1. 클라이언트가 서버에 접속 요청함.
  2. 서버가 클라이언트에게 토큰 발급함.
  3. 클라이언트는 쿠키나 스토리지에 저장함. 클라이언트가 서버에 요청할 때마다 토큰을 HTTP 요청 헤더에 포함시켜 전달함.
  4. 서버는 받은 토큰을 검증하고 응답함. 토큰에는 정보가 담겨있기 때문에 서버는 DB조회 없이 누가 요청하는지 알 수 있음.
### 단점
  - 쿠키/세션과 다르게 토큰 자체의 데이터 길이가 길어, 인증 요청이 많아지면 네트워크 부하가 심해짐.
  - Payload 자체는 암호화 되지 않기 때문에 중요한 정보를 담을 수 없음.
  - 토큰을 탈취당하면 대처 힘듦(만료 기간설정으로 대처).

## JWT
### 특징
- Header, Payload, Signature의 세 부분으로 나뉨.
#### Header
     '''
     {
       "alg" : "HS256",
       "typ" : "JWT"
     }
     '''

alg : 서명 암호화 알고리즘
typ : 토큰 유형

#### Payload (Registered claims, Public claims, Private claims로 세 가지로 나뉨)
* Claim : key-value 형식으로 이루어진 한 쌍의 정보 

      '''
      {
        "sub" : "12",
        "name" : "Jondae",
        "iat" : "1515566"
      }
      '''

Registed claims : 미리 정의된 클레임
- iss(issuer-발행자)
- exp(expireation time-만료 시간)
- sub(subject-제목)
- iat(issued At-발행 시간)
- jti(JWI ID)

Public claims : 사용자가 정의할 수 있는 클레임 공개용 정보 전달을 위해 사용.

Private claims : 해당하는 당사자들 간에 정보를 공유하기 위해 만들어진 사용자 지정 클레임. 외부에 공개되도 상관없지만 해당 유저를 특정할 수 있는 정보들을 담는다


#### Signature
- 시그니처에서 사용하는 알고리즘 = 헤더에서 정의한 알고리즘.
- 시그니처 : (헤더 + 페이로드) + key값을 헤더에서 정의한 알고리즘으로 암호화한 것.


## jwt.io에서 jwt 토큰 확인 가능
![image](https://github.com/user-attachments/assets/e9e9ae37-6643-4258-a2aa-ed426da45280)

### 인증 과정
  1. 클라이언트가 서버에 로그인 인증을 요청함.
  2. 서버에서 클라이언트로부터 인증 요청을 받으면 Header, PayLoad, Signature를 정의한다. 각각 Base64로 한번 더 암호화하여 JWT를 생성 후 쿠키에 담아 클라이언트에게 발급함.
  3. 클라이언트는 서버로부터 받은 JWT를 로컬 스토리지에 저장함(쿠키나 다른 곳도 가능).
  4. 이후 클라이언트가 서버에 API 요청을 하면 "Authorization" header에 Access Token을 담아서 보냄.
  5. 서버는 받은 Access Token을 헤더에 "Authorization"에서 추출하여 인증 과정을 거침.
  6. 이후 받은 PayLoad에서 정보를 추출하여 로그인을 승인함.
  7. 만약 Access Token이 만료되면 클라이언트는 Refresh Token을 통해서 Access Token을 한번 더 발급 받아야함.

### JWT를 쓰는 이유
: Header, PayLoad, Signature 중 Header나 PayLoad 중 하나를 수정했을 경우 Signature가 달라져 인증이 실패하고 조작되었음을 알 수 있음.
* Token은 정보 전달을 위한 수단이 아님. 위조 방지가 목적임.

### 장점
- 데이터 위변조를 막을 수 있음.
- 인증에 대한 저장소 불필요.
- JWT는 토큰 기본 정보, 전달 정보, 서명 정보를 기본적으로 지님.
- 세션은 클라이언트 인증 정보를 저장하지만, 토큰을 사용하면 서버는 Stateless 상태가 되어 서버 확장성이 올라감.
- 다른 로그인 시스템에 접근 및 권한 공유 가능함(쿠키와 차이점).
- Oauth 경우 다른 소셜 로그인 가능함.
- 앱은 쿠키나 세션이 없기 때문에 토큰을 사용함.
- 토큰 인증 이후 페이로드에서 원하는 정보를 얻는 경우도 있기 때문에 DB 조회 과정이 생략됨.

### 단점
- 토큰 자체에 정보가 있음.
- 페이로드 자체는 암호화 된 것이 아니기 때문에 탈취당하면 데이터가 빠져나감.
- 토큰을 탈취당하면 대처 힘듦.
- 정보가 많아질수록 토큰 길이가 늘어나 네트워크 부하량이 늘어남.

### JWT 탈취 방지를 위한 Access Token / Refresh Token
- Access Token : 유저 정보가 담겨있는 실제 클라이언트가 보유하는 토큰임. 클라이언트에서 요청이 오면 서버에서 해당 토큰을 활용하여 응답함.
- Refresh Token : Access Token의 기간이 만료되면 재발급 하기 위한 토큰임. Refresh Token에는 오직 Access Token을 발급하기 위한 정보만 존재. 보통 DB에 유저정보와 함께 기록함.



# <JWT 구현 필수 요소 (Spring Security)>
## Provider
1.  JWT 생성(createAccessToken() + createRefreshToken()) + 검증(vaildateToken) 
    : Access Token + RefreshToken + Validation
2.  JWT 생성(createAccessToken() + 검증(vaildateToken)
    : Access Token + Validation
    
## JwtFilter
- 요청 들어올 때마다 Authorization 헤더에서 JWT 추출, 검증하고 JWT가 유효하면 사용자를 SecurityContext에 저장
## Config(webSecurityConfig)
Spring Security에 JWT 인증 적용 후 필요한 설정
- jwt필터 설정(SecurityFilterChain)
- CORS 설정(CorsConfigurationSource)
- JWT 인증 필터 등록 (JwtAuthenticationFilter)
- OAuth2 로그인 설정 (OAuth2UserServiceImplement)
- 예외 처리 (인증 실패 시 AuthenticationFailEntryPoint)

# <JWT 받아오는 흐름>

1. 클라이언트가 로그인 API 요청 (ex - /api/v1/login)
2. 서버가 create()를 호출해서 JWT 생성, 성공 시 클라이언트에 반환
3. 클라이언트는 받은 JWT를 저장
4. 이후 클라이언트가 API 요청 시 Authorization: Bearer {JWT} 헤더에 포함해서 전달
5. 서버에서 Filter로 JWT 검증, 유효하면 SecurityContext에 사용자 정보 저장
6. SecurityContext에서 사용자 정보 가져와서 요청 처리, @AuthenticationPrincipal로 인증된 사용자 정보 조회 가능

SecurityContext는 요청 받으면 로컬 스레드(메모리)에 저장이 되고, 요청이 끝나면 삭제되는 방식

- jwt Token Signiture 암호화 방식 (jjwt 의존성)
  1. 인코딩된 byte array : SecretKey key = Keys.hmacShaKeyFor(encodedKeyBytes);
  2. Base64 인코딩된 문자열 : SecretKey key = Keys.hmacShaKeyFor(Decoders.BASE64.decode(secretString));
  3. Base64URL 인코딩된 문자열 : Keys.hmacShaKeyFor(Decoders.BASE64URL.decode(secretString));
  4. 인코딩 안된 문자열 : Keys.hmacShaKeyFor(secretString.getBytes(StandardCharsets.UTF-8));

# 실제 예시
1. protected(나머지 필터는 private을 사용) 필터에서 API 요청 들어올 때마다 try-catch로 헤더에서 토큰 추출 후 검증함.
2. 검증할 때 provider에서 validate 검증을 사용함(토큰을 넣어서).
3. 추출 검증 다 되면 시큐리티 컨텍스트에 등록함. 이후 에러처리 필수. 다음 필터에 전달함.
4. 이후 시큐리티 컨텍스트에 등록을 성공하면 jwt가 유효하다는 말.
5. 다음 필터에 전달하면 요청 헤더에서 그 bearer 토큰 형식의 jwt를 추출함(bearer 형태로).
6. 추출 성공한다면 사용자 정보를 시큐리티 컨텍스트에 저장함.
7. 'private void setContext'에서 요청 request(인자1)랑 String userId(인자2) 를 받음.
8. 이후 바로 AccessToken을 생성함.
9. 이러면 접근 주체와 비밀번호 같은 것을 쓰려면 쓰고 권한을 써야함. 생성할 때 써야함. 접근 주체는 유저 id, 비밀번호는 null, 권한은 없다고 작성함.
10. 그런데 어떤 요청을 한건지 상세 정보를 추가해야 하는데 setDetails 제공해주는걸 쓰면됨. 예시 -> "authenticationToken.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));" 이런 식으로 하면 추가됨.
11. bean security context 생성하고 authentication token 주입하고 이러면 끝남. 방금 생성한 security Context 등록하면 필터 끝
