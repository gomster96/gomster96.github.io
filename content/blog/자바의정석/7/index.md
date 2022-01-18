---
title: '[자바의 정석] 7장 객체지향 프로그래밍 2'
date: 2021-12-23 16:39:13
category: '자바의 정석'
draft: false
---

# 1. 클래스 간의 관계

## 1. 상속

상속이란 기존의 클래스를 재사용하여 새로운 클래스를 작성하는 것이다.

- 적은 양의 코드로 새로운 클래스를 작성할 수 있다.
- 코드를 공통으로 관리 할 수 있다.
- 상속받고자하는 클래스 이름을 extends 와 함께 사용하면 상속이 가능하다.
- 생성자와 초기화 블럭은 상속되지 않는다. 멤버만 상속된다.
- 자손클래스의 멤버 개수는 부모 클래스보다 항상 같거나 많다.

→ 상속받은 자손 클래스는 부모 클래스의 모든 멤버를 상속받기때문에 Child클래스는 Parent 클래스의 멤버들을 모두 포함 할 수 있다.

### 단일 상속(single inheritance)

자바에서는 오직 하나의 클래스만을 상속받을 수 있는 단일 상속만을 허용한다.

→ 클래스간의 관계가 보다 명확해지고 코드를 더욱 신뢰할 수 있게 만들어 준다는 장점이 있다.

### Object 클래스 - 모든 클래스의 부모

Object클래스는 모든 클래스의 상속계층도의 최상위에 있는 부모클래스이다.

→ 결국 마지막 최상위 조상은 Object 클래스이다.

→ toString(), equals와 같은 메서드를 정의하지 않고 사용할 수 있었던 이유는 이 메서드들이 모두 Object클래스에 정의된 것들이기 때문이다.

<p align="center"><img src="./1.png" height="100px" width="500px"></p>

## 2. 포함

클래스 내부에서 새로운 클래스를 다시 만드는 것을 **포함 관계**를 맺는다고 한다.

```java
class Circle {
	Point c = new Point();
	int r;
}
```

## 클래스의 관계 결정하기

is - a 관계 : **상속**

- ~은 ~이다
- ex) 원(Circle)은 점(Point)이다

has - a 관계 : **포함**

- ~은 ~을 가지고 있다.
- ex) 원(Circle)은 점(Point)를 가지고 있다.

# 2. 오버라이딩(overriding)

부모클래스로부터 **상속받은 메서드**의 내용을 **변경하는 것**을 오버라이딩이라고 한다.

- 상속받은 메서드를 그대로 사용할 수 도 있지만, 자손 클래스 자신에 맞게 변경해야 할 경우 사용한다.

### 조건

오버라이딩은 메서드의 내용만을 작성하는 것이므로 메서드의 선언부는 조상의 것과 완전히 일치해야 한다.

- 이름이 같아야한다
- 매개변수가 같아야한다
- 반환타입이 같아야한다.
- 접근 제어자를 조상 클래스의 메서드보다 좁은 범위로 변경할 수 없다
- 예외는 조상 클래스의 메서드보다 많이 선언할 수 없다.
- 인스턴스 메서드를 static메서드로 또는 그 반대로 변경 할 수 없다.

### super

상속받은 멤버와 자신의 멤버와 이름이 같을 때는 super와 this를 사용하여 구별이 가능하다.

```java
super.변수 (부모의 변수)
this.변수 (자식 즉 자신의 변수)
```

변수 뿐만이 아니라 메서드 역시 마찬가지이다.

→ 조상 클래스의 메서드를 자손클래스에서 오버라이딩 한 경우 super를 사용하여 오버라이딩 전의 조상 클래스의 메서드를 호출할 수 있다.

### super() - 조상클래스의 생성자

super()는 조상 클래스의 생성자를 호출할 때 사용된다.

자식 클래스가 인스턴스를 생성하면, 조상의 멤버와 부모의 멤버가 모두 합쳐진 하나의 인스턴스가 생성된다

→ **이를 위해서 부모의 생성자가 필요하다**, 만약 없을경우 컴파일러는 생성자의 첫줄에 **자동적으로**

**super();** 를 추가해준다.

→ **조상클래스의 멤버변수는 조상의 생성자**에 의해 초기화 되도록 해야한다.

# 3. Package와 import

패키지란 **클래스의 묶음**이다.

클래스가 물리적으로 하나의 클래스파일(.class)인 것처럼 패키지는 물리적으로 **하나의 디렉토리**이다.

### 선언

클래스나 인터페이스의 소스파일(.java)의 **맨 위에** 다음과 같이 적어주면 된다.

```java
package 패키지명;
```

- 클래스명과 구분하기 위해서 소문자로 하는것을 원칙으로한다
- 모든 클래스는 반드시 하나의 패키지에 포함되어야한다
- 맨위에 package가 적혀있지 않을 경우 기본적으로 제공하는 **이름없는 패키지**(unnamed package)에 포함된다.

### import문

import 문으로 사용하고자 하는 클래스의 패키지를 미리 명시해주면 소스코드에 사용되는 클래스이름에서 패키지명은 생략 할 수 있다.

```java
import java.util.*;
import java.util.ArrayList;
// 이렇게 적을시 util패키지에서 쓸 수 있는 모든것은 패키지명을 생략해도 된다.
// 만약 import 를 하지 않았을 경우 java.util.ArrayList 이런식으로 패키지 명도 항상 추가해야한다.
```

### static import

static import 문을 사용하면 가지고올 클래스의 이름까지 생략할 수 있다.

```java
import static java.lang.System.out;
import static java.lang.Math.*;

class StaticImportEx1 {
	public static void main(String[] args){
		//System.out.println(Math.random()); 해당 라인을 아래처럼 바꿀 수 있다.
		out.println(random());
	}
}
```

# 4. 제어자 (modifier)

**접근제어자**

- public
- protected
- default
- private

**그 외**

- static
- final
- abstract
- 등

### static - 클래스의, 공통적인

static은 **클래스에 관계** 된 것이기 때문에 **인스턴스를 생성하지 않고도** 사용할 수 있다.

- static은 멤버변수, 메서드, 초기화 블럭에서 사용가능하다.

### final - 마지막의, 변경될 수 없는

변수에 사용되면 **값을 변경할 수 없는** 상수가 된다.

메서드에 사용되면 오버라이딩을 할 수 없게 된다.

클래스에 사용되면 자신을 확장하는 자손클래스를 정의하지 못하게 된다.

ex) 대표적인 final class로 String과 Math가 있다.

### abstract - 추상의, 미완성의

abstract는 메서드의 선언부만 작성하고 실제 수행내용은 구현하지 않은 추상 메서드를 선언하는데 사용된다.

abstract클래스는 자체로는 쓸모가 없지만, 다른 클래스가 **이 클래스를 상속받아서 일부의 원하는 메서드만 오버라이딩 해도 된다는 장점**이 있다. (interface와의 차이)

### 접근제어자 (private, default, protected, public)

접근제어자는 외부로부터 데이터를 보호하기 위해, 외부에는 불필요한, 내부적으로만 사용되는 부분을 감추기 위해서 사용된다.

private

- 같은 클래스 내에서만 접근이 가능하다

default

- 같은 패키지 내에서만 접근이 가능하다

protected

- 같은 패키지 내에서, 그리고 다른 패키지의 자손클래스에서 접근이 가능하다..

public

- 접근 제한이 전혀 없다

```java
public > protected > default > private
```

접근 제어자를 통해 객체지향의 개념인 캡슐화(encapsulation)을 할 수 있다.

- 멤버변수의 값을 읽는 메서드의 이름을 ‘get멤버변수이름’ (getter라고 부름)
- 멤버변수의 값을 변경하는 메서드의 이름을 ‘set멤버변수이름’ (setter라고 부름)

### 싱글톤 만들기

```java
class Singleton{
	private static Singleton s = new Singleton();
	// instance 가 미리 생성되어야 하므로 static으로 선언해야한다.
	private Singleton(){
		// 생성자 내용
	}
	public static Singleton getInstance(){
		return s;
	}
}
```

# 5. 다형성

## 다형성이란?

다형성이란 여러가지 형태를 가질 수 있는 능력을 의미한다.

- 조상클래스 타입의 참조변수로 자손 클래스의 인스턴스를 참조 할 수 있다.
- 참조변수가 사용할 수 있는 멤버의 개수는 인스턴스의 멤버의 개수보다 같거나 적어야한다.
- 자손타입의 참조변수로 조상타입의 인스턴스를 참조할 수 없다

```java
class Tv{
	boolean power;
	int channel;

	void power() { power = !power; }
	void channelUp() { ++channel;}
	void channelDown() {--channel; }
}

class CaptionTv extends Tv {
	String text;
	void caption() { /*내용 생략*/ }
}

```

위에 예제에서 다형성에 의해 다음과 같은 선언이 가능하다.

```java

CaptionTv c = new CaptionTv();
Tv t = new CaptionTv();

```

단 둘다 같은 타입의 인스턴스지만 참조변수의 타입에 따라 사용할 수 있는 멤버의 개수가 달라진다

- Tv는 CaptionTv의 조상클래스이므로 Tv는 text라는 멤버변수를 사용할 수 없다.

## 참조변수의 형변환

기본형 변수처럼 참조변수도 형변환이 가능하다.

- 서로 상속관계에 있는 클래스 사이에서만 가능하다. 즉 자손타입의 참조변수를 조상타입으로, 조상타입의 참조변수를 자손타입의 참조변수로의 형변환만 가능하다.
- 참조변수의 형변환도 캐스트연산자를 사용한다.
  - 다음과 같이 사용한다 : (변환하고자하는 클래스명)변환하고자하는참조변수
    > 자손타입 -> 조상타입(Up-casting) : 형변환 생략가능 <br>
    > 자손타입 <- 조상타입(Down-casting) : 형변환 생략불가

형변환은 참조변수의 타입을 변환하는 것이지 **인스턴스를 변환하는 것이 아니기 때문에** 참조변수의 형변화는 **인스턴스의 아무런 영향을 미치지 않는다**. <br>

참조변수의 형변환을 통해서 참조하고 있는 인스턴스에서 **사용할 수 있는 멤버의 범위(개수)를 조절**하는 것이 가능하다.

참조변수가 가리키는 **인스턴스의 자손타입으로 형변환은 허용되지 않는다.(더 넓은 범위를 갖는 것이기 때문)** 따라서 참조변수가 **가르키는 인스턴스의 타입**이 무엇인지 확인하는 것이 중요하다.

## instanceof 연산자

- **인스턴스의 실제 타입**을 알아보기 위해 사용하는 연산자이다.
- intanceof를 이용한 연산결과로 true를 얻었다는 것은 참조변수가 검사한 타입으로 **형변환이 가능**하다는 것을 의미한다.

```java
class InstanceofTest{
	public static void main(String args[]){
		FireEngine fe = new FireEngine();

		if(fe instanceof FireEngine){
			System.out.println("This is a FireEngine Instance.");
		}
		if(fe instanceof Car){
			System.out.println("This is a Car Instance.");
		}
		if(fe instanceof Object){
			System.out.println("This is an Object Instance");
		}
	}
}
class Car{}
class FireEngine extends Car{}

/*
결과는 다음과 같다
This is a FireEngine Instance
This is a Car Instance
This is a Object Instance
*/
```

- 생성된 인스턴스는 FireEngine의 인스턴스이며 Car의 인스턴스이고, Object의 인스턴스이다.
- Object클래스는 모든 클래스의 조상클래스이다.

## 매개변수의 다형성

참조변수의 다형적인 특징에 의해 매개변수도 마찬가지로 다형성을 지닌다.

아래 예시의 Computer와 Audio가 Product를 extends했다고 치면

```java
void buy(Computer c){
	money = money - c.price;
	bonusPoint = bonusPoint + c.bonusPoint;
}
void buy(Audio a){
	money = money - a.price;
	bonusPoint = bonusPoint + a.bonusPoint;
}
// 위에 중복되는 두개의 메서드는 조상 클래스인 Product에 멤버변수로 price와 bonusPoint가 있다면
// 다음과 같이 하나의 메서드로 처리해줄 수 있다.
void buy(Product p){
	money = money - p.price;
	bonusPoint = bonusPoint + p.bonusPoint;
}
```

## 여러 종류의 객체를 배열로 다루기

만약 아래의 Tv, Computer, Audio클래스가 모두 Product클래스를 extends 했다고 가정하면 다음과 같이 사용할 수 있다.

```java
Product p1 = new Tv();
Product p2 = new Computer();
Product p3 = new Audio();
// 위에 3줄을 아래와 같이 배열로 바꿀 수 있다.
Product[] p= new Product[3];
p[1] = new Tv();
p[2] = new Computer();
p[3] = new Audio();
```

# 6. 추상클래스

- 추상클래스는 미완성의 설계도이다.
- 미완성의 메서드(추상메서드)를 포함하고 있다.
- 추상클래스로는 인스턴스를 생성할 수 없다. 하지만 츠상클래스는 상속을 통해 자손클래스에 의해서만 완성될 수 있다.
- 추상클래스는 클래스에 키워드 **abstract**만 붙이면 된다.
- 추상클래스에도 생성자가 있으며, 멤버변수와 메서드도 가질 수 있다.

```java
abstract class 추상클래스이름 {
	// 다음과 같이 선언하여 사용한다.
}
```

## 추상메서드

추상메서드는 선언부만 작성하고 구현부는 작성하지 않은 채로 남겨 둔 메서드를 말한다. 즉 설계만 해놓고 실제 수행될 내용을 작성하지 않은 미완성의 메서드이다.

```java
// 주석을 통해 어떤 기능을 수행할 목적으로 작성하였는지 설명한다.
abstract 리턴타입 추상메서드이름();
```

메서드를 작성할 때 식제 작업내용인 구현부보다 더 중요한 부분은 선언부이다. 추상메서드를 사용하는 이유는 해당 메서드는 내용을 구현해주어야 한다는 사실을 인식하고 각자 알맞게 자신의 클래스에 맞춰서 구현하도록 알려주는 신호이다.

- 자손클래스에서 추상메서드를 반드시 구현하도록 강요하기 위해서이다.

# 7. 인터페이스

인터페이스는 일종의 추상클래스이다

인터페이스는 추상클래스처럼 추상메서드를 갖지만 추상클래스보다 추상화 정도가 높다. 추상클래스가 '미완성의 설계도'라면 인터페이스는 밑그림만 그려져 있는 '기본 설계도'이다.

- 몸통을 갖춘 일반 메서드를 가질 수 없다.
- 멤버변수를 가질 수 없다.
- 오직 추상메서드와 상수만을 멤버로 가질 수 있다.

```java
interface 인터페이스이름{
	public static final 타입 상수이름 = 값;
	public abstract 추상메서드(매개변수);
}
```

위에 예시와 같이 인터페이스를 작성 할 수 있다.

- 모든 멤버변수는 public static final 이어야 하며 이를 생략할 수 도 있다.
- 모든 메서드는 public abstract 이어야 하며 이를 생략 할 수도 있다.

## 인터페이스의 상속

인터페이스는 인터페이스로부터만 상속받을 수 있으며, 클래스와는 달리 다중 상속, 즉 여러개의 인터페이스로부터 상속을 받는것이 가능하다.

```java
interface Movable{
	void move(int x, int y);
}
interface Attackable{
	void attack(Unit u);
}
interface Fightable extends Movable, Attackable{}

```

따라서 Fightable 자체에는 정의된 멤버가 하나도 없지만 조상인터페이스로부터 상속받은 두개의 추상메서드 move, attack을 멤버로 갖게 된다.

## 인터페이스의 구현

인터페이스도 추상클래스처럼 그 자체로는 인스턴스를 생성할 수 없다.

implements 라는 키워드를 사용하여 인터페이스를 구현한다.

```java
class 클래스이름 implements 인터페이스이름{
	// 인터페이스에 정의된 추상메서드를 구현해야한다.
}
```

만약 구현하는 인터페이스의 메서드 중 **일부만 구현한다면 abstract를 붙여서 추상클래스 선언**해야한다

인터페이스의 모든 추상메서드를 구현 할 때만 implments를 사용한다.

## 인터페이스를 이용한 다형성

다형성에서 자손클래스의 인스턴스를 조상타입의 참조변수로 참조하는 것이 가능했다. 마찬가지로 인터페이스도 이를 구현한 클래스의 조상이라고 할 수 있으므로, 구현한 클래스의 인스턴스는 해당 인터페이스의 타입으로 참조 할 수 있으며 형변환 또한 가능하다.

인터페이스 Fightable을 클래스 Fighter가 구현했을 떄 다음과 같이 참조하는 것이 가능하다

```java
Fightable f = (Fightable) new Fighter();
Fightable f = new Fighter();

// 또한 인터페이스는 다음과 같이 매개변수의 타입으로 사용 될 수 있다.
void attack(Fightable f){
	// 내용
}

// 또한 리턴타입이 인터페이스라는 것은 메서드가 해당 인터페이스를 구현한 클래스의 인터페이스를 반환한다는 것을 뜻한다.
Fightable method() {
	Fighter f = new Fighter();
	return f;
}
```

## 인터페이스의 장점

- 개발 시간을 단축시킬 수 있다
- 표준화가 가능하다
- 서로 관계없는 클래스들에게 관계를 맺어 줄 수 있다.
- 독립적인 프로그래밍이 가능하다.

## 디폴트 메서드

인터페이스에 메서드를 추가할때 발생하는 문제

- 인터페이스에 메서드를 추가하는 것은, 추상 메서드를 추가한다는 것이기 때문에 이 인터페이스를 구현한 기존의 모든 클래스들이 새로 추가된 메서드를 구현해야한다.

따라서 **디폴트 메서드**라는 것을 만들었다.

- 디폴트메서드는 앞에 default 키워드를 붙인다.
- 추상메서드와 달리 일반메서드처럼 몸통{} 이 있어야한다

```java
interface My{
	void method();
	default void newMethod();
}
```

# Reference

- 남궁성, Java의 정석 (3rd Edition), 도우출판
