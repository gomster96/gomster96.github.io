---
title: '[자바의 정석] 4장 조건문, 반복문'
date: 2021-12-23 16:24:13
category: '자바의 정석'
draft: false
---

# 조건문

# 1. if문

```java
if(조건식1) {
	if안에 조건식이 참일 경우 수행된다.
}
else if(조건식2){
	조건식1이 false이고 조건식 2가 true일 때 수행된다.
}
else{
	조건식 1, 2 가 모두 false일 때 실행된다.
}
```

- 자바에서 조건식의 결과는 반드시 true 또는 false이어야 한다.
  - 즉 조건식의 결과는 반드시 boolean 이여야 한다.

```java
int a = 0;
if(a){} // syntax error : int 형이 들어갔기 때문에
```

# 2. switch 문

단 하나의 조건식으로 많은 경우의 수를 처리할 때 사용한다.

```java
switch(조건식){
	case 값1 :
		// 수행할 문장
		break; // break를 적어주어야 switch문을 벗어난다.

	case 값2: case 값3 : // 이런식으로 여러 case를 한번에 적어줄 수 있다.
		break;
	default:
  	// 모든 case에 일치하지 않을 경우 수행되는 문장이다
}
```

- switch문의 조건식 결과는 정수 또는 문자열이어야 한다.
- case문의 값은 정수 상수만 가능하며, 중복되지 않아야 한다.

# 반복문

# 1. for

```java
for(초기화; 조건식; 증감식){
	수행될 문장
}
```

- for문의 증감식은 다양한 연산자들로 작성할 수 있다
  - ex) I++, I—, I+=2, I\*=3
- 증감식도 쉼표','를 이용해서 두문장 이상을 하나로 연결해서 쓸 수 있다.

```java
for(int i=1, j=10; i<=18; i++, j--);
```

- 초기화, 조건식, 증감식 모두 생략 가능하다

```java
for( ; ; ){} // 이 경우 무한루프이다.
```

## enhanced for 문

```java
for(타입 변수명 : 배열 또는 컬랙션) {
	// 반목할 문장
}
```

다음과 같이 for문을 변경해서 사용할 수 있다.

```java
for (int i=0; i<arr.length; i++){
	System.out.println(arr[i]);
}
// 다음과 같이 가독성을 얻을 수 있다.
for(int tmp : arr){
	System.out.println(tmp);
}
```

**향상된 for문은 일반적인 for문과 달리 배열이나 컬렉션에 저장된 요소들을 읽어오는 용도로만 사용할 수 있다.**

# 2. while

```java
while(조건) {
	// 조건이 만족되는 동안 해당 라인이 계속 진행됨
}
```

# 3. do-while

```java
do{
	// 조건식의 연산결과가 참일 때 수행
} while(조건식); // 끝에 반드시 ; 가 들어간다
```

최소한 한번은 수행될 것을 보장하는 while문이다.

# 이름 붙은 반복문

여러개의 반복문이 중첩된 경우에는 break문으로 중첩 반복문을 완전히 벗어날 수 없다.

→ 이때는 중첩 반복문 앞에 이름을 붙이고 break문과 continue문에 이름을 지정해 줌으로써 하나 이상으 ㅣ반복문을 벗어나거나 반복을 건너 띌 수 있다.

```java
Loop1 : for(int i=0; i<10; i++){
	Loop2: for(int j=0; j<10; j++){
		break Loop1; // Loop1 을 탈출하므로 전체 Loop를 탈출 하게 된다.
		break; // Loop2 를 탈출한다
		continue Loop1; // Loop2를 탈출하고 Loop1의 continue를 실행한다.
		continue; // Loop2에 continue를 실행한다.
	}
}
```
