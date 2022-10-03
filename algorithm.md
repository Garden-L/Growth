## 소수
### 1. 개념
약수가 자신과 1 밖에 없는 2이상인 정수

### 2. 에라토스테네스의 체, O(Nlog(logn))
#### 원리
2부터 N까지 각각의 배수를 제외하면 남은 수는 소수만 남게된다. 여러 소수를 판별할 때 미리 판별 배열을 만들어 놓으면 찾을 때 O(1) 시간에 찾을 수있다.

#### 소스코드
```c++
vector<int> eratosthenes(int n);
{
    vector<int> ret(n+1, true);
    
    for(int i = 2; i <= n/2; i++) // j가 i*2부터 출발하기 때문에 절반까지만 i를 반복한다.
    	for(int j = i*2; j <= n; j+=2)
	    ret[i] = false;
	    
    return ret;
}
```

### 3. 단일 숫자 소수 판별
#### 원리
다수의 숫자가 소수인지 판별 할때는 에라토스테네스의 체가 효율적이지만, 소수의 숫자를 판별할 때는 N의 제곱근 보다 작거나 같은 확인하는 것이 효율적이다. N의 제곱근까지 하는 이유는 N이 될려면 N의 약수 a\*b로 이루어져야하는데 a와 b는 최대 N의 제곱근과 같다 ( N^(1/2) x N^(1/2)).

```c++
bool isPrime(int n)
{
    for(int i = 2; i <= sqrt(n); i++)
        if(n%i != 0) return false;

    return true;
}
```

## MOD 연산

### 1. 개념
MOD는 나머지를 연산자를 의미한다. 
5 MOD 2 = 1

### 2. 사칙연산
#### (A+B) MOD C = ((A MOD C) + (B MOD C))MOD C 
A = CxK + A' -> AmodC = A'  (K=몫, A'=나머지)  
B = CxR + B' -> BmodC = B'  (0 <= A',B' <C)
A + B = (K + R)xC + (A' + B') 성립하고,  0<= A'+B' <2C 이므로 (A+B)modC = (A'+B')modC = (AmodC + BmodC)modC 이다.  
곱셉, 나눗셈, 뺄셈에서도 성립한다.

##### 자세한 것은 https://www.khanacademy.org/computing/computer-science/cryptography/modarithmetic/a/modular-multiplication

### 3. 긴 숫자의 나머지 구하기
C/C++에서는 int, long long 자료형으로는 엄청 긴 숫자의 나머지 연산이 불가능하다. 긴 숫자의 나머지를 구하기 위해서는 나머지 연산자의 성질을 이용하여 해결해야한다.

#### "86430"을 N으로 나눈 나머지 구하기
"86430"을 표현하면 8x10000 + 6x1000 + 4x100 + 3x10 + 0 으로 표현가능하고 이것을 다시 ((((0x10 + 8)x10 + 6)x10 + 4)x10 + 3)x10 +0 표현할 수있다. 이것은 모듈려 연산 덧셈, 곱셈으로 표현가능 하다.  
"86430" mod N = (((((0x10 + 8)modNx10 +6)modNx10+6)modNx10+4)modNx10+3)modNx10)modN+0)modN)  
프로그래밍 할때 최대한 mod 연산을 자제하는 것이 성능에 도움되므로 최대한 나눠야할 값을 높여서 나누는 것이 좋다. (modN x 10modN)modN + 8modN)modN 이런 연산은 자제해야한다.

#### 소스코드
```c++
//BOJ 1837
#include <iostream>
#include <string>
#include <vector>

#define SIZE 1000000

using namespace std;
vector<bool> eratos()
{
    vector<bool> ret(SIZE+1, true);
    for(int i = 2; i <= SIZE/2; i++)
    {
        for(int j = i*2; j <= SIZE; j += i)
            ret[j] = false;
    }

    return ret;
}

bool remainZero(const string& s, int n)
{
    int re = 0;

    for(char ch : s)
    {
        re = (re * 10 + (ch-'0')) % n; // N이 최대 10^6이기 때문에 mod연산을 int 형이 넘지만 안도록 취해주면 된다.
    }
    
    if (!re) return true;

    return false;
}

void solution(const string &s, int k)
{
    vector<bool> isPrime = eratos();

    for(int i = 2; i < k; i++)
    {
        if(isPrime[i] && remainZero(s, i))
        {
            cout << "BAD " << i;
            return;
        }
    }

    cout << "GOOD";
}


int main(void)
{
    string s;
    int k;

    cin >> s >> k;

    solution(s, k);

    
    return 0;
}
```

<br></br>
## 팰린드롬(palindrome)

### 1. 개념
앞뒤로 뒤집어도 똑같은 문자열을 palindrome이라 한다.
* 예시
  + aba  -> aba  O
  + abba -> abba O
  + acs  -> sca  X

### 2. 문자열 S가 팰린드롬인지 확인하는 알고리즘, O(N)
Two pointer 개념을 이용하여 문자열 S가 팰린드롬인지 확인가능하다. start를 시작포인트로 end를 마지막 포인트에 위치시켜 한 개씩 줄여나가면서 일치하는지 확인한다.
```c++
bool isPalindrome(const string& s)
{
    int left = 0, right = s.length()-1;
    while (left <= right)
    {
        if (s[left] != s[right]) return false;
        left++;
        right--;
    }

    return true;
}
```

### 3. 문자열 S의 부분문자열의 팰린드롬 최대 길이를 확인하는 알고리즘, O(N^2)
two pointer 개념을 이용하여 문자열 S를 순회하면서 순회지점부터 right - left 사이즈를 증가시키면서 각 위치에 문자가 일치하는지 확인한다. 단, 같은 지점에서 시작하는 경우와 옆에서 시작하는 경우 두 가지 케이스를 확인해야한다.
#### 케이스
1. left, right가 같은지점에서 시작하는 경우, S="aba"에서 b가 시작지점이라할 때 "aba"를 구할 수 있다. 하지만 S="abba"일 때, 첫번째 b지점에서 시작한다고 하면 이 문자열의 부분문자열은 팰린드롬이 존재하지 않는다.
2. 1번의 반례 때문에 right는 left의 1만큼 옆에서 시작해야한다. left는 첫 번째 b, right는 두 번째 b에서 시작하면 팰린드롬 "abba"를 구할 수 있다.

```c++
// 프로그래머스, https://school.programmers.co.kr/learn/courses/30/lessons/12904
int isPalindrome(const string& s, int left, int right)
{
    while (0 <= left && right <= s.length() - 1)
    {
        if (s[left] != s[right]) break;
        left--;
        right++;
    }

    //cabad 일경우 left= 0, right= 4 팰린드롬의 최대길이 3
    //cabbad 일경우 left = 0, right = 5 팰린드롬의 최대길이 4
    return right - left - 1;
}

int solution(string s)
{
    int answer = 0;

    for (int i = 0; i < s.length(); i++)
    {
        int odd, even;
        odd = isPalindrome(s, i, i); // 같은 지점
        even = isPalindrome(s, i, i + 1); // 1번 반례

        int _max = max(odd, even);

        answer = max(answer, _max);
    }

    return answer;
}
```

### 4. Problems
* BJ
  + https://www.acmicpc.net/problem/10942
* Programers
  + https://school.programmers.co.kr/learn/courses/30/lessons/12904

<br></br>
## Greedy (탐욕법)
### 1. 개념
각 단계마다 현재에서 가장 좋은 방법을 선택한다. 이 선택은 남은 선택들에 어떤 영향을 미칠지 고려하지 않는다.

#### greedy choice property (탐욕적 선택 속성)
탐욕적인 선택으로만으로도 최적해를 구할 수 있다. 동적계획법은 부분문제를 모두 고려하여 답을 도출하지만 탐욕법은 현재의 선택에만 집중한다. 차후 선택은 고려대상이 아니다. 현재 부분문제의 해결답이 차후 부분문제에 사용된다면 동적계획법, 차후에 사용되지 않는다면 탐욕법이다.

#### optimal substructure (최적 부분 구조)
부분문제의 최적해에서 전체 문제의 최적해를 만들 수 있는지 증명되어야 한다. 탐욕법은 대게 이 속성을 만족하기 때문에 따로 증명할 필요는 없다.

### 2. 문제
#### 거스름돈 문제
거스름돈문제는 잔돈이 배수인 경우만 가능하다. 잔돈이 10, 200, 400, 800 경우는 그리디로 해결가능하지만 잔돈이 10, 200, 300 인경우는 300이 10의 배수는 되지만 200의 배수는 안되기 때문에 그리디로 해결이 불가능하다. 만약 손님이 낸 잔돈이 900원이라면 그리디로 해결가능한 경우 800원 1개 10원 10개로 총 11개면 되지만 2번의 경우 200원 4개 10원 10개가 아닌 300원 3개로 그리디가 불가능한 것을 알 수 있다. 
* BOJ
  + https://www.acmicpc.net/problem/5585

#### 회의실 배정 문제
회의실 하나에 사람들이 원하는 시간대의 이용 신청을 받을때, 회의실 이용이 겹치지 않고 최대한 많이 회의를 하는 방법을 강구하는 문제. 회의가 끝나는 순으로 정렬하면 그리디한 문제로 해결 가능하다.

```c++
//BOJ 1931
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

bool compare(vector<int>& a, vector<int>& b)
{
    //문제의 조건
    if (a[1] == b[1]) return a[0] < b[0]; 
    return a[1] < b[1];
}

int solution(vector<vector<int>>& times)
{
    int answer = 0;

    //회의끝시간으로 정렬
    sort(times.begin(), times.end(), compare);

    int earliest = 0; //최근 끝난 회의의 마지막 시간

    for(int i = 0; i < times.size(); i++)
    {
        int meetingBegin = times[i][0], meetingEnd = times[i][1];

        // 최근 끝난 미팅 시간보다 현재 시작할 미팅 시간이 크거나 같다면
        // 새로운 미팅을 추가할 수 있음
        if(earliest <= meetingBegin) 
        {
            //새로운 미팅 시간을 최근 미팅으로 변경
            earliest = meetingEnd; 
            answer++; // 회의한 수
        }
    }

    return answer;
}

int main(void)
{
    int n;
    cin >> n;

    vector<vector<int>> times(n);

    for(int i = 0; i < n; i++)
    {
        int a, b;
        cin >> a >> b;
        times[i].push_back(a);
        times[i].push_back(b);
    }

    cout << solution(times);
    return 0;
}
```


#### 최소 신장 트리

<br></br>
## 최소 스패닝 트리
### 1. 개념
스패닝 트리란 그래프에서 속한 간선을 이용하여 모든 정점을 포함하는 트리이다. 간선의 가중치를 최소로 하여 스패닝 트리를 구성하면 최소 스패닝 트리(Minimum spanning tree)라고 한다. 하나의 그래프에 스패닝 트리는 여러개 나올 수 있다. 최소비용의 연결과 최단거리는 다르다. 
![image](https://user-images.githubusercontent.com/56042451/193393865-acdc17f4-9f6e-48a0-bb2d-21a5a3f3a859.png)

### 2. 크루스칼 알고리즘 O(ElogE)
크루스칼 알고리즘으 greey algorithm + disjoint set 을 이용한 방법이다. edge의 가중치가 최소인 경로부터 연결하면 최소 가중치로 신장트리를 구성할 수 있다. 

#### 의사코드
1. 모든 간선의 가중치를 오름차순으로 정렬한다.
2. 가중치가 가장 적은 간선의 정점을 연결한다.  
2-1. 정점이 이미 같은 집합에 속한다면 최소로 연결된 것이므로 연결하지 않는다. 같은 집합에 있음에도 불구하고 추가하면 사이클이 생기게된다.
3. 모든 정점을 끝까지 반복하면 최소 신장 트리가 완성된다.

#### 소스코드
```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

class djs{
private:
    vector<int> parent;
    vector<int> height;

public:
    djs(int size):
        parent(size), height(size)
    {
        for(int i = 0; i < parent.size(); i++)
            parent[i] = i;
    }

    int Find(int n)
    {
        while(n != parent[n])
            n = parent[n];
        
        return n;
    }

    void Union(int a, int b)
    {
        a = Find(a);
        b = Find(b);

        if( a == b) return;

        if( height[a] < height[b]) parent[a] = b;
        else
        {
            parent[b] = a;

            if(height[a] == height[b]) height[a]++;
        }
    }
};

bool compare(vector<int>& a, vector<int>& b)
{
    return a[2] < b[2];
}

int solution(int n, vector<vector<int>> costs)
{
    int answer = 0;
    djs set(n);

    sort(costs.begin(), costs.end(), compare);
    for(vector<int>& edge: costs)
    {
        int v1 = edge[0], v2 = edge[1];
        int weight = edge[2];
        if(set.Find(v1) != set.Find(v2))
        {
            set.Union(v1, v2);
            answer += weight;
        }
    }

    return answer;
}
```

### Prim algorithm(프림 알고리즘)
정점을 모두 다 돌면서 아직 속하지 못한 인접 정점 중 최소의 가중치를 찾아 연결해야하기 때문에 이 방법은 O(V^2)에 시간복잡도를 갖게 된다. 이 알고리즘은 다익스트라랑 유사하다. 하지만 우선순위 큐를 사용한다면 O(ElogV) 시간에 가능하다. 우선순위 큐에는 모든 정점이 반복적으로 들어간다. 이 정점은 간선으로 연결된 정점이다????
#### 의사코드
1. 초기 정점을 선택(아무거나 선택해도 모두 같은 최소 연결비용을 가진다)
2. 아직 추가되지 않은 정점중에서 인접한 최소 가중치의 정점을 추가한다.
3. 모든 정점이 추가될때까지 반복한다.

#### 소스코드

# Two pointer
## 방식
1. 한 포인터는 앞에서 나머지 한 포인터는 맨 뒤에서 시작하는 방식. 특정 중간 지점에서 만나는 조건이 종료조건이 된다.
2. 같은 지점에서 시작하는 방식, 앞서나가는 포인터는 fast pointer, 늦게 나가는 포인터를 slow pointer라 한다.


# range sum(구간합)
구간합이란 구간 (i, j)의 합을 의미한다.


## 누적합을 이용한 구간합, O(n)
누적합이란 (0,i) 구간의 합을 의미한다. 구간 \[i,j\]의 합을 구하기 위해서 (0,j)의 누적합에서 (0,i-1)의 누적합을 빼주면 i,j의 구간합을 구할 수 있다.

### 소스코드
``` c
#include<iostream>

using namespace std;

int prefixSum[100001];

int main(void) {
	std::ios_base::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);
	int N, M;

	cin >> N >> M;

	int sum = 0;
	for (int i = 1; i <= N; i++) {
		int num;
		cin >> num;
		sum += num;
		prefixSum[i] = sum;
	}

	for (int i = 0; i < M; i++) {
		int s, e;
		cin >> s >> e;
		cout << prefixSum[e] - prefixSum[s-1] << '\n';
	}

	return 0;
}
```

## segment tree를 이용한 구간합


### 소스코드
```c
#include<iostream>
#include<vector>
#define INF 1000000000+1
using namespace std;

int arr[100000];
int n;

class segment {
private:
	vector<int> tree;

public:
	segment() {
		tree.resize(4*n);
		init(1, 0, n-1);
	}

	int init(int node, int start, int end) {
		if (start == end) return tree[node] = arr[start];

		int mid = (start + end) / 2;

		return tree[node] = min(init(node * 2, start, mid), init(node * 2 + 1, mid + 1, end));
	}

	int query(int node, int start, int end, int rangeLeft, int rangeRight) {
		if (start > rangeRight || end < rangeLeft) return INF;
		if (rangeLeft <= start && end <= rangeRight) return tree[node];

		int mid = (start + end) / 2;

		return min(query(node * 2, start, mid, rangeLeft, rangeRight), query(node * 2 + 1, mid + 1, end, rangeLeft, rangeRight));
	}

	int query(int rangeLeft,int rangeRight) {
		return query(1, 0, n - 1, rangeLeft - 1, rangeRight - 1);
	}
};

int main(void) {
	std::ios_base::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

	int m;
	cin >> n >> m;

	for (int i = 0; i < n; i++)
		cin >> arr[i];

	segment seg;
	
	for (int i = 0; i < m; i++) {
		int a, b;
		cin >> a >> b;

		cout << seg.query(a, b) << '\n';
	}
	return 0;
}
```
## Fenwick tree를 이용한 구간합 O(logN)
펜윅 트리 오른쪽 자식이 없는 트리이다. 구간합, 누적합을 구하기 최적화 되어있다. 세그먼트에 비해 공간 활용도가 좋다(O(N+1)). 구현이 간단하다.

### 소스코드
```c

```

# divide and conquer 
### 개념
> 분할정복(divide and conquer)은 큰 문제를 작은 문제로 쪼개 해결하는 방법이다.

* divide : 문제를 하위문제로 분할. 하위 문제는 주어진 문제와 유사한 형태로 분할 되어야 하며 최소 2개 이상의 하위 문제로 분할 되어야한다.
* conquer : 하위 문제를 해결한다.
* combine : 해결된 하위 문제를 조합하여 상위 문제를 해결한다.

### 소스코드
```c
// 백준 1780
#include<iostream>
#define SIZE 20000

using namespace std;

int N;
int in[SIZE][SIZE];
int ans[3];
bool check(int y, int x, int n) {
	for (int dy = y; dy < y + n; dy++) {
		for (int dx = x; dx < x + n; dx++) {
			if (in[y][x] != in[dy][dx]) {
				return false;
			}
		}
	}

	return true;
}

void solution(int y, int x, int n) {
	if (check(y, x, n)) {// conquer
		ans[in[y][x]]++; 
	}
	else {
		int s = n / 3;

		for (int i = 0; i < 3; i++) {
			for (int j = 0; j < 3; j++)
				solution(y + i * s, x + j * s, n); //divide
		}
	}
}

int main(void) {
	cin >> N;

	for (int i = 0; i < N; i++)
		for (int j = 0; j < N; j++)
		{
			cin >> in[i][j];
			in[i][j]++;
		}
	solution(0, 0, 9);
	cout << ans[0] << endl << ans[1] << endl << ans[2];
	return 0;
}
```

<br></br>
# 문자열 search 알고리즘
문자열 search 알고리즘은 사용자가 원하는 문자열을 게시글 또는 긴 문자열에서 효율적으로 찾는 알고리즘이다.

## KMP Algorithm (Knuth-Morris-Pratt Algorithm)
커누스, 모리스, 플랫이 같이 개발한 string-Searching algorithm의 대표적인 알고리즘이다.

### 원리
#### 1
![image](https://user-images.githubusercontent.com/56042451/184581448-cb5571c4-84a7-4575-b47a-812770f1a729.png)
S의 0~3번 인덱스까지 전체 일치한다. 이후 계속 비교를 위해 S가 몇 번 인덱스로 이동해야할까? S의 2~3번 인덱스를 보면 S의 0~1번 인덱와 동일하므로 0번 -> 2번, 1번 -> 3번으로 이동하면 효율적으로 비교할 수 있을 것이다. 앞의 0~1번 AB가 접두사, 2~3이 접미사가 된다. 

#### 2
![image](https://user-images.githubusercontent.com/56042451/184581659-6e00ec21-a776-485c-962d-c4a025a4105f.png)
이것도 1번과 같이 앞에 2개가 일치하였다. 다음번 이동해야할 곳은 1번과 같다.

#### 3
![image](https://user-images.githubusercontent.com/56042451/184581742-d8cd2fc5-e603-4eb5-99c3-c346b76cc729.png)

#### 4
![image](https://user-images.githubusercontent.com/56042451/184582226-021c5fa9-2295-4693-9ac0-31b4b1bb8f30.png)

#### 5
![image](https://user-images.githubusercontent.com/56042451/184582120-68a696c5-2f48-49a9-8f20-ca2e5af05d2f.png)

#### 6
![image](https://user-images.githubusercontent.com/56042451/184582137-034d3c0f-233c-4ad0-91c4-d5352e10f33b.png)


### 개념
접두사와 접미사가 같은 경우 해당 접두사의 위치를 접미사로 옮겨 효율적으로 비교 가능하다.

#### prefix, suffix
> 찾으려는 문자열이 S라고 가정할 때, KMP 알고리즘에서 접두사, 접미사 
"abababaaa" 

