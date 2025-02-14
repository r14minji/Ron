# Ron
Backend for Frontend Server

![image](https://github.com/user-attachments/assets/b380a1f8-e2aa-428c-bde4-c480caf07f8b)

# Spring Boot Security & JWT 기반 인증 시스템 학습 로드맵

이 프로젝트는 Spring Boot와 JWT를 활용하여 회원가입, 로그인, 로그아웃, 토큰 갱신 기능을 구현하는 것입니다. 아래는 해당 기능들을 구현하기 위한 학습 로드맵입니다.

## 1. 기본 개념 학습

### Spring Security 기초
- Spring Security의 개념과 동작 방식
- 필터 기반 아키텍처 이해
- 기본 인증 및 인가 과정

### JWT 개념
- JWT(JSON Web Token)의 구조 (Header, Payload, Signature)
- JWT의 장점과 단점
- Access Token vs Refresh Token
- 토큰 만료 및 갱신 전략

### OAuth 2.0 & JWT (필요 시 참고)
- OAuth 2.0 개념
- Spring Security와 OAuth 2.0의 관계

## 2. 개발 환경 설정

### Spring Boot 프로젝트 설정
- `spring-boot-starter-web`, `spring-boot-starter-security`, `spring-boot-starter-data-jpa` 의존성 추가
- JWT 관련 라이브러리 (`jjwt` 또는 `auth0/java-jwt`) 추가
- 데이터베이스 설정 (H2, MySQL 등)

### Spring Security 기본 설정
- `SecurityConfig` 클래스 생성
- `UserDetailsService`와 `PasswordEncoder` 설정

## 3. 회원가입 (Signup) 기능 구현

### 사용자 엔티티 및 DB 설계
- `User` 엔티티 생성 (id, email, password, roles 등)
- JPA를 활용한 `UserRepository` 구현

### 비밀번호 암호화
- `BCryptPasswordEncoder`를 사용하여 비밀번호 저장

### 회원가입 API 구현
- `UserService`에서 회원 정보 저장
- 예외 처리 및 중복 가입 방지

## 4. 로그인 (Authentication) 및 JWT 생성

### 로그인 요청 처리
- `/login` API에서 사용자 인증 처리
- 사용자 정보 검증 후 JWT 발급

### JWT 생성 및 반환
- `JwtProvider` 또는 `JwtUtil` 클래스 생성
- `sign()` 메서드를 활용한 JWT 토큰 생성
- `@RestControllerAdvice`를 활용한 예외 처리

### JWT 반환 방식
- HTTP 응답 헤더에 토큰 포함 (`Authorization: Bearer {token}`)
- 또는 Body에 JSON 형태로 반환

## 5. JWT 기반 인증 및 인가 (Authorization)

### JWT 필터 구현
- `OncePerRequestFilter`를 상속하여 JWT 검증 필터 생성
- JWT를 검증하고 `SecurityContextHolder`에 사용자 정보 저장

### Spring Security와 JWT 연동
- Security 설정에서 JWT 필터를 `UsernamePasswordAuthenticationFilter` 앞에 배치
- 특정 API 보호 (예: `/api/v1/users` 접근 제한)

### 권한(Role) 기반 접근 제어
- `@PreAuthorize("hasRole('ROLE_ADMIN')")`
- `HttpSecurity`에서 경로별 접근 제한 설정

## 6. 로그아웃 구현

### JWT 무효화 처리 전략
- JWT는 자체적으로 무효화할 수 없으므로 Redis 등을 활용한 블랙리스트 관리
- 클라이언트에서 토큰 삭제 (LocalStorage, Cookie 등)

### 로그아웃 API 구현
- Redis를 활용하여 만료된 토큰 저장 후 필터에서 차단
- 또는 Access Token을 클라이언트에서 삭제하는 방식

## 7. 토큰 재발급 (Refresh Token)

### Refresh Token 저장 전략
- DB 또는 Redis에 Refresh Token 저장
- 만료 시간 설정 및 보안 고려

### Refresh Token을 활용한 재발급 API 구현
- Access Token이 만료되었을 때 Refresh Token을 이용하여 새로운 Access Token 발급
- Refresh Token의 보안 관리 (쿠키 저장, Redis 활용 등)

## 8. 실전 적용 및 추가 보안 고려

### CORS 설정
- 프론트엔드와 연동 시 CORS 문제 해결

### JWT 만료 및 예외 처리
- `ExpiredJwtException` 등 예외 처리

### 로그인 실패 및 알림 기능
- 특정 횟수 이상 로그인 실패 시 계정 잠금 등 추가 보안 설정

### OAuth 2.0 연동 (선택 사항)
- Google, Kakao, Naver 로그인 연동
