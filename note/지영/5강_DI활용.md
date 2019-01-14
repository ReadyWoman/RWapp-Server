# 5강. DI활용
- 의존 관계
- DI 사용에 따른 장점

### 5-1. 의존 관계
Ex) StudentInfo 객체는 Student 객체에 의존한다 
  - setter() 방법 
    - xml 을 사용하는 방법
    - java 파일을 사용하는 방법
  - 생성자 방법 : 앞으로 배울 것
    - spring_5_1_ex1_springex
    - 필드에서는 선언만 해주고, 생성자에서 값을 받아서 필드 값을 초기화해준다.
    - 이 때 설정 파일에서는 **property 태그 대신 constructor-arg**라는 태그를 써준다.
    - 자바 코드를 추가로 주입할 수도 있다 (setter에서도 되겠지?ㅇㅅㅇ)

### 5-2. DI 사용에 따른 장점
 java의 interface 처럼 작은 프로젝트에서은 DI의 장점을 느끼기 힘들 수 있다, 하지만 **규모가 큰 프로젝트나 유지보수가 발생하는 업무일 때 개발 장점을 느낄 수 있다.**

#### 장점
- 사용자의 요구에 따라, main의 **java 코드는 바뀌지 않고**, xml(스프링 설정 파일)에서 클래스만 바꿔주면 다양한 설정이 가능하다.
- spring_5_2_ex1_springex 에서 연습
```
AbstractApplicationContext ctx = new GenericXmlApplicationContext("classpath:applicationCTX.xml");
```
 xml 파일을 가져와서 ctx에 넣어준다.
```
Pencil pencil = ctx.getBean("pencil", Pencil.class);
```
 ctx로부터 "pencil"을 가져온다. -> xml에 pencil이라는 id를 가진 bean이 있나보다, 그리고 그 객체의 타입은 Pencil.class 이구나
```
pencil.use();
```
  그리고 그 객체에는 use()라는 method가 있구나.
```
public interface Pencil {
	public void use();
}
```
Pencil은 타입만 제공, 기능이 없다.
그리고 강제로 오버라이드 하라고 메소드만 가지고 있다.
```
<bean id="pencil" class="com.javalec5_2_ex1.Pencil4B" />
```
interface를 쓰면 config 파일만 수정해서 사용할 수 있다.