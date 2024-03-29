dp문제를 풀다가 lis를 사용해야하는 문제가 나왔다. 

예전에 알고특을 들을 때 그냥 가볍게 읽고 넘어간 내용이라 다시 떠오르려 하니 기억이 안 나는 상황 발생...(허걱)

완전히 이해하지 않고 그냥 이런거구나~ 하고 넘어가서 그런것같다. 

게다가 난 dp에 약하기도 하고... 문제를 풀 아이디어가 번뜩하고 떠올랐음 좋겠다 .....ㅠㅠ

쨋든 최장증가수열에 대해 포스팅 시작하겠다

최장증가수열

: 최장증가수열은 **길이 N인 수열이 있을때, 처음부터 하나씩 idx를 증가해나갈때 값이 증가하는 길이의 최댓값을 가지는 수열**이다.  

표로 보면 더 쉽다.

수열 arr

| value | 5 | 2 | 4 | 1 | 6 | 3 | 7 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| idx | 0 | 1 | 2 | 3 | 4 | 5 | 6 |

여기서 idx가 0인 5부터 시작해서 하나씩 idx를 증가해가면서 value가 증가하는 경우는 여러가지가 있다. 

{5,6}

{5,6,7}

.....

{2,4,6,7}

{4,6}

..

{1,3,7}

.....etc

끝까지 하며 모든 경우의 수를 구하면 {2,4,6,7}이 길이가 가장 길다는것을 알 수 있다. 

하지만 이런 방식으로 하나씩 계산하다보면 너무 시간이 오래 걸리게 된다. 

길이가 n인 수열을 n번 돌게 되므로 이를 빅오표기법으로 표시하면 O(N^2)의 시간복잡도를 가지게 된다. 

그래서 이 방법을 해결하기 위해 이 최장증가수열을 바이너리서치 / 세그먼트트리를 사용하면 O(NlogN)의 시간복잡도를 가질 수 있게 된다. 

\* 바이너리 서치 (이분탐색)

[https://www.youtube.com/watch?v=TocJOW6vx\_I&t=208s](https://www.youtube.com/watch?v=TocJOW6vx_I&t=208s)

<iframe src="https://www.youtube.com/embed/TocJOW6vx_I" width="860" height="484" frameborder="0" allowfullscreen="true"></iframe>

이 유튜브에서 꽤나 자세히 알려준다.

정리하자면 아래와 같다. 

1\. lis를 정리하는 배열을 하나 더 생성한다. (기존 배열은 arr, lis를 정리하는 배열은 lisArr이라한다. )

2\. arr의 원소를 인덱스 순서로 하나씩 lisArr에 넣는데, 이때 값의 크기에 따라 삽입한다. 

index i 에 따라 

\* 만약 lisArr의 맨 마지막에 있는 원소가 arr\[i\]보다 크다면 lisArr 마지막에 삽입한다. 

\* 만약 lisArr의 맨 마지막에 있는 원소가 arr\[i\]보다 작다면 바이너리 서치에 따라 맞는 위치에 lisArr 에 삽입한다. 

(1)  lisArr에 10 추가. 

arr

| 10 | 50 | 40 | 70 | 20 |
| --- | --- | --- | --- | --- |

lisArr

| 10 |   |   |   |   |
| --- | --- | --- | --- | --- |

(2)  50은 lisArr의 마지막 원소인 10보다 큰 수이므로 추가

arr

| 10 | 50 | 40 | 70 | 20 |
| --- | --- | --- | --- | --- |

lisArr

| 10 | 50 |   |   |   |
| --- | --- | --- | --- | --- |

(3)  40은 lisArr의 마지막 원소인 50보다 작은 수이므로, 바이너리서치를 통해 40이 들어갈 위치를 찾아 갱신.

arr

| 10 | 50 | 40 | 70 | 20 |
| --- | --- | --- | --- | --- |

lisArr

| 10 | 40 |   |   |   |
| --- | --- | --- | --- | --- |

(4)  70은 lisArr의 마지막 원소인 40보다 큰 수이므로 추가

arr

| 10 | 50 | 40 | 70 | 20 |
| --- | --- | --- | --- | --- |

lisArr

| 10 | 40 | 70 |   |   |
| --- | --- | --- | --- | --- |

(5)  70은 lisArr의 마지막 원소인 40보다 큰 수이므로 추가

arr

| 10 | 50 | 40 | 70 | 20 |
| --- | --- | --- | --- | --- |

lisArr

| 10 | 40 | 70 |   |   |
| --- | --- | --- | --- | --- |

(6)  20은 lisArr의 마지막 원소인 70보다 작은 수이므로, 바이너리서치를 통해 20이 들어갈 위치를 찾아 갱신.

arr

| 10 | 50 | 40 | 70 | 20 |
| --- | --- | --- | --- | --- |

lisArr

| 10 | 20 | 70 |   |   |
| --- | --- | --- | --- | --- |

\--> LIS의 최대길이는 3이다.

다만 여기서 주의할 점은 바이너리 서치로 진행하면 시간 복잡도가 줄어들지만, lisArr에 들어있는 값이 

LIS(최장증가수열)을 이루는 원소와는 다르다는 점이다. 위 방법으로 진행하면 arr의 index상 더 뒤에 있는 값이 

lisAr에서 더 앞에 있을 수 있다. 

\* 세그먼트 트리
https://blog.naver.com/kks227/220791986409
이 블로그에서 잘 설명하고 있다. 
나중에 이 이론을 사용해서 문제를 풀어봐야겠다...

\*이와 관련된 문제

[https://www.acmicpc.net/problem/7570](https://www.acmicpc.net/problem/7570)