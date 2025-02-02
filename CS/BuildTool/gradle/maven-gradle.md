# maven 프로젝트 gradle로 변경하기

> 예전 개인적으로 만들어 두었던 eisenUtils 라이브러리에 몇가지 메서드를 추가하기로 했습니다. 그런데 몇가지 수정해야할 부분들이 보이더군요. 작년 딱 이맘때쯤에 github의 eisenUtils repo를 생성했었습니다. 확인해보니 필요없는 파일들도 많이 올라가져 있고 eclipse와 maven으로 생성되었더군요. 그래서 이번에 maven을 graddle로 변경해 보기로 했습니다. 참고: [여기](https://shanepark.tistory.com/360)를 참고하여 변경해 보았습니다.

# maven to gradle

## why gradle?

>  https://gradle.org/maven-vs-gradle/

위의 링크에서 볼 수 있듯이 maven과 비교했을 때 performance 차이가 생각보다 큽니다.

자세한 내용은 링크에서 확인하실 수 있습니다.

## gradle 설치

### 1. 다운로드

아래 링크에서 파일을 다운로드 합니다.

>  https://gradle.org/releases/  

![image-20230214092212760](https://raw.githubusercontent.com/KrGil/TIL/4e24259dd20ae165a3d2998def375f37f20c0185/CS/BuildTool/gradle/maven-gradle.assets/image-20230214092212760.png)

### 2. 환경 변수 편집

환경변수 설정에서 Path에 C:\Gradle\gradle-8.0\bin 추가 합니다.

![image-20230214092828343](https://raw.githubusercontent.com/KrGil/TIL/4e24259dd20ae165a3d2998def375f37f20c0185/CS/BuildTool/gradle/maven-gradle.assets/image-20230214092828343.png)

터미널에서(저는 git bash를 사용했습니다.) 켜고 `gradle -v` 를 실행해 잘 설치되었는지 확인해 봅니다.

![image-20230214092701533](https://raw.githubusercontent.com/KrGil/TIL/4e24259dd20ae165a3d2998def375f37f20c0185/CS/BuildTool/gradle/maven-gradle.assets/image-20230214092701533.png)



## maven to gradle

### gradle init

`gradle -v` 커멘드로 graddle이 설치된 것을 확인하셨다면 변환할 폴더(`pom.xml`이 위치한)로 이동합니다.

eisenUitls 프로젝트의 pom.xml이 있는 폴더 위치에서 `gradle init`을 실행합니다.  

![image-20230214093013188](https://raw.githubusercontent.com/KrGil/TIL/4e24259dd20ae165a3d2998def375f37f20c0185/CS/BuildTool/gradle/maven-gradle.assets/image-20230214093013188.png)

build scirpt DSL을 선택하라고 하는데 저는 Groovy에 눈꼽만큼 좀 더 익숙하기에 Groovy를 선택했습니다.

또한 새로운 API 들을 생성할것이냐고 묻는데 해당 API는 바뀔 수도 있다고 해서 no를 입력했습니다.

![image-20230214093112650](https://raw.githubusercontent.com/KrGil/TIL/4e24259dd20ae165a3d2998def375f37f20c0185/CS/BuildTool/gradle/maven-gradle.assets/image-20230214093112650.png)

Build가 성공했습니다.

![image-20230214093159123](https://raw.githubusercontent.com/KrGil/TIL/4e24259dd20ae165a3d2998def375f37f20c0185/CS/BuildTool/gradle/maven-gradle.assets/image-20230214093159123.png)

### gradle setting

`gradle init` 커맨드가 성공적으로 실행 되었으면 몇가지 gradle 파일이 추가된 것을 확인할 수 있습니다. 이제 폴더에 존재하는 pom.xml파일은 삭제해 줍니다.

![image-20230214093236007](https://raw.githubusercontent.com/KrGil/TIL/4e24259dd20ae165a3d2998def375f37f20c0185/CS/BuildTool/gradle/maven-gradle.assets/image-20230214093236007.png)

그 후 intellij로 프로젝트를 open 하면 아래와 같이 `Gradle build scripts found`라는 문구가 우측 하단에 뜹니다. `Load Gradle Project`를 클릭해 줍니다.

![image-20230214093358073](https://raw.githubusercontent.com/KrGil/TIL/4e24259dd20ae165a3d2998def375f37f20c0185/CS/BuildTool/gradle/maven-gradle.assets/image-20230214093358073.png)

## Test

Converting이 성공적으로 이루어 졌는지 Test를 작성해서 확인해 봅니다.

![image-20230214095015450](https://raw.githubusercontent.com/KrGil/TIL/4e24259dd20ae165a3d2998def375f37f20c0185/CS/BuildTool/gradle/maven-gradle.assets/image-20230214095015450.png)

위와 같이 `No tests found for given includes`라는 오류가 나옵니다. 

https://stackoverflow.com/questions/30474767/no-tests-found-for-given-includes-error-when-running-parameterized-unit-test-in

![image-20230214095209733](https://raw.githubusercontent.com/KrGil/TIL/4e24259dd20ae165a3d2998def375f37f20c0185/CS/BuildTool/gradle/maven-gradle.assets/image-20230214095209733.png)

stackoverflow에서 발췌한 글입니다. 변환 시 test 환경은 따로 고려하지 않는다는군요. 직접 추가해 줍니다.

```java
test {
    useJUnitPlatform()
}
```

테스트가 성공적으로 완료된 것을 확인할 수 있습니다.

![image-20230214130626506](https://raw.githubusercontent.com/KrGil/TIL/4e24259dd20ae165a3d2998def375f37f20c0185/CS/BuildTool/gradle/maven-gradle.assets/image-20230214130626506.png)

### build.gradle

최종적인 소스코드 입니다.

```java
/*
 * This file was generated by the Gradle 'init' task.
 */

plugins {
    id 'java-library'
    id 'maven-publish'
}

repositories {
    mavenLocal()
    maven {
        url = uri('https://repo.maven.apache.org/maven2/')
    }
}

dependencies {
    api 'javax.validation:validation-api:2.0.1.Final'
    testImplementation 'org.junit.jupiter:junit-jupiter:5.8.1'
}

group = 'com.tistory.eisen'
version = '1.01'
description = 'eisenUtils'
java.sourceCompatibility = JavaVersion.VERSION_1_8

publishing {
    publications {
        maven(MavenPublication) {
            from(components.java)
        }
    }
}

test {
    useJUnitPlatform()
}
```

이제 수정한 작업들을 github에 `push` 해서 적용시켜 봅니다.



## deploy

maven -> gradle로 변경한 기념으로 버전을 1.0.1로 수정해서 배포해 봅니다.

> jitpack을 이용한 배포 방법은 [JitPack을 활용하여 라이브러리 생성하기(maven, gradle)](https://jjam89.tistory.com/216) 글에서 확인할 수 있습니다.



### github

gihub repo에서 release를 새로 따줍니다.

![image-20230214131525291](https://raw.githubusercontent.com/KrGil/TIL/4e24259dd20ae165a3d2998def375f37f20c0185/CS/BuildTool/gradle/maven-gradle.assets/image-20230214131525291.png)



잘 배포되었는지 확인해 봅니다. jitpack 사이트에서 제가 배포한 프로젝트의 log들을 확인할 수 있습니다.

> https://jitpack.io/com/github/KrGil/eisenUtils/1.0.1/build.log

![image-20230214130835385](https://raw.githubusercontent.com/KrGil/TIL/4e24259dd20ae165a3d2998def375f37f20c0185/CS/BuildTool/gradle/maven-gradle.assets/image-20230214130835385.png)

로그 보고 확인했는데 `build.gradle`에 version을 1.01로 잘못 기입했었네요. 



## 사용

이제 사용할 곳에서 수정된 버전으로 사용해 봅니다.

개인적으로 사용하는 daily 프로젝트에서 사용하고 있는데 아래와 같이 수정해 줍니다.

```java
// implementation ('com.github.KrGil:eisenUtils:master-SNAPSHOT')
implementation ('com.github.KrGil:eisenUtils:1.0.1')
```

변경 후 gradle build를 새로 하면 아래와 같이 새로운 버전을 다운받는 것을 확인할 수 있습니다. 

![image-20230214131112394](https://raw.githubusercontent.com/KrGil/TIL/4e24259dd20ae165a3d2998def375f37f20c0185/CS/BuildTool/gradle/maven-gradle.assets/image-20230214131112394.png)



긴 글 읽느라 고생하셨습니다. maven을 사용하는 프로젝트를 gradle로 변경해 보았습니다. 처음에 잘 진행될 줄 알았는데 생각보다 오래 걸렸네요. 아직 많이 부족한 느낌이 드네요. 즐거운 코딩 되시길 바랍니다.

감사합니다. 
