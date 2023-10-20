이번에 \*\*기업의 코테를 보다가 문제에서 hash map과 hash set을 사용하는 문제가 나왔다. 

해당 문제를 풀며 만약 ide사용이 안되었더라면 백퍼 틀렸을거라는 생각이 들었다. 

그래서 map과 set에 대해서 완벽히 (는 안되겠지만 그래도 최선을 다해) 이해하려고 글을 쓴다. 

**1\. Map**

Map은 인터페이스로, hashmap, LinkedListMap, TreeMap등으로 구현된다. 

Map< key 타입, value타입> = new Map인터페이스의 구현클래스; 형태로 초기화된다. 

작성하는 예시는 아래와같다.

```
 Map<String,Integer> maps = new HashMap<>();
```

Map은 아래와 같은 일부 메서드들을 가진다. 

| 메서드명 | 역할 | 구현 | 결과값 |
| --- | --- | --- | --- |
| put | 인수로 저장한 요소를 map에 추가한다. | maps.put ("test",1);   maps.put("test2",2); |   |
| remove | 인수로 저장한 키를 갖는 요소를 map에서 삭제한다. | maps.remove("test"); |   |
| get | 인수로 지정한 키의 값을 map에서 취득한다. | maps.get("test2"); | 2 |
| size | map안에 요소의 수를 취득한다. | maps.size(); | 1 |
| isEmpty | map안에 요소가 있는지 여부를판단한다. | maps.isEmpty(); | false |
| entrySet | map안에 모든 요소의 집합을 취득한다. | maps.entrySet(); |   |
| keySet | map안에 모든 요소의 키 집합을 취득한다. | maps.keySet(); |   |
| values | map안에 모든 요소의 값 집합을 취득한다. | maps.values(); |   |
| containsKey | map안에 특정 키가 포함되어있는지 판단한다. | maps.containsKey("test2"); |   |
| containsValue | map안에 특정 값이 포함되어있는지 판단한다. | maps.containsValue(2); |   |
| forEach | map안에 모든요소가 순차적으로 처리된다.  |   |   |

```
import java.util.*;
class Main {
        public static void main(String[] args) {
            Map<String,Integer> maps = new HashMap<>();
            maps.put ("test",1);
            maps.put("test2",2);
            maps.put("test3",3);
            maps.put("test4",4);
            maps.put("test5",5);

            maps.remove("test");
            maps.get("test2");
            System.out.println("maps.size() : "+ maps.size());
            System.out.println("maps.isEmpty() : "+maps.isEmpty());
            System.out.println("maps.entrySet() : "+ maps.entrySet());
            System.out.println("maps.keySet() : "+maps.keySet());
            System.out.println("maps.values() : "+maps.values());
            System.out.println("maps.containsKey(test2) : "+maps.containsKey("test2"));
            System.out.println("maps.containsValue(2) : "+maps.containsValue(2));

            maps.forEach((s, v) -> {
                System.out.println("Key: " + s + ", Value: " + v);
            });

            }

    }
```

위 코드로 테스트해보면 아래와 같은 결과가 나온다. 

[##_Image|kage@cML2m8/btsyTeD4hvE/c0dHnCbDXxzyM5ElADZD51/img.png|CDM|1.3|{"originWidth":533,"originHeight":256,"style":"alignCenter"}_##]

이때 maps.forEach()에 대해서는 잘 몰랐던 메서드라 한번 까보기를 했다.

[##_Image|kage@bnkrJb/btsyTtVl4Yp/o7sxIStCfx4tc2n6KbSwF0/img.png|CDM|1.3|{"originWidth":774,"originHeight":363,"style":"alignCenter"}_##]

해당 코드에 대해 해석을 해보자면

1\. forEach 는 BiConsumer라는 인터페이스를 매개변수로 받고 맵의 각 요소에 대해 BiConsumer를 호출한다.

\* BiConsumer는 매개변수로 키, 값 을 받아 동작을 수행하는 인터페이스다. 

[https://docs.oracle.com/javase/8/docs/api/java/util/function/BiConsumer.html](https://docs.oracle.com/javase/8/docs/api/java/util/function/BiConsumer.html)

 [BiConsumer (Java Platform SE 8 )

andThen default BiConsumer  andThen(BiConsumer  after) Returns a composed BiConsumer that performs, in sequence, this operation followed by the after operation. If performing either operation throws an exception, it is relayed to the caller of the compo

docs.oracle.com](https://docs.oracle.com/javase/8/docs/api/java/util/function/BiConsumer.html)

2\. 702Line은 

action이 null인 경우 널포인트 에러가 나는것을 방지하기 위함이다. 즉 action이 반드시 지정되어야한다. 

3\. 703Line은

entrySet()을 사용해 맵의 요소를 반복한다. 위 코드출력결과에서 봤듯이 entrySet()은 map안의 모든 요소를 취득한다. 

4\. 706 ~712 Line은 

try-catch문으로 entrySet()을 통해 키, 값 을 가져온다. 이때 만약 IllegalStateException이 발생했다면 

( 주로 맵이 더이상 존재하지 않을때) ConcurrentModificationException 예외를 던진다.

[https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/lang/IllegalStateException.html](https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/lang/IllegalStateException.html)

 [IllegalStateException (Java SE 20 & JDK 20)

All Implemented Interfaces: Serializable Direct Known Subclasses: AcceptPendingException, AlreadyBoundException, AlreadyConnectedException, CancellationException, CancelledKeyException, ClosedDirectoryStreamException, ClosedFileSystemException, ClosedSelec

docs.oracle.com](https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/lang/IllegalStateException.html)

\* ConcurrentModificationException  : 한 리스트를 다수의 스레드가 순회중인 상황을 가정. 

이 경우 어떤 스레드가 리스트를 수정할때 다른 스레드가 해당 리스트를 순회중이면 문제가 발생 할 수 있음.

이런 상황에 발생하는 예외임.

[https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/util/ConcurrentModificationException.html](https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/util/ConcurrentModificationException.html)

 [ConcurrentModificationException (Java SE 20 & JDK 20)

All Implemented Interfaces: Serializable Direct Known Subclasses: DirectoryIteratorException This exception may be thrown by methods that have detected concurrent modification of an object when such modification is not permissible. For example, it is not g

docs.oracle.com](https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/util/ConcurrentModificationException.html)
