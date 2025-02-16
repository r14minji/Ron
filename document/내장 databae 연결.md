# Spring Boot에 H2 내장 DB 설정하기

Spring Boot 프로젝트에서 **H2 내장 데이터베이스**를 설정하는 방법을 Gradle을 사용하는 환경에서 설명합니다.

## 1. Gradle 의존성 추가

`build.gradle` 파일에 **H2 의존성**을 추가합니다.

```gradle
dependencies {
    // H2 데이터베이스 의존성 추가
    implementation 'com.h2database:h2'
}
```

## 2. Application.properties 파일 설정
```properties
# H2 데이터베이스 설정
spring.datasource.url=jdbc:h2:mem:testdb     # 메모리 내장 DB 사용 (애플리케이션 종료 시 데이터 삭제)
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa                  # 기본 사용자명
spring.datasource.password=                 # 기본 비밀번호
spring.datasource.initialization-mode=always   # 애플리케이션 시작 시 데이터베이스 초기화
spring.datasource.schema=classpath:h2/schema.sql  # schema.sql 파일 경로
spring.datasource.data=classpath:h2/data.sql    # data.sql 파일 경로

# H2 콘솔 설정 (웹 인터페이스)
spring.h2.console.enabled=true                # H2 콘솔 활성화
spring.h2.console.path=/h2-console            # H2 콘솔 경로 설정
```

## 3. schema.sql 및 data.sql 파일 생성
### schema.sql
```
CREATE TABLE users (
    id VARCHAR(71) NOT NULL,
    name VARCHAR(71) NOT NULL,
    age INT NOT NULL,
    PRIMARY KEY(id)
);
```

### data.sql
```
INSERT INTO users (id, name, age) VALUES ('123', 'rim', 20);
```

## 4. H2 콘솔에 접속하기
### [h2 접속](http://localhost:8080/h2-console)



