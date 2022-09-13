---
title:  "컬렉션 프레임워크에 대해 알아보기 "

categories:
  - collection
tags:
  - collection
  
last_modified_at: 2022-01-14T08:06:00-05:00
---

오늘은 자바의 컬렉션 프레임워크에 대해 알아보겠습니다. 자바에서는 데이터를 저장하는 여러 자료구조와 데이터를 처리하는 알고리즘을 구조화하여 표준화된 방법으로 다수의 데이터를 효과적으로 관리할 수 있도록 구현된 다양한 클래스(컬렉션)들을 제공해줍니다. 이러한 클래스들의 집합을 컬렉션 프레임워크라고 합니다. 이런 컬렉션들은 원소의 추가 삭제가 자유롭고, 배열처럼 한번 선언할 때 크기가 고정되는 것이 아니라 동적 배열의 성질을 가져서 크기에 자유롭습니다. 다만,<span style="color:yellow"> 컬렉션의 데이터들은 객체로서 일반 타입을 객체화해 일반타입처럼 사용할 수 있는 박스모델을 사용하게 되거나, 사용자 정의 객체를 데이터로 사용합니다.</span>

# 클레스를 구현하는 주요 인터페이스 

이런 <span style="color:yellow">컬렉션 프레임워크의 클래스(컬렉션)들</span>은 여러가지 인터페이스를 상속받아 구현됩니다.
자료구조의 종류에따라 주요 인터페이스인 <span style="color:yellow"> List인터페이스, Set인터페이스, Map인터페이스 </span>중 하나를  <span style="color:yellow">상속</span>받습니다.

또한, List와 Set 인터페이스는 모두 Collection 인터페이스를 상속받지만, 구조상의 차이로 Map 인터페이스는 별도로 정의됩니다.
List 인터페이스와 Set 인터페이스의 공통된 부분을 Collection 인터페이스에서 정의하고 있고, Map인터페이스는 따로 정의 되지만 Map을 상속받은 클래스들도 Collection으로 취급합니다.


    Collection -> List, Set (상속)
    Map( 따로 구현 but, 상속받은 클래스들은 collection 취급)

# List 인터페이스

 List 인터페이스를 상속받은 클래스들은 순서가 있는 데이터들의 집합으로서 데이터값의 중복을 허용합니다.
 
 ### List 인터페이스를 상속받는 컬렉션 클래스
  
  ArrayList, LinkedList, Vector, Stack  

# Set 인터페이스
 Set 인터페이스를 상속받는 클래스들은 데이터들이 순서를 유지하지 않고 중복을 허용하지 않습니다,

### Set 인터페이스를 상속받는 컬렉션 클래스
  
  HashSet, TreeSet 등

# Map 인터페이스
 Map 인터페이스를 상속받는 클래스들은 데이터들은 키와 값의 쌍으로 이루어져 있으며 순서를 유지하지 않고 키는 중복을 허용하지 않고, 값은 중복을 허용합니다.

### Map 인터페이스를 상속받는 컬렉션 클래스
  
  HashMap, TreeMap, HashTable, Properties 등  




# Collection 인터페이스의 매서드(Set과 List를 상속한 클래스는 모두 사용가능)


## 추가 


  1. boollean add(Object O) :지정된 객체를 컬렉션 뒤에 추가합니다. ex) col1.add(new Integer(11));


  2. boollean addAll(Collection c) :매개변수로 지정된 컬렉션의 객체들을 컬렉션 뒤에 추가 합니다.


  ex) col1.addAll( Set or List 혹은 그들의 자손 클래스);


## 삭제


  1. void clear(): Collection의 모든 객체를 삭제합니다.


  2. boollean remove(Object O): 지정된 객체를 컬렉션 뒤에 삭제합니다. 


  ex) col1.remove(new Integer(11));


  3. boollean removeAll(Collection c): 매개변수로 지정된 컬렉션에 포함된 객체들을 모두 삭제합니다.


  ex) col1.removeAll( Set or List 혹은 그들의 자손 클래스);


  4. boolean retainsAll(Collecion c):매개변수로 지정된 컬렉션에 있는 객체들을 제외한 모든 객체들을 삭제 합니다.


## 탐색
  1. boollean contains(Object O) 지정된 객체가 컬렉션에 포함되어 있다면 true를 반환합니다.ex) col1.add(new Integer(11));


  2. boollean containsAll(Collection c) 매개변수로 지정된 컬렉션의 객체들이 컬렉션에 포함되어 있다면 true를 반환합니다


  ex) col1.retainsAll( Set or List 혹은 그들의 자손 클래스);
## 기타

  1. boolean equal(Collection c):지정된 컬렉션과 동일한 컬렉션인지 확인합니다.


  2. boolean isEmpty() :컬렉션에 데이터가 비었는지 확인합니다.


  3. int size() :컬렉션에 저장된 객체 데이터의 수를 반환합니다.


  4. Iterator iterator(): 컬렉션의 이터레이터를 반환합니다. 
    ```java
    //ex)
      Iterator itr=col1.iterator();
      if(itr.hasNext()){
         System.out.println(itr.next());
       } 
    ```

  5. Object[] toArray(): 컬렉션에 저장된 객체들을 객체배열로 반환합니다.


  6. Object[] toArray(Object[]):매개변수로 지정된 배열에 컬렉션의 객체를 저장해서 반환합니다.     

  7. int hashCode(): 컬렉션의 해시코드를 반환합니다.

# List인터페이스의 메서드 (List를 상속한 클래스에서 사용, 객체 원소 간의 순서가 존재, 값의 중복 허용)

## 추가 
  
  1. void add(int index,Object element) : 지정된 위치(index)에 객체워소를 추가합니다.


  2. boelean addAll(int index,Collection c) : 지정된 위치에 지정된 컬렉션의 객체 원소를 추가합니다.  


## 삭제
  1. Object remove(int index):지정된 위치의 객체를 삭제합니다.


## 원하는 객체 가져오기
  
  1. Object get(int index): 지정된 위치에 있는 객체를 반환합니다.


  2. List sublist(int fromIndex, int toIndex): 지정된 범위의 객체들을 리스트로 반환합니다.

## 객체 원소 치환

  1. Object set(int index, Object O):  지정된 객체를 지정된 위치에 저장합니다.


## 객체의 위치 반환
  
  1. int indexOf(Object O): 지정된 객체의 위치를 반환합니다.(첫번째 원소부터 순방향으로 탐색)


  2. int lastIndexOf(Object O): 지정된 객체의 위치를 반환합니다.(마지막 원소부터 역방향으로 탐색)

## 기타 
  1. void sort(Comperator C):지정된 비교자로 List를 정렬합니다.


  2. ListIterator listIterator():리스트 객체에 접근할 수 있는 ListIterator를 반환합니다


  3. ListIterator listIterator(int index):지정된 위치의 리스트 객체부터 리스트 객체에 접근할 수 있는 ListIterator를 반환합니다

# Set 인터페이스의 메서드 (Set을 상속한 클래스에서 사용)

Set 인터페이스의 매서드는 앞서 설명한 Colection 메서드와 완전히 동일합니다. 다만, List와 달리 순서가 존재하지 않지 때문에 index에 대한 개념이 없고, 중복을 허용하지 않기 때문에 Set을 상속한 컬렉션이 이미 가진 원소를 추가하게 된다면, 적용되지 않습니다.

# map 인터페이스의 매서드(map을 상속한 클레스에서 사용, 순서 존재X, 키는 중복허용X but, 값은 중복허용)

## 추가
  
  1. Object put(Object key,Object value) :Map에 key객체를 value객체에 매핑하여 저장합니다.  


  2. void putAll(Map map) 지정된 Map의 모든 key-value쌍을 추가합니다.

## 삭제 
  1. Object remove(Object key):해당 키를 가진 key-value쌍을 삭제합니다.


  2. void clear(): Map의 모든 요소를 삭제합니다.
  
## Map의 모든 키, 값, 키-값 쌍을 Set,Collection으로 반환
  1. Set entrySet(): Map에 저장되어 있는 key-value쌍을 Map.Enrty 타입의 객체로 저장한 Set으로 반환합니다.


  2. Set keySet(): Map에 저장된 모든 key 객체들을 Set으로 반환합니다.


  3. Collection values():Map에 저장된 value를 반환합니다.

## 탐색
  1. Object get(Object Key): Map에서 해당 키를 가진 value객체를 반환합니다.


  2. boolean containsKey(Object key):지정된 키를 Map이 포함하는지 알려줍니다.


  3. boolean containsValue(Object value):지정된 값을 Map이 포함하는지 알려줍니다.


## 기타
  1. boolean equals(object obj(Map을 상속받은 객체) ): 두 개의 맵이 동일한 객체 원소를 가지고 있는지 확인합니다.


  2. boolean isEmpty() :Map에 데이터가 비었는지 확인합니다.


  3. int size() :Map에 저장된 객체 데이터의 수를 반환합니다. 

  4. int hashCode(): Map의 해시코드를 반환합니다.
