# 3강. DI(Dependency Injection)-I

- 스프링을 이용한 객체 생성과 조립
- 스프링 설정 파일의 이해

### 지난 시간 복습

A, B, C라는 객체가 있을 때
 - A가 직접 new B()를 해서 B, C 객체를 사용하는 방법과
 - 외부의 어떤 객체가 만든 B, C 객체를 setter 또는 constructor를 통해 A에서 사용하는 방법이 있음
> 스프링은 후자의 방법을 사용한다.

### 3-1. 스프링을 이용한 객체 생성과 조립

- **스프링은 객체를 생성하고 조립한다.**
- 생성 시 xml 파일 이용

##### <spring project 비교> 
- spring 기법을 사용하지 않은 플젝 > spring_3_1_ex1_springex
- spring 기법을 사용한 플젝 > spring_3_1_ex2_springex

#### spring_3_1_ex2_springex의 applicationCTX.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/bean/spring-beans.xsd">

	<bean id="calculator" class="com.javalec.ex.Calculator" />
	
	<bean id="myCalculator" class="com.javalec.ex.MyCalcultor">
		<property name="calculator">
			<ref bean="calculator" />
		</property>
		<property name="firstNum" value="10"></property>
		<property name="secondNum" value="2"></property>
	</bean>

</beans>
```

- java 'bean' : 객체 하나하나를 bean이라고 생각하면 된다.
  - id는 명찰 같은 고유한 값. ex) calculator라는 bean이라는 객체고 실체는 com.javalec.ex.Calculator이다.
  - 코드에서 new Calculator로 직접 생성하는 것이 아니고 xml 코드로 bean 태그를 이용해서 객체를 생성
  - 또 myCalculator라는 bean을 만든다.
- property : 필드를 설정해준다.
  - ref : 참조하고 있는 객체 설정, calculator라는 객체를 참조한다.
  - 기본 자료형, value 값을 넣어줄 수 있다.

```
package com.javalec.ex;

import org.springframework.context.support.AbstractApplicationContext;
import org.springframework.context.support.GenericXmlApplicationContext;

public class MainClass {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		/*
		스프링 특징이 적용되지 않은 코드
		MyCalculator myCalculator = new MyCalculator();
		myCalculator.setCalculator(new Calculator());
		
		myCalculator.setFirstNum(10);
		myCalculator.setSecondNum(2);
		
		myCalculator.add();
		myCalculator.sub();
		myCalculator.mul();
		myCalculator.div();
		*/
		
		//변하는 건 이 부분, 나머지 코드는 다 똑같다.
		String configLocation = "classpath:applicationCTX.xml";
		AbstractApplicationContext ctx = new GenericXmlApplicationContext(configLocation);
		MyCalculator myCalculator = **ctx.getBean("myCalculator", MyCalculator.class);**
		//MyCalculator 클래스를 외부에서 불러옴
		
		myCalculator.add();
		myCalculator.sub();
		myCalculator.mul();
		myCalculator.div();
	}

}
```
> String configLocation = "classpath:applicationCTX.xml";
configLocation에 xml의 classpath를 넣어주고, 
> AbstractApplicationContext ctx = new GenericXmlApplicationContext(configLocation); 
에서 파싱한다. 이 프로젝트의 context를 구한다.
> MyCalculator myCalculator = ctx.getBean("myCalculator", MyCalculator.class); 
getBean을 통해 Bean을 얻어온다, 첫번째 값은 bean의 id, 두번째 값은 타입, 클래스 이름

### 3-2. 스프링 설정 파일의 이해