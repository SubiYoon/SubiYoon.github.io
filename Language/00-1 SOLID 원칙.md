
# SOLID원칙이란?
- 로버트 마틴이 정리한 객체 지향 설게의 5가지 원칙을 의미

## S(SRP; Single Responssibility Principle)
- 단일 책임의 원칙
- 하나의 객체는 하나의 책임만 갖어야 한다.
- 응집도 : 모듈 내보의 기능적인 응집도
	- ex) 카메라라는 기능 안에 카메라의 모든 기능이 모여있어야(응집되어야) 한다.
- 결합도 : 모듈 간의 상호 결합 정도
	- 하나의 객체는 하나의 기능만 하도록 설계하여 다른 기능들과의 상호관계가 없이 만들어야 한다.
	- ex) 시스템에 변화가 생겼을시 결합도가 높으면 다른 기능을 수정 할 때 고려해야 할 사항이 많아진다.
- 응집도는 높게, 결합도는 낮게 설계해야 한다.


## O(OCP; Open Closed Principle)
- 개방 폐쇄 원칙
- 확장에 대해서는 개방적, 변경에 대한것은 폐쇄적이어야 한다는 원칙
- 변경되어야하는 것과 변경되면 안되는 것들을 확실히 구분지어 개발해야한다

~~~java
public class Trainer {
	public Trainer(Animal animal){
		if (animal == null){
			throw new AnimalException("where is animal?");
		}

		if(animal instanceof Cat cat){
			cat.cry();
		} else if (animal instanceof Dog dog){
			dog.cry();
		}
		//만약 동물이 한마리 더 추가 된다면... 조건무을 더 증가시켜야 한다.
		else if (animal instanceof Bird bird){
			bird.cry();
		}
	}
}
~~~
- 위와 같은 코드보단....
- 어차피 각 동물에  **cry()** 함수를 재정의 했으면 다음과 같이 사용하는게 더 개발 폐쇄원칙 적절하다.
~~~java
public class Trainer {
	public Trainer(Animal animal){
		if (animal == null){
			throw new AnimalException("where is animal?");
		}

		animal.cry();
	}
}
~~~


## L(LSP; Liscov Substitution Principle)
- 리스코프 치환 원칙
- 서브 타입(상속을 받는 객체)는 언제나 기반 타입(상속 객체)으로 교체할 수 있어야한다.
- 다형성을 극대화 하기 위한 원칙
- 만약 두 객체가 똑같은 일을 한다면, 둘을 하나의 클래스로 표현하고 이들을 구분할 수 있는 필드를 둔다.
- 똑같은 연산을 제공하지만, 이들을 약간씩 다르게 구현한다면 공통의 인터페이스를 만들고 둘이 이를 구현한다.
- 공통된 연산이 없다면 완전 별개인 2개의 클래스를 만든다.
- 만약 두 객체가 하는 일에 추가적으로 무언가를 더 한다면 구현 상속을 사용한다.

~~~java
void code(){
	LinkedList list = new LinkedList();

	update(list);
}

void update(LinkedList list){
	list.add(new Object());
}

void newCode(){
	HashMap map = new HashMap();

	newUpdate(map);
}

void newUpdate(HashMap map){
	map.add(new Object());
}
~~~
- 위와 같은 코드보다는
~~~java
void code(){
	Collection list = new LinkedList();

	update(list);
}

void update(Collection list){
	list.add(new Object());
}

//만약 LinkedList가 아닌 HashMap으로 사용하고 싶을 경우에는 뒤에 new ~~~ 부분만 수정해준면 된다.
void newcode(){
	Collection list = new HashMap();

	update(list);
}
~~~

## I(ISP; Inerface Segregation Principle)
- 인터페이스 분리 원칙
- 한 클래스는 자신이 사용하지 않는 인터페이스는 구현하지 말아야 한다는 원리
- 하나의 일반적인 인터페이스를 여럿이 구현하는 것보다 구체적인 인터페이스가 낫다
	- 해당 인터페이스에서 사용하지 않는 메서드들 까지 Override해야 하기 때문에 비효윯적


## D(DIP; Dependency Inversion Principle)
- 의존 역전 원칙
- 일반적으로 인터페이스를 활용하여 이 원칙을 지킨다.(레이어링, Layering)
- ex) 많은 동물들이 있겠지만, 최상위 녀석하나를 사용해 의존이 역전되지 않도록 만든다.
~~~java
public class Trainer {

	private Animal animal;
	
	public Trainer(Animal animal){
		if (animal == null){
			throw new AnimalException("where is animal?");
		}
		this.animal = animal;
	}

	public void training(){
		animal.cry();
	}
~~~