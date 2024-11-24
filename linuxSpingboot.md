# 리눅스 스프링부트 실행 방법
## 목차
- [패키지 목록 업데이트](#패키지-목록-업데이트)
- [Git 설치](#git-설치)
- [Git 설치 확인](#git-설치-확인)
- [Git 기본 설정](#git-기본-설정)
- [Git 설정 확인 및 clone](#git-설정-확인-및-clone)
- [Java 및 Gradle 설치](#java-및-gradle-설치)
- [의존성 캐시 제거 및 설치](#의존성-캐시-제거-및-설치)
- [애플리케이션 실행](#애플리케이션-실행)

## 1. 패키지 목록 업데이트
```
sudo apt update
```
## 2. Git 설치
```
sudo apt install git
```
## 3. Git 설치 확인
```
git --version
```
## 4. Git 기본 설정
```
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```
## 5. Git 설정 확인 및 clone
```
git config --list
```
```
git clone [clone url 주소]
```
## 6. Java, Gradle 설치 및 설치 확인
#### 6.1 Java, Gradle 설치
```
sudo apt install openjdk-11-jdk
sudo apt install gradle
```
#### 6.2 Java, Gradle 설치 확인
```
java -version
gradle -v
```
## 7. 의존성 캐시 제거 및 설치
```
./gradlew clean
./gradlew build
```
## 8. 애플리케이션 실행
```
java -jar [~~자바파일명].jar # 위 tree 구조를 보고 위치 참고
./gradlew bootRun
```
