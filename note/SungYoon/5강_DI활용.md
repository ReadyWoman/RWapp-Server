# 스프링 5강: DI 활용
- 의존관계
- DI 사용에 따른 장점

## 의존 관계
1. 생성자() applicationCTX.xml
2. setter() Java파일

예를 들어 StudentInfo 객체는 Student객체에 의존한다.

1. applicationCTX.xml
<bean id="studentInfo" class="com.javalec.ex.StudentInfo">
	<contructor-arg>
		<ref bean="student1" />
	</contructor-arg>
</bean>

2. Java 파일
Student student2 = ctx.getBean("student2", Student.class);
studentInfo.setStudent(student2);
studentInfo.getStudentInfo();

'''
setter()를 사용할 때는 property 키워드 사용
생성자를 사용할 때는 contructor-arg 키워드 사용
'''

## DI 사용 장점
- Java 파일 수정 없이 스프링 설정 파일만을 수정하여 부품들을 생성 및 조립할 수 있다.