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

```

package chapter9;

import java.math.BigDecimal;
import java.time.LocalDate;

public class Emp {
	
	private int empno;
	
	private String ename;
	
	private String job;

	private Integer mgr;
	
	private LocalDate hireDate;
	
	private BigDecimal sal;
	
	private BigDecimal comm;
	
	private int deptno;

	//===================================================================
	
	public int getEmpno() {
		return empno;
	}

	public void setEmpno(int empno) {
		if(empno<0||empno>9999){ this.empno=0; return; }
		this.empno = empno;
	}

	public String getEname() {
		return ename;
	}

	public void setEname(String ename) {
		if(ename.length()>10) { System.out.println("11글자 미만 입력"); this.ename=""; }
		this.ename = ename;
	}

	public String getJob() {
		return job;
	}

	public void setJob(String job) {
		if(job.length()>9) { System.out.println("10글자 미만 입력"); this.job=""; }
		this.job = job;
	}

	public Integer getMgr() {
		return mgr;
	}
//매니저 없을 수도 있는
	public void setMgr(Integer mgr) {
		if(mgr == null || mgr<0 || mgr>9999)
		{
			this.mgr=null; return;
		}
		this.mgr = mgr;
	}

	public LocalDate getHireDate() {
		return hireDate;
	}

	public void setHireDate(LocalDate hireDate) {
		this.hireDate = hireDate;
	}

	public BigDecimal getSal() {
		return sal;
	}
// 5.2자리
	public void setSal(BigDecimal sal) {
		if(sal.compareTo(BigDecimal.ZERO) < 0 ||
			sal.compareTo(new BigDecimal("9999.99")) > 0)
		{
			this.sal=BigDecimal.ZERO; return;
		}
		
		this.sal = sal;
	}

	public BigDecimal getComm() {
		return comm;
	}
	
//커미션 없을 수도 있는 5.2자리
	public void setComm(BigDecimal comm) {
		if(comm==null ||
		comm.compareTo(BigDecimal.ZERO) < 0 ||
		comm.compareTo(new BigDecimal("9999.99")) > 0)
		{ 
			this.comm=BigDecimal.ZERO; return;
		}
		
		this.comm = comm;
	}

	public int getDeptno() {
		return deptno;
	}

	public void setDeptno(int deptno) {
		if(deptno<0||deptno>99){ this.deptno=0; return; }
		this.deptno = deptno;
	}
	
}
```
```
몇 가지 제안만 드리자면:

sal과 comm에 기본값 넣는 생성자 추가하면 객체 생성 후 바로 쓸 때 편해요

혹시 sal이 null이면 안 된다고 해도, setter에 null 체크 한 번 넣는 게 안전할 수 있어요. (예외 던지거나 0으로 초기화 등)

출력용 toString() 메서드 만들어 두면 디버깅할 때 좋습니다
```
