# WooriFISA-Docker

# :shark:Docker
도커는 컨테이너화 기술을 통해 애플리케이션과 해당 애플리케이션의 모든 의존성을 독립적인 환경에서 실행할 수 있도록 하는 오픈 소스 플랫폼이다.

## Docker 사용 이유
애플리케이션이 **실행되는 환경과 관계없이 일관된 동작을 보장**할 수 있으며 리소스를 더 효율적으로 사용할 수 있고, 배포 속도도 빠르기 때문에 Docker를 사용한다.

## Docker Image
Docker 컨테이너를 실행하는 데 필요한 파일 시스템과 설정을 포함하는 읽기 전용 템플릿이다.
이미지는 애플리케이션이 필요로 하는 라이브러리, 종속성, 환경 변수, 설정 파일 등을 포함하고 있으며, 이를 기반으로 컨테이너를 생성할 수 있다.

## Docker Image 최적화
배포 속도, 비용 절감, 자원 효율성 개선, 환경 일관성 유지를 하여 전반적인 애플리케이션 성능과 안정성을 높일 수 있어 최적화를 진행한다.

## Dockerfile

### 기획
이미지 크기를 최소화하여 배포 속도를 높여 빌드 속도를 최적화하고, 자원을 효율적으로 쓰게 한다.

### 설계
```java
# Test.java
public class Test {
    public static void main(String[] args) {
        System.out.println("쩡 Dockerfile 구축 test");
    }
}
```
```bash
# Dockerfile
# Step 1: Build stage
FROM openjdk:17-jdk-alpine AS build

# 작업 디렉토리 생성
WORKDIR /usr/src/myapp

# 소스 코드 복사
COPY . /usr/src/myapp

# Java 소스 컴파일
RUN javac Test.java

# Step 2: Run stage
FROM openjdk:17-alpine

# 빌드한 결과물을 복사
WORKDIR /usr/src/myapp
COPY --from=build /usr/src/myapp /usr/src/myapp

# 실행 명령
CMD ["java", "Test"]
```

### 구축
```bash
# Dockerfile을 기반으로 'testimg'이라는 이름의 이미지를 빌드
$ docker build -t testimg .

# 현재 시스템에 존재하는 Docker 이미지 목록을 출력
$ docker images

# 'testimg' 이미지를 사용하여 'test-container'라는 이름의 컨테이너를 생성하고 실행
$ docker run --name test-container testimg

# 현재 시스템에 존재하는 모든 컨테이너(실행 중인 컨테이너와 종료된 컨테이너 포함)를 출력
$ docker ps -a
```
