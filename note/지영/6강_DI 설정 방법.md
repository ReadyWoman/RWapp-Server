# 6강. DI 설정 방법
- XML 파일을 이용한 DI 설정 방법
- JAVA를 이용한 DI 설정 방법 (어노테이션 사용, 추천하고 싶지는 않음)
- XML과 JAVA를 같이 사용

### 6-1. XML 파일을 이용한 DI 설정 방법
- 그동안 살펴 본 방식
- 생성자를 이용해서 필드를 초기화 시켜 줄 때는 <constructor-arg>
- setter()를 이용할 경우에는 <property>
- 두 개를 동시에 사용할 수 있다.

##### spring_6_1_ex1_springex
- applicationCTX1.xml
```
<bean id="family" class="com.javalec6_1.ex1.Family" c:papaName="정아빠" c:mamaName="정엄마" p:sisterName="정누나">
		<property name="brotherName" value="정형" />
	</bean>
```
1. 생성자 이용 -> constructor-arg
2. setter() 이용 -> property
3. 1,2도 귀찮다 하면 
  - c네임스페이스 : c:xxxxx -> c가 constructor-arg, 
  - p네임스페이스 : p:xxxxx -> p가 property
4. 대신 3처럼 쓸려면 아래 코드를 beans 코드 안에 추가 해줘야함
```
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	**xmlns:c="http://www.springframework.org/schema/c"**
	**xmlns:p="http://www.springframework.org/schema/p"**
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
```
- MainClass.java
```
AbstractApplicationContext ctx = new GenericXmlApplicationContext(configLocation1, configLocation2);
```
config 파일이 여러개면 계속 추가해서 다중으로 써주면 된다.
```
Student student1 = ctx.getBean("student1", Student.class);
		System.out.println(student1.getName()); //김지영
		System.out.println(student1.getHobbys()); //모바일 게임, 영화 보기
		
		StudentInfo studentInfo = ctx.getBean("studentInfo1", StudentInfo.class);
		Student student2 = studentInfo.getStudent(); //student1 == student2
		System.out.println(student2.getName()); // 김지영
		System.out.println(student2.getHobbys()); //모바일 게임, 영화 보기
```
`같은 객체를 다른 방법으로 가져올 수 있다.`

### 6-2. JAVA를 이용한 DI 설정 방법
- XML 쓰기 너무 싫어요! (추천X)
- 클래스 이름 앞에 @Configuration이라는 어노테이션을 꼭 붙여주어야 한다.
  - '이 클래스는 스프링 설정에 사용되는 클래스 입니다.' 라고 명시하는 어노테이션
- @Bean 어노테이션도 설정하고 값을 설정해주면 됨
##### spring_6_2_ex1_springex
```

import java.util.ArrayList;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class ApplicationConfig {
	
	@Bean
	public Student student1() {
		
		ArrayList<String> hobbys = new ArrayList<String> ();
		hobbys.add("수영");
		hobbys.add("요리");
		
		Student student = new Student("홍길동", 20, hobbys); //생성자 이용
		student.setHeight(180); //setter() 이용
		student.setWeight(80);
		
		return student;
	}
	
	@Bean
	public Student student2() {
		
		ArrayList<String> hobbys = new ArrayList<String> ();
		hobbys.add("독서");
		hobbys.add("음악감상");
		
		Student student = new Student("홍길순", 25, hobbys);
		student.setHeight(170);
		student.setWeight(65);
		
		return student;
	}
}
```
1. xml 파일을 java로 만든 것
2. @Configuration 이걸 설정했기 때문에 DI 역할을 할 것

### 6-3. XML과 JAVA를 같이 사용
1. XML 파일에 JAVA 파일을 포함시켜 사용하는 방법
##### spring_6_3_ex1_springex
```
<context:annotation-config />
	<bean class="com.javalec6_3.ex1.ApplicationConfig" />
```
xml에서 java 파일의 configuration 코드를 쓰겠다고 명시

2. JAVA 파일에 XML파일을 포함시켜 사용하는 방법
##### spring_6_3_ex2_springex
- applicationConfig.java
```

@Configuration
@ImportResource("classpath:applicationCTX.xml")
public class ApplicationConfig {
	
	@Bean
	public Student student1() {
		
		ArrayList<String> hobbys = new ArrayList<String> ();
		hobbys.add("수영");
		hobbys.add("요리");
		
		Student student = new Student("홍길동", 20, hobbys); //생성자 이용
		student.setHeight(180); //setter() 이용
		student.setWeight(80);
		
		return student;
	}
}
```
ImportResource 어노테이션을 통해 xml을 불러와서 쓴다.

> DI, IOC 컨테이너를 이용해서 객체를 뽑아올 때, 각 객체가 어떤 객체인지 잘 알아두어야한다!