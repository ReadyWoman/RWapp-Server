# ������ 5��: DI Ȱ��
- ��������
- DI ��뿡 ���� ����

## ���� ����
1. ������() applicationCTX.xml
2. setter() Java����

���� ��� StudentInfo ��ü�� Student��ü�� �����Ѵ�.

1. applicationCTX.xml
<bean id="studentInfo" class="com.javalec.ex.StudentInfo">
	<contructor-arg>
		<ref bean="student1" />
	</contructor-arg>
</bean>

2. Java ����
Student student2 = ctx.getBean("student2", Student.class);
studentInfo.setStudent(student2);
studentInfo.getStudentInfo();

'''
setter()�� ����� ���� property Ű���� ���
�����ڸ� ����� ���� contructor-arg Ű���� ���
'''

## DI ��� ����
- Java ���� ���� ���� ������ ���� ���ϸ��� �����Ͽ� ��ǰ���� ���� �� ������ �� �ִ�.