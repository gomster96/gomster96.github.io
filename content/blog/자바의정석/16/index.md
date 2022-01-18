---
title: '[자바의 정석] 14-2장 스트림(Stream)'
date: 2022-01-07 19:51:00
category: '자바의 정석'
draft: false
---

# 스트림 (stream)

스트림은 데이터 소스를 추상화 하고, 데이터를 다루는데 자주 사용되는 메서드들을 정의해 놓았다.

- 예를 들어 list를 정렬할 때는 Collections.sort()를 써야하고 배열을 정렬할 때는 Arrays.sort()를 사용해야 하기 때문에 이런 불편함을 해결하기 위해 나타났다.

## 스트림의 특징

1. 스트림은 데이터 소스를 변경하지않는다.
2. 스트림은 일회용이다.
   - 스트림은 Iterator처럼 일회용이다.
   - 스트림은 한번 사용하면 닫혀서 다시 사용할 수 없으므로 필요하다면 다시 스트림을 생성해야한다.
3. 스트림은 작업을 내부 반복으로 처리한다.
   - 반복문을 내부에 숨겨 구현되어있다.

### 스트림의 연산

스트림이 제공되는 연산은 중간 연산과 최종 연산으로 분류 할 수 있다.

`중간연산` : 연산 결과가 스트림인 연산. 스트림에 연속해서 중간 연산할 수 있다.
`최종연산` : 연산결과가 스트림이 아닌 연산. 스트림의 요소를 소모하므로 단 한번만 가능

```java
stream.distinct() //중간연산
        .limit(5) //중간연산
        .sorted() //중간연산
        .forEach(System.out::println); // 최종연산
```

이렇게 중간연산은 stream을 반환하기 때문에 JS처럼 체이닝을 할 수 있다.

### 기본형 스트림

요소의 타입이 T인 스트림으로 인해 생기는 오토박싱&언박싱으로 인한 비효율을 줄이기 위해 데이터 소스를 기본형으로 다루는 스트림을 만들었다. 이는 Stream<Integer>이렇게 하는 것보다 효율이 좋다.

- IntStream
- LongStream
- DoubleStream

### 병렬스트림

parallel()이라는 메서드를 호출해서 병렬로 연산을 수행하도록 지시한다. 모든 스트림은 기본적으로 병렬 스트림이 아니고 parallel()을 호출 한 것을 취소할 때만 sequential()을 사용한다

```java
int sum = strStream.parallel() // strStream을 병렬스트림으로 전환
                   .mapToInt(s -> s.length())
                   .sum();
```

병렬처리가 항상 더 빠른 결과를 얻게 해주는 것은 아니므로 필요할 때만 사용한다.

## 스트림 만들기

스트림의 소스가 될 수 있는 대상은 배열, 컬렉션, 임의의 수 등 다양하다.

### 컬랙션

컬렉션의 최고 조상인 Collection에 stream()이 정의되어 있다.

```java
Stream<T> Collection.stream();
///
List<Integer> list = Arrays.asList(1,2,3,4,5);
Stream<Integer> intStream = list.stream();
```

### 배열

배열을 소스로 하는 스트림을 생성하는 메서드는 Stream과 Arrays에 static메서드로 저장되어있다.

```java
Stream<T> Stream.of(T... values) //가변인자
Stream<T> Stream.of(T[])
Stream<T> Arrays.stream(T[])
Stream<T> Arrays.stream(T[] array, int startInclusive, int endExclusive)
```

### 특정 범위의 정수

IntStream과 LongStream은 지정된 범위의 연속된 정수를 스트림으로 생성해서 반환하는 함수인ㅇ range()와 rangeClosed()를 가지고 있다.

```java
IntStream IntStream.range(int begin, int end) // end 미포함
IntStream IntStream.rangeClosed(int begin, int end) // end 포함
```

### 임의의 수

아래 메서드들은 해당 타임의 난수들로 이루어진 스트림을 반환하며 스트림의 크기가 정해지지 않은 무한스르팀이므로 limit()도 같이 사용해서 스트림의 크기를 제한해 주어야한다.

```java
IntStream ints()
LongStream longs()
DoubleStream doubles()

//예시 limit
intStream.limit(5).forEach(System.out::println) // 5개의 요소만 출력한다.
// 또는 매개변수로 스트림의 크기를 지정해서 유한스트림을 생성할 수 있다.
IntStream ints(long streamSize)
LongStream longs(long streamSize)
DoubleStream doubles(long streamSize)
```

### 람다식 - iterate(), generate()

Stream클래스의 iterate()와 genereate()는 람다식을 배개변수로 받아서, 이 람다식에 의해 계산되는 값들을 요소로 하는 `무한 스트림`을 생성한다

```java
static<T> Stream<T> iterate(T seed, UnaryOperator<T> f)
// seed값으로 시작해서 계산을 시작하며, 계산된 결과를 다시 seed값으로 해서 결과를 반복한다.
static<T> Stream<T> generate(Supplier<T> s)
// 매개변수가 없는 람다식을 허용한다.

//ex
Stream<Integer> evenStream = Stream.iterate(0, n->n+2);
// 0으로 시작해서 0, 2, 4, 6 ... 이렇게 생성된다.
Stream<Double> randomStream = Stream.generate(Math::random);
```

# 스트림의 중간연산

## 스트림 자르기 - skip(), limit()

skip()과 limit()는 스트림의 일부를 잘라낼때 사용하며, skip(3)은 처음 3개의 요소를 건너뛰고, limit(5)는 스트림의 요소를 5개로 제한한다.

```java
Stream<T> skip(long n)
Stream<T> limit(long maxSize)

// ex
IntStream intStream = IntSTream.rangeClosed(1, 10); // 1~10의 요소를 가진 스트림
intStream.skip(3).limit(5).forEach(System.out::print); //45678 출력
```

## 스트림의 요소 걸러내기 - filter(), distinct()

distinct()는 스트림에서 중복된 요소들을 제거하고, filter()는 주어진 조건(Predicate)에 맞지않는 요소를 걸러낸다.

```java
Stream<T> filter(Predicate<? super T> predicate);
Stream<T> distinct()
```

- filter는 Predicate를 필요로 하지만, boolean인 람다식을 사용해도 된다.
- distinct()와 filter()는 둘다 중간연산자이다.
- 필요하다면 filter()를 다른 조건으로 여러 번 사용할 수 있다.

## 정렬 - sorted()

스트림을 정렬할 때는 sorted()를 사용한다.

```java
Stream<T> sorted()
Stream<T> sorted(Comparator<? super T> comparator)
```

- Comparator를 지정하지 않으면 스트림 요소의 기본 정렬 기준(Comparable)으로 정렬한다.
- 스트림의 요소가 Comparable을 구현한 클래스가 아니면 예외가 발생한다.

```java
strStream.sorted(); // 기본정렬
strStream.sorted(Comparator.naturalOrder()); // 기본정렬
strStream.sorted((s1, s2) -> s1.compareTo(s2));
strStream.sorted(String::compareTo);
```

Comparator에서 정렬에 사용되는 메서드의 개수가 많지만, 가장 기본적인 메서드는 comparing이다.

```java
comparing(Function<T, U> keyExtrator)
comparing(Function<T, U> keyExtrator, Comparator<U> keyComparator)
// 정렬 조건을 추가할 때는 thenComparing()을 사용한다
studentStream.sorted(Comparator.comparing(Student::getBan))
            .thenComparing(Student::getTotalScore)
            .thenComparing(Student::getName)
            .forEach(System.out::println);
```

자세히 사용하는 예시는 아래에 있으니 잘 익혀보도록 하자.

```java
import java.util.*;
import java.util.stream.*;
public class StreamEx1 {
    public static void main(String[] args){
        Stream<Student> studentStream = Stream.of(
                new Student("이자바", 3, 300),
                new Student("김자바", 1, 200),
                new Student("안자바", 2, 100),
                new Student("박자바", 2, 150),
                new Student("소자바", 1, 200),
                new Student("나자바", 3, 290),
                new Student("감자바", 3, 180)
        );
        // comparator를 구현하여 ban별로 내림차순
        studentStream.sorted(Comparator.comparing(Student::getban, (s1, s2)-> {
                            return s2 - s1;
                        })
                        .thenComparing(Comparator.naturalOrder()))
                .forEach(System.out::println);// 반별 정렬

        // 기본정렬 반별 오름차순
//        studentStream.sorted(Comparator.comparing(Student::getban)
//                .thenComparing(Comparator.naturalOrder()))
//                .forEach(System.out::println);// 반별 정렬

    }
}

class Student implements Comparable<Student>{
    String name;
    int ban;
    int totalScore;
    Student(String name, int ban, int totalScore){
        this.name = name;
        this.ban = ban;
        this.totalScore = totalScore;
    }
    public String toString(){
        return String.format("[%s, %d, %d]",name, ban, totalScore);
    }
    String getName() {return name;}
    int getban() { return ban; }
    int getTotalScore() { return totalScore; }

    // 다중 조건을 주고싶으면 그냥 comparator를 잘 만들면 된다.
    public int compareTo(Student s){
        return s.totalScore - this.totalScore;
//        if(s.ban == this.ban) return s.totalScore - this.totalScore;
//        return s.ban - this.ban;
    }
}

```

## 변환 - map()

스트림의 요소에 저장된 값중에서 원하는 필드만 뽑아내거나 특정 형태로 변환해야 할때 사용한다

```java
Stream<R> map(Function<? super T, ? extends R> mapper)

// ex
Stream<String>  filenameStream = fileStream.map(File::getName);
filenameStream.forEach(System.out::println)
```

- map() 은 중간연산자이므로 스트림을 반환한다.
- map()도 filter()처럼 하나의 스트림에 여러번 적용할 수 있다.

## 조회 - peek()

- forEach()와 달리 스트림의 요소를 소모하지 않고 연산사이가 올바른지 확인이 가능하다.
- 연산 사이에 여러번 끼워 넣어도 된다.

## mapToInt(), mapToLong(), mapToDouble()

```java
// 더 효율적으로 만들어주는 기본형 스트림으로 바꿔주는 map
DoubleStream mapToDouble(ToDoubleFunction<? super T> mapper)
IntStream mapToDouble(ToIntFunction<? super T> mapper)
LongStream mapToDouble(ToLongFunction<? super T> mapper)

// 위의 기본형 스트림들은 아래의 함수를 사용할 수 있다.
int sum() // 스트림의 모든 요소의 총합
OptionalDouble average() // sum() / (double)count()
OptionalInt max() //스트림의 요소 중 제일 큰 값
OptionalInt min() //스트림의 요소 중 제일 작은 값
```

## flatMap() - Stream<T[]>를 Stream<T>로 변환

```java
// 아래처럼 Stream<String>[]을 Stream<String>으로 바 꿀 수 있다.
Stream<String[]> strArrStrm = Stream.of(
                new String[]{"abc", "def", "jkl"},
                new String[]{"ABC", "GHI", "JKL"}
        );

Stream<String> strStrm = strArrStrm.flatMap(Arrays::stream);
```

# Optional\<T>와 OptionalInt

Optional\<T>는 지네릭 클래스로 'T타입의 객체'를 감싸는 래퍼클래스이다. 그래서 Optional 타입의 모든 객체에는 모든 타입의 참조변수를 담을 수 있다

```java
public final class Optional<T>{
    private final T value; // T타입의 참조변수
}
```

- 최종 연산의 결과를 Optional객체에 담아서 반환할 때가 있다.
- Optional객체에 담으면, 반환된 결과가 null인지 매번 체크할 필요없이 Optional에 정의된 메서드를 통해 간단히 처리할 수 있다.
  - isPresent() 함수를 이용하여 null 여부를 확인할 수 있다.
- Stream처럼 Optional 객체에도 filter(), map(), flatMap()을 사용할 수 있다.

# 스트림의 최종연산

## forEach

반환타입이 void이므로 스트림의 요소를 출력하는 용도로 많이 사용된다

```java
void forEach(Consumer<? super T> action)
```

## 조직검사 - allMatch(), anyMatch(), noneMatch(), findFirst(), findAny()

요소들이 일치하는지, 일부가 일치하는 지 등을 확인하는 데에 사용하는 메서드들이다. 모두 매개변수로 Predicate를 요구하며, 연산 결과로 boolean을 반환한다.

```java
boolean allMatch(Predicate<? super T> predicate)
boolean anyMatch(Predicate<? super T> predicate)
boolean noneMatch(Predicate<? super T> predicate)

<T> findFirst(Predicate<? super T> predicate) // 조건에 일치하는 첫번쨰 것을 반환한다
<T> findAny(Predicate<? super T> predicate) // 조건에 일치하는 첫번쨰 것을 반환한다, 병렬 스트림의 경우 사용한다.
```

## 리듀싱 - reduce()

스트림의 요소를 줄여나가면서 연산을 수행하고 최종 결과를 반환한다. 따라서 매개변수의 타입이 BinaryOperator\<T>인 것이다. 처음 두 요소를 가지고 연산한 결과를 가지고 그 다음 요소와 연산한다.

```java
Optional<T> reduce(BinaryOperator<T> accumulator)
```

## Collect()

```java
collect() : 스트림의 최종연산, 매개변수로 컬렉터를 필요로 한다.
Collector : 인터페이스. 컬렉터는 이 인터페이스를 구현해야 한다.
Collectors : 클래스. static메서드로 미리 작성된 컬렉터를 제공한다.
```

collect()는 매개변수 타입이 Collector인데, 매개변수가 Collector를 구현한 클래스의 객체이어야 한다는 뜻이다.

### 스트림을 컬렉션과 배열로 변환 - toList(), toSet(), toMap(), toCollection(), toArray()

```java
List<String> names = stuStream.map(Student::getName)
                                .collect(Collectors.toList());
ArrayList<String> list = names.stream()
                                .collect(Collectors.toCollection(ArrayList::new));
Map<String, Person> map = persionStream
                            .collect(Collectors.toMap(p->p.getRegId(), p->p));
```

### 다양한 활용

- 리듀싱 : reducing()
- 문자열 결합 : joining()
- 통계 : counting(), summingInt(), averagingInt(), maxBy(), minBy()
- 그룹화와 분할
  - groupingBy() : 스트림의 요소를 Function으로 받고 결과는 Map에 담겨 나온다
  - partitioningBy() : 스틀미의 요소를 Predicate로 받고 결과는 Map에 담겨 나온다

### Collector 구현하기

Collectors클래스가 제공하는 컬렉터를 사용할수도 있지만 Collector를 직접 만들 수 도 있다.

4개의 람다식을 작성하면 그에따라 Collector를 사용할 수 있다.

```java
supplier() : 작업 결과를 저장할 공간을 제공
accumulator() : 스트림의 요소를 수집(collect)할 방법을 제공
combiner() : 두 저장공간을 병합할 방법을 제공(병렬 스트림)
finisher() : 결과를 최종적으로 변환할 방법을 제공
```

# Reference

- 남궁성, Java의 정석 (3rd Edition), 도우출판
