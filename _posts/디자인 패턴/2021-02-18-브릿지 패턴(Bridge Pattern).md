---
title:  "브릿지 패턴(Bridge Pattern)"
excerpt: "브릿지 패턴(Bridge Pattern)을 알아보자!"

categories:
  - 디자인 패턴
  
last_modified_at: 2021-02-18T18:35:00
---

브릿지 패턴(Bridge Pattern)은 **구조(Structural) 패턴 중 하나로써 구현부에서 추상층을 분리하여 각자 독립적으로 변형할 수 있게 하는 패턴**이다.  

간단하게 얘기하면 다양한 기능과 구현을 가질 수 있는 주제?일 경우 기능 부분과 구현 부분을 분리하여 각각 추상화를 통해 확장 및 유지 보수가 용이하도록 설계하는 디자인 패턴이다.  

브릿지 패턴의 구조는 다음과 같다.  

![브릿지1](https://user-images.githubusercontent.com/53072057/108298318-17275a00-71e0-11eb-9876-a5d5c869969b.JPG)  

* Abstraction : 기능 부분의 추상 인터페이스, 구현 부분의 클래스를 통해 구현 부분의 메서드를 호출  
* RefinedAbstraction : Abstraction 인터페이스의 기능을 확장한 클래스  
* Implementor : 구현 부분의 추상 인터페이스, Abstracion의 기능을 구현하기 위한 기능을 정의  
* ConcreteImplementor : Implementor 인터페이스의 기능을 구현한 클래스  

그렇다면 간단한 예시를 통해 브릿지 패턴에 대해서 알아보도록 하자. 마땅한 예시가 잘 안 떠올라서 약간 억지스러운 부분이 있을 수도 있지만 가볍게 이해해 보는 것이기 때문에 너그럽게 봐주면 감사하겠다.  

격식을 차려야 하는 극장을 예시로 들어보자. 극장에는 머리 스타일과 복장 입장 규칙이 존재한다고 하자. 따라서 극장은 남자와 여자를 각각의 규칙에 적합한지 확인하는 상황이라고 하자. 이를 브릿지 패턴을 활용하여 구현해보자.  

먼저 기능 부분과 구현 부분을 나눠야 한다. 기능 부분은 규칙에 적합한지를 확인​하는 것이고 구현 부분은 남자와 여자의 머리 스타일과 복잡 입장 규칙을 정의하는 부분일 것이다. 하나씩 살펴보도록 하자.  

먼저 구현 부분의 **Implementor**인 AdmissionRules 인터페이스이다. 간단하게 머리 스타일과 드레스코드를 검사하는 메서드를 가지도록 한다.  

```java
public interface AdmissionRules {
	public void hairRule();
	public void dressRule();
}
```

다음으로 **Concretelmplemenotr**이다. 머리 스타일과 드레스코드를 검사하는 메서드를 남자와 여자에 맞게 구현해준다.  

```java
public class ManRules implements AdmissionRules {
	@Override
	public void hairRule() {
		System.out.println("이마가 보이도록 머리를 뒤로 넘겨주세요");
	}

	@Override
	public void dressRule() {
		System.out.println("흑색 계열의 정장을 입어주세요");
	}
}

public class WomanRules implements AdmissionRules {
	@Override
	public void hairRule() {
		System.out.println("머리를 단정하게 뒤로 묶어주세요.");
	}

	@Override
	public void dressRule() {
		System.out.println("백색 계열의 드레스를 입어주세요.");
	}
}
```

다음으로 기능 부분의 **Abstracion**인 Theater 추상 클래스이다. 기능 부분이기 때문에 직접 확인하는 메서드를 구현하지 않고 구현 부분의 **AdmissionRules 인터페이스에게 위임**하여 구현하도록 한다.  

```java
public abstract class Theater {
	//구현 부분의 AdmissionRules 객체를 통해 규칙을 확인
	private AdmissionRules admissionRules;
	
	//생성자 주입을 통해 권한을 위임
	public Theater(AdmissionRules admissionRules) {
		this.admissionRules = admissionRules;
	}

	public abstract Theater ruleForWho();
	
    //머리 스타일과 복장 규칙을 확인하는 메서드는 AdmissionRules를 통해 실행
	public Theater hairRule() {
		admissionRules.hairRule();
		return this;
	}

	public Theater dressRule() {
		admissionRules.dressRule();;
		return this;
	}
}
```

마지막으로 **RefinedAbstraction**이다. 추상 클래스는 객체를 생성할 수 없기 때문에 ruleForWho() 메서드를 구현해주는 남자와 여자 클래스를 구현해준다.  

```java
public class Man extends Theater {
	public Man(AdmissionRules admissionRules) {
		super(admissionRules);
	}

	@Override
	public Theater ruleForWho() {
		System.out.println("Rules for Man");
		return this;
	}
}

public class Woman extends Theater{
	public Woman(AdmissionRules admissionRules) {
		super(admissionRules);
	}

	@Override
	public Theater ruleForWho() {
		System.out.println("Rules for Woman");
		return this;
	}
}
```

**Main** 클래스는 다음과 같다.  

```java
public class Main {

	public static void main(String[] args) {
		Theater man = new Man(new ManRules());
		man.ruleForWho().hairRule().dressRule();
		System.out.println("=========================");
		Theater woman = new Woman(new WomanRules());		
		woman.ruleForWho().hairRule().dressRule();
	}
}
```

![브릿지2](https://user-images.githubusercontent.com/53072057/108298319-18588700-71e0-11eb-9be3-b36d330c794d.JPG)  

기능 부분과 구현 부분을 분리하여 각각 추상화한 후 하위 클래스에게 구현하도록 하여 Main 클래스의 코드가 간단해진 것을 확인할 수 있다.  

또한, 다른 기능을 추가한다 하여도 Main 클래스에는 별도의 수정이 필요 없기 때문에 확장 및 유지 보수가 쉬우며 실제 구현 코드를 내부로 숨겼기 때문에 사용자 입장에서 훨씬 편해졌음을 확인할 수 있다.  
