# JRE와 JDK
### JRE (Java Runtime Environment)

- 클래스 라이브러리, 로드 클래스 및 JVM이 포함된다.
- 즉, java 프로그램을 실행하려면 JRE가 필요하다.
    - JRE만 설치하면 java 프로그램을 실행할 수 있다.
    - class 파일 혹은 jar 파일, exe를 실행되게 만들어준다.
    - 일반적으로는 jar을 exe로 만들어도 jre는 필요하며 jre 파일을 포함한 exe를 만들면 자바가 설치되지 않은 환경에서도 실행할 수 있다.

    
- 자바 프로그래밍 도구는 포함되어있지 않아서 자바 프로그래밍을 하기 위해서는 JDK가 필요하다.
</br>

### JDK (Java Development Kit)

- JDK는 개발에 필요한 도구 (javac, java등)들을 포함한다.
- JDK = JRE + @라서 생각하면 된다.
    - 컴파일러, java 애플리케이션 시작 프로그램 등을 포함한다.
- 동일한 컴퓨터에 둘 이상의 JDK 버전을 설치할 수 있다.
- java 프로그래밍을 수행할 계획이 없더라도 JDK를 설치해야 할 수 있다.
    - JSP를 사용하여 웹 애플리케이션을 배포하면 jdk가 필요하다 (애플리케이션 서버는 jsp를 서빌릿으로 변환하고 jdk를 사용하여 servlet을 컴파일해야하기 때문)

### 참고
[[JAVA] JDK와 JRE의 차이](https://coding-nyan.tistory.com/90)
    
[자바 exe 실행파일 만들기 - Launch4j](https://digiconfactory.tistory.com/entry/%EC%9E%90%EB%B0%94-exe-%EC%8B%A4%ED%96%89%ED%8C%8C%EC%9D%BC-Launch4j)
    
[[자바] Launch4j를 이용한 jar파일 실행파일(exe)로 만들기 (jre포함)](https://hydok.tistory.com/15)