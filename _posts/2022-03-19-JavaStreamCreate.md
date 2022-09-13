---
title:  "자바 Stream에 대한 개념 및 생성"

categories:
  - stream
tags:
  - stream
  
last_modified_at: 2022-01-14T08:06:00-05:00
---

오늘은 Stream에 대해 공부해 보겠습니다. Stream은 자바8부터 지원하는 배열이나 컬렉션의 데이터 요소들을 하나식 참조해 조건에 맞게 가공하여 최종연산에 따라 원하는 결과를 반환하도록 하는 기능입니다.

## 스트림의 특징
  1. Stream은 원본 데이터를 읽기만하며, 원본으로부터 가공한 데이터를 반환하기만 할 뿐, 원본데이터에 변형을 가하지 않습니다.

  2. Stream은 이터레이터와 마찬가지로 한번 쓰고 버려지는 일회용입니다.

  3.Stream은 내부반복 구조로 되어있어 간결한 코드를 작성할 수 있습니다.

Stream은 배열이나 컬랙션으로부터 Stream을 생성하고, 중간연산들과 하나의 최종연산으로 이루어져 있으며, 최종연산이 끝나면 Stream은 더이상 사용할 수 없게 됩니다.

스트림을 사용하기 위해 java.util.stream.Stream를 import 해줘야 합니다.
IntStraem처럼 기본타입을 원소로 가지는 Stream 경우에는 java.util.stream.IntStream을 import하는것 처럼 따로 관련 패키지를 import해줘야 합니다.

# 스트림 생성하기 


## 컬렉션으로부터 스트림 생성하기

    List<Integer> list = Arrays.asList(1,2,3,4,5);  컬렉션(List) 생성
    
    Stream<Integer> intStream = list.stream();  생성한 컬렉션으로 스트림만들기  

  스트림을 만들 때는 Stream < T > Collection.stream(); 의 꼴을 따릅니다. 

## 배열로 부터 스트림 생성하기 
    1. Stream<String> strStream = Stream.of("a","b","c");  //원소를 직접 넘겨서 스트림을 만들 수 있습니다.
    
    2. Stream<String> strStream = Stream.of(new String[]{"a","b","c"});  
    
    3. Stream<String> strStream = Arrays.stream(new String[]{"a","b","c"});
    
    4. Stream<String> strStream = Arrays.stream(new String[]{"a","b","c"}, 0, 3); 
    
    //4. 배열과 함께 fromIndex와 ToIndex를 인자로 넘겨 배열의 일부분을 스트링 요소로 가져올 수 있습니다.
  
## 특정 범위의 정수를 요소로 갖는 스트림 생성하기(Stream)
    
    1. IntStream intStream = IntStream.range(1, 5); // 1,2,3,4
    
    2. IntStream intStream = IntStream.rangeClosed(1, 5); // 1,2,3,4,5 
   
## 난수를 요소로 갖는 스트림 생성하기

    1. IntStream intStream = new Random().ints(); //무한 스트림으로 중간연산에서 범위를 지정하지 않으면 요소를 계속 생성합니다.
    
    intStream.limit(5).forEach(System.out::println); 
    
    //위에서 무한 스트림으로 계속 난수를 받게 하고, 5개만 받도록 중간 연산으로 제어합니다. 최종연산으로 람다식을 지정해 요소를 출력합니다.  람다식(class or package:: 매서드)  
    
    2. IntStream intStream =  new Random().ints(5); 
    
    // 무한 스트림에 매개변수를 지정해줘 요소가 5개인 (Stream의 크기가 5인) Straem을 생성할 수 있습니다.

## 지정된 범위의 난수를 요소로 갖는 스트림 생성하기 

  begin 값과 end값 사이의 난수를 무한 스트림을 계속 받을 수 있습니다.
  
  1. IntStream ints(int begin, int end) 

  2. LongStream longs(long begin, long end) 

  3. DoubleStream doubles(double begin, double end)
  
  매개변수의 첫 번째 요소로 Stream의 크기를 넘겨주고, 사이즈의 개수 만큼 지정 범위 사이의 값들 중 랜덤하게 값을 받습니다.    

  4. IntStream ints(long streamSize, int begin, int end)
  
  5. LongStream longs(long streamSize, long begin, long end)
  
  6. DoubleStream doubles(long streamSize, double begin, double end)

## 람다식을 소스로하는 스트림 생성하기 

  ### 초기 값과 함께 람다식을 넘겨주어 모든 요소가 이전 요소의 종속적인 관계를 가지도록하는 스트림을 만듭니다.

  1. Stream<Integer> evenStream = Stream.iterate(0, n->n+2); // 0,2,4,6, ...

  ### 람다식만 넘겨주어 이전 요소에 독립적인 관계를 가지도록 하는 스트림을 만듭니다. 

  2. Stream<Double> randomStream = Stream.generate(Math::random);


  3. Stream<Integer> Stream<Integer> oneStream oneStream = Stream generate Stream.generate(()->1); 

