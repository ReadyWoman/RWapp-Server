# 7강. 생명 주기(life cycle)와 범위(scope)
- 스프링 컨테이너 생명 주기
- 스프링 빈 생명 주기
- 스프링 빈 범위

### 7-1. 스프링 컨테이너 생명 주기
(spring_7_1_ex1_springex)

생명 주기 | 코드
--- | ---
스프링 컨테이너 **생성** | AbstractApplicationContext ctx = new GenericXmlApplicationContext("");
↓ | ↓
스프링 컨테이너 **설정** | ctx.load("classpath:applicationCTX.xml");
 | ctx.refresh(); //load를 하면 꼭 refresh해줘야함
↓ | ↓
스프링 컨테이너 **사용** | Student student = ctx.getBean("student", Student.class);
 | System.out.println("이름 : " + student.getName());
 | System.out.println("나이 : " + student.getAge());
↓ | ↓
스프링 컨테이너 **종료** | ctx.close(); //자원 해제

### 7-2. 스프링 빈 생명 주기
(spring_7_2_ex1_springex)
1) 인터페이스인 경우 : implements **InitializingBean, DisposableBean**
- InitializingBean을 만들면 afterPropertiesSet()이라는 클래스를 반드시 오버라이드하게 되어있음
  - 빈 초기화 과정에서 호출된다 -> ctx.refresh(); 에서 호출
- DisposableBean을 만들면 destroy()라는 클래스를 오버라이드 해야한다.
  - 빈 소멸 과정에서 생성 -> ctx.close(); 에서, 자원 해제 할 때
  - ctx.close();는 컨테이너가 소멸하는 단계인데, 컨테이너가 소멸하면 빈도 자동 소멸된다. 그러나 빈만 없애고 싶다면 student.destroy() API를 이용하면 된다.
```
import org.springframework.beans.factory.DisposableBean;
import org.springframework.beans.factory.InitializingBean;

public class Student implements InitializingBean, DisposableBean{

	private String name;
	private int age;
	
	public Student(String name, int age) {
		this.name = name;
		this.age = age;
	}

	public String getName() {
		return name;
	}

	public int getAge() {
		return age;
	}
	// 오버라이드 해야하는 것
	**@Override 
	public void destroy() throws Exception {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void afterPropertiesSet() throws Exception {
		// TODO Auto-generated method stub
		
	}**	
}
```
빈 생성과정, 소멸과정에서 뭔가 하고 싶을 때 사용할 수 있다.
```

import javax.annotation.*;

public class OtherStudent {

	private String name;
	private int age;
	
	public OtherStudent(String name, int age) {
		this.name = name;
		this.age = age;
	}

	public String getName() {
		return name;
	}

	public int getAge() {
		return age;
	}
	
	@PostConstruct
	public void initMethod() {
		System.out.println("initMethod()");
	}
	
	@PreDestroy
	public void destroyMethod() {
		System.out.println("destroyMethod()");
	}
}
```

2) 그냥 클래스인 경우 : @PostConstruct, @PreDestroy 어노테이션 사용 방법
- 내가 어떤 메소드를 그냥 만들고, 이 메소드가 빈이 생성되는 때에 어떤 일을 했으면 좋겠다. -> 이때 @PostConstruct 를 써준다.
```
@PostConstruct
public void initMethod() {
	System.out.println("initMethod()");
}
```
- 소멸될 때는 @PreDestroy 추가
```
@PreDestroy
public void destroyMethod() {
	System.out.println("destroyMethod()");
}
```
이렇게 설정하고 xml에서 <context:annotation-config /> 이거를 추가했지만 에러가 뜨는데, beans 안에 아래 코드를 추가해주면 된다.
```
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 		
		http://www.springframework.org/schema/beans/spring-beans.xsd 
		**http://www.springframework.org/schema/context
		http://www.springframework.org/schema/context/spring-context-3.0.xsd**">
...
</beans>
```
xmlns:context만 추가했더니 안되고 xsi:schemaLocation 안에도 context 링크를 썼더니 됐다.

### 7-3. 스프링 빈 범위(scope)
- 스프링 컨테이너가 생성되고, 스프링 빈이 생성될 때, 생성된 스프링 빈은 scope를 가지고 있다.
- scope이란 해당 객체가 어디까지 영향을 미치는지 결정하는 것
- 디폴트가 singleton

#### spring_7_3_ex1_springex
- MainClass.java
```
import org.springframework.context.support.AbstractApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;

public class MainClass {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		
		AbstractApplicationContext ctx = new GenericXmlApplicationContext("classpath:applicationCTX.xml");
		Student student1 = ctx.getBean("student", Student.class);
		System.out.println("이름 : " + student1.getName());
		System.out.println("나이 : " + student1.getAge());
		System.out.println("=============================");
		
		Student student2 = ctx.getBean("student", Student.class);
		student2.setName("홍길자");
		student2.setAge(100);
		
		System.out.println("이름 : " + student1.getName());
		System.out.println("나이 : " + student1.getAge());
		System.out.println("=============================");
		
		if(student1.equals(student2)) {
			System.out.println("student1 == student2");
		} else {
			System.out.println("student1 != student2");
		}
	}

}
```
student1과 student2는 같은 객체. setter()를 통해 (속성)값을 바꿔준 것.
내가 생성한 객체가 아니고, 스프링 컨테이너가 생성한 객체