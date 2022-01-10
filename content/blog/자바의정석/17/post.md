---
title: '[자바의 정석] 15장 입출력 I/O'
date: 2022-01-10 16:45:00
category: '자바의 정석'
draft: false
---

<!-- <p align="center"><img src="1.png" height="400px" width="600px"></p> -->

# 입출력이란?

I/O란 Input과 Output의 약자로 입력과 출력, 단단히 줄여서 입출력이라고 한다. 입출력은 컴퓨터 내부 또는 외부의 장치와 프로그램간의 데이터를 주고받는 것을 말한다.

- 이번 포스팅에서는 이러한 것들이 있다고 간단하게 익히고 필요할 때마다 찾아가는 식으로 하면 좋을 것 같다.

# 스트림(stream)

자바에서 입출력을 수행하려면, 두 대상을 연결하고 데이터를 전송할 수 있는 무언가가 필요한데 이것을 스트림(stream)이라고 한다.

- 이전 장(14장)의 스트림과 같은 용어를 쓰지만 다른 개념이다.
- 단방향 통신만 가능하다.
  - 입출력을 동시에 수행하려면 입력을 위한 입력스트림(input stream)과 출력을 위한 출력스트림(output stream) 모두 2개의 스트림이 필요하다.
  - 스트림은 먼저 보낸 데이터를 먼저 받게 되어있으며 중간에 건너뜀 없이 연속적으로 데이터를 주고받는다. (FIFO)

> 스트림이란 데이터를 운반하는데 사용되는 연결 통로이다.

## 바이트 기반 스트림

스트림은 바이트 단위로 데이터를 전송하며 입출력 대상에 따라 다음과 같은 입출력 스트림이 있다.

<p align="center"><img src="1.png" height="200px" width="600px"></p>
- InputStream 또는 OutputStream의 자손들이기 때문에 필요한 추상메서드를 자신에 맞게 구현해 놓았다.

<p align="center"><img src="2.png" height="200px" width="600px"></p>

- InputStream, OutputSteram을 구현할 때 read(), write()를 구현해야지만 다른 함수들도 다 사용 가능하다.
  - 다른 함수들은 read(), write()를 기반으로 만들어진 함수들이기 때문
- 위의 사진에 나온 함수들만 잘 쓸줄알면 데이터를 읽고 쓰는 것은 입출력 대상의 종류에 관계없이 잘 할 수 있다.

```java
    public abstreact class InputStream{
        ...
        // 입력스트림으로부터 1byte를 읽어서 반환한다. 읽을 수 없으면 -1을 반환한다.
        abstract int read();

        // 입력스트림으로부터 len개의 byte를 읽어서 byte배열 b의 off위치부터 저장한다.
        int read(byte[] b, int off, int len){
            ...
            for(int i=off; i<off+len; i++){
                // read()를 호출해서 데이터를 읽어서 배열을 채운다.
                b[i] = (byte) read();
            }
        }
        //  입력스트림으로부터 byte배열 b의 크기만큼 데이터를 읽어서 배열 b에 저장한다.
        int read(byte[] b){
            return read(b, 0, b.length);
        }
    }

```

## 보조스트림

볻조스트림은 스트림의 기능을 보완하기 위한 것으로, 실제 데이터를 주고받는 스트림이 아니기 때문에 데이터를 입출력할 수 있는 기능은 없지만, 스트림의 기능을 향상시키거나 새로운 기능을 추가할 수 있다.

```java
//기반 스트림 생성
FileInputStream fis = new FileInputStream("test.txt");
// 기반스트림을 이용해서 보조스트림을 생성한다
BufferedInputStream bis = new BufferedInputStream(fis);

bis.read(); // 보조스트림인 BufferedInputStream으로부터 데이터를 읽는다.
```

- 코드상으로는 보조스트림인 BufferedInputStream이 입력기능을 하는것으로 보이지만 실제 기능은 BufferedInputStream와 연결된 FileInputStream이 수행한다.
  - 보조스트림인 BufferedInputStream은 버퍼만 제공한다.
  - 버퍼를 사용한 입출력은 성능 향상이 상당하기에 대부분 버퍼를 이용한 보조스트림을 사용한다.

<p align="center"><img src="3.png" height="200px" width="600px"></p>

- 모든 보조스트림 역시 InputStream과 OutputSTream의 자손들이므로 사용법은 같다.

# 바이트 기반 스트림 상세

InputSTream과 OutputStream은 모든 바이트 기반 스트림의 조상이며 다음과 같은 메서드를 가지고있다.

<p align="center"><img src="4.png" height="200px" width="600px"></p>

```java
import java.io.*;
import java.util.Arrays;
public class IOEx3 {
    public static void main(String[] args){
        byte[] inSrc = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
        byte[] outSrc = null;
        byte[] temp = new byte[4];
        ByteArrayInputStream input = null;
        ByteArrayOutputStream output = null;

        input = new ByteArrayInputStream(inSrc);
        output = new ByteArrayOutputStream();

        System.out.println("Input Source : "+ Arrays.toString(inSrc));

        try{
            while(input.available() > 0){
                input.read(temp);
                output.write(temp);

                outSrc = output.toByteArray();
                printArrays(temp, outSrc);
            }
        }catch(IOException e){}
    }
    static void printArrays(byte[] temp, byte[] outSrc){
        System.out.println("temp         : " + Arrays.toString(temp));
        System.out.println("output Source: " + Arrays.toString(outSrc));
    }
}
```

위의 예시를 실행하면 결과는 다음과 같다

<p align="center"><img src="5.png" height="200px" width="600px"></p>

마지막에 출력하는 배열의 9번째와 10번째 값이 이전 출력값인 6,7 이 출력된다.

- 보다 나은 성능을 위해서 temp에담긴 내용을 지우고 쓰는 것이 아니라 기존 내용위에 덮어 쓰는 것이기 때문이다.

따라서 코드를 다음과 같이 변경해주어야 한다.

```java
while(input.availabe() > 0){
    int len = input.read(temp);
    output.write(temp, 0, len);
}
```

즉 읽어온 만큼만 출력하도록, 읽어온 만큼만 write하는 것이다.

## FileInputSTream 과 FileOutputStream

FileInputSTream 과 FileOutputStream은 이름처럼 파일에 입출력을 하기 위한 스트림이다. 실제 프로그래밍에서 많이 사용되는 스트림중 하나이므로 잘 익혀두자.

<p align="center"><img src="6.png" height="200px" width="600px"></p>

# 바이트 기반의 보조 스트림

## FilterInputStream, FilterOutputStream

FilterInputStream과 FilterOutputStream은 InputStream, OutputStream의 자손이면서 모든 보조스트림의 조상이다.

```java
protected FilterInputStream(InputStream in);
public FliterOutputStream(OutputStream out);
```

하지만 FilterInputStream의 생성자는 protected이므로 FilterInputStream자체를 바로 쓸 수는 없고 반드시 오버라이딩 해서 사용해야한다.

따라서 FilterInputStream/FilterOutputStream을 상속받아서 구현한 보조스트림 클래스는 다음과 같다

FilterInputStream의 자손

- BufferedInputStream
- DataInputStream
- PushbackInputStream

FilterOutputStream의 자손

- BufferedOutputStream
- DataOutputStream
- PrintSTream

## BufferedInputStream과 BufferedOutputStream

- 바이트 단위가 아닌 버퍼 크기만큼 데이터를 읽어서 자신의 내부 버퍼의 저장하기 때문에 작업효율이 좋으므로 많이 사용한다.
- 버퍼가 가득 찼을 때만 출력소스에 출력을 하기 때문에, 모든 출력작업을 마친 후에는 colse()나 flush()를 호출해서 버퍼에 있는 모든 내용이 출력 내용이 출력되도록 해야한다.
  <p align="center"><img src="7.png" height="250px" width="600px"></p>

## DataInputSTream과 DataOutputStream

데이터를 읽고 쓰는데 있어서 byte단위가 아닌 8가지 기본 자료형의 단위로 읽고 쓸 수 있다는 장점이 있다.

- 여러가지 종류의 자료형을 출력한 경우, 읽을 때는 반드시 쓰인 순서대로 읽어야 한다.
  <p align="center"><img src="8.png" height="600px" width="600px"></p>

## SequenceInputStream

여러개의 입력스트림을 연속적으로 연결해서 하나의 스트림으로부터 데이터를 읽는 것 같이 처리할 수 있도록 도와준다

- 큰 파일을 여러개의 작은 파일로 나누었다가 하나의 파일로 합치는 것과 같은 작업을 수행할 때 사용하면 좋다.
  <p align="center"><img src="9.png" height="100px" width="600px"></p>

사용 예는 다음과 같다

```java
// ex1)
Vector files = new Vector();
files.add(new FileInputStream("file.001"));
files.add(new FileInputStream("file.002"));
SequenceInputStream in = new SequenceInputStream(files.elements());

// ex2)
FileInputStream file1 = new FileInputStream("file.001");
FileInputStream file2 = new FileInputStream("file.002");
SequenceInputStream in = new SequenceInputStream(file1, file2);
```

## PrintStream

- 데이터를 다양한 양식으로 출력할 수 있도록 기능을 제공하는 보조스트림이다.
- System.out 이 printStream이다.
- JDK1.1에서부터 printStream보다 향상된 PrintWriter가 추가되었으나 System.out이 PrintStream이라 이를 사용하게 되지만 다양한 언어의 문자를 처리하는데는 PrintWriter가 적합하기 때문에 가능하면 PrintWriter를 사용하자.

문자열 형식화 양식

<p align="center"><img src="10.png" height="800px" width="600px"></p>

# Reference

- 남궁성, Java의 정석 (3rd Edition), 도우출판
- 사진 및 내용일부
  - https://blog.daum.net/gunsu0j/111
  - https://catsbi.oopy.io/20112bd1-0d38-48ab-b8bc-c01fded65fab
