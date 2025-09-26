# 1. 아직 모르는 부분이 많다



# 2. SQL 테이블 연결 첫 부분
```
public class Emp {
	
	private int empno;
	
	private ...

	//===================================================================
	
	public int getEmpno() {
		return empno;
	}

	public void setEmpno(int empno) {
		if(empno<0||empno>9999){ this.empno=0; return; }
		this.empno = empno;
	}
...get set코드...
```
🤷‍♀️여기까지 했는데, 이 테이블은 null값이 있는것에 대해서 어떻게 해줘야 하지?

✨ Wrapper 클래스 사용 (null 허용) ← 더 정확한 표현

int	Integer, double	Double, boolean	Boolean

🤷‍♀️wrapper를 모르겠는데 일단 궁금한 건 자바에서 null을 뭐라고 확인하지?

✨자바에서 null을 확인하려면?	if (변수 == null)

기본형도 null이 되나?	❌ 안 됨 (int, double 등은 null 불가)

null을 쓰려면?	래퍼 클래스(Integer, Double)를 써야 함

🤷‍♀️그럼 if(mgr<0||mgr>9999 || mgr==null)이라고 쓰면 오류가 나나?

✨mgr이 null인 상태에서 mgr < 0을 먼저 실행하려다 NullPointerException 또는 컴파일 오류가 나요.

✅ 해결 방법: null 체크를 먼저 하자

🤷‍♀️앞에 썼다고 해도 int면 에러가 나나?

✨네, int 타입이면 mgr == null 자체가 컴파일 에러입니다.

✔ null 처리를 하고 싶다면 Integer를 써야 함

# 2-1. BigDecimal의 존재

NUMBER(7,2)  라는게 마음에 걸리는데, 그럼 7000000도 되는 값인가?

총 7자리 중에서 소수점 아래 2자리를 사용한다는 뜻입니다. 따라서 ❌

그럼 sal comm은 무슨 변수로 해야 적절하지

👉 가장 적절한 건 BigDecimal

🔢 소수점 정밀도 유지 double이나 float은 오차 발생 위험이 있음

원래는 double이랑 Double인건가?

네, 정확히 짚으셨어요! 👏

책엔 bigdecimal이 없던데 뭐지

BigDecimal은 객체, 정밀 수학 계산, 클래스 내부 메서드 사용, 성능 고려 등 심화 개념이 들어가기 때문에 중급~실무 레벨에서 주로 다뤄요.

# 2-2. 실제로 BigDecimal로 고치는 작업을 해봤는데 시간이 많이 걸렸다
