# Docker Image Optimization

|<img src="https://avatars.githubusercontent.com/u/82391356?v=4" width="120" height="120"/>|
|:-:|
[@이정민](https://github.com/jjeong1015) 

# 🦈 왜 Docker를 써야 하는가?
Docker는 컨테이너 기술을 통해 애플리케이션과 해당 애플리케이션의 모든 의존성을 독립적인 환경에서 실행할 수 있도록 하는 오픈 소스 플랫폼으로, **환경에 구애받지 않고 애플리케이션을 신속하게 배포 및 확장**할 수 있는 장점이 있다.

## 🖼️ Docker Image란?
Docker Image는 Docker 컨테이너를 실행하는 데 필요한 파일 시스템과 설정을 포함하는 읽기 전용 템플릿이다.<br>
애플리케이션이 필요로 하는 **라이브러리, 종속성, 환경 변수, 설정 파일** 등을 포함하고 있으며, 컨테이너를 생성할 수 있다.

## 🚀 Docker Image 최적화
Docker Image 최적화를 하면 배포 속도를 높이고 자원을 효율적으로 사용하며, 애플리케이션의 성능과 안정성을 높일 수 있다.

## 📄 Dockerfile 최적화 과정

### 🎯 기획 의도
1. 멀티 스테이지 빌드: 빌드를 나누어 이미지 크기를 최소화
2. Alpine 사용: 가벼운 리눅스 배포판으로 이미지 크기 줄이기
3. .dockerignore: 불필요한 파일을 제거해 이미지 최적화

### 🏗️ 설계
**폴더 구조**<br>
![image](https://github.com/user-attachments/assets/a04a2c0f-e636-48c0-975e-773f58e65786)

**코드**<br>
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
```bash
# .dockerignore
# Java 컴파일 시 생성되는 .class 파일을 제외
*.class

# 로그 파일을 제외
*.log

# Node.js 프로젝트의 의존성 폴더인 node_modules를 제외
node_modules

# Git 리포지토리의 버전 관리 정보를 제외
.git
```

### 🛠️ 구축
```bash
# Dockerfile을 기반으로 'testimg'이라는 이름의 이미지를 빌드
$ docker build -t testimg .

# 현재 시스템에 존재하는 Docker 이미지 목록을 출력
$ docker images

# 'testimg' 이미지를 사용하여 'test-container'라는 이름의 컨테이너를 생성하고 실행
$ docker run --name test-container testimg

# 현재 시스템에 존재하는 모든 컨테이너를 출력
$ docker ps -a
```

![image](https://github.com/user-attachments/assets/e1fd482b-ac10-40a3-9fe7-f5a1bd7d3c50)

### 📊 이미지 크기 비교
![image](https://github.com/user-attachments/assets/9a2b21aa-6e23-441c-97f8-d0eeb41677b9)
<br><br>**default, slim, alpine을 다 실행해보며 이미지 크기를 비교**해보았다.
<br>testimg : alpine, testimg2 : slim, testimg3 : default

**이미지 크기 : alpine < slim < default**는 이러며, 이미지 크기가 작을수록 자원을 아끼고 속도가 빨라진다.
