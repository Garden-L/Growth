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
