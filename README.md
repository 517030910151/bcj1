# bcj1
问题:有任意两个村庄的距离，使全省任意两个村庄之间都可以畅通（不要求直接相连，间接联通也可以），要求铺设公路最短和次最短。请计算最小公路总长度 输入:第一行给出村庄数N(&lt;100),随后的N(N-1)/2给出村庄之间的距离，每行给出一对正整数，分别是两个村庄的编号,以及两个村庄之间的距离。村庄从1到N编号，N=0时结束 输出:求出最短公路总长度



//首先可以确定的是，最短的总长度一定是n-1条边，且这n条边不能是重复的

    #include <iostream>
    #include <cstring>
    using namespace std;

    int p[1000];
    struct edge {
    	int iv1;
    	int iv2;
	    int weight;
	    bool operator<=(edge& other) {
	    	return weight <= other.weight;
	    }
    };

    edge edgeArr[1000];
    int findRoot(int x) {
    	if (p[x] == -1)return x;
    	else {
    		int iroot = findRoot(p[x]);
    		p[x] = iroot;
    		return iroot;
    	}
    }

    int divide(edge a[], int low, int high) {
    	edge k = a[low];
    	do {
    		while (low < high&&k <= a[high])--high;
    		if (low < high) { a[low] = a[high]; ++low; }
    		while (low < high&&a[low] <= k)++low;
    		if (low < high) { a[high] = a[low]; --high; }
    	} while (low != high);
    	a[low] = k;
    	return low;
    }
    void quickSort(edge a[], int low, int high) {
    	if (low >= high)return;
    	int mid = divide(a, low, high);
    	quickSort(a, low, mid);
    	quickSort(a, mid + 1, high);
    }


    int main() {
	    ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
    	memset(p, -1, sizeof(p));
    	int N; cin >> N; int x, y, z;
	    while (N != 0) {
	    	int m = N * (N - 1) / 2;
	    	for (int i = 1; i <= m; ++i) {
	    		cin >> edgeArr[i].iv1 >> edgeArr[i].iv2 >> edgeArr[i].weight;
	    	}
	    	quickSort(edgeArr, 1, m); int sum = 0;
		    for (int i = 1; i <= m; ++i) {
		    	int root1 = findRoot(edgeArr[i].iv1);
			    int root2 = findRoot(edgeArr[i].iv2);
			    if (root1 != root2) {
			    	sum += edgeArr[i].weight;
			    	p[root1] = root2;
			    }
		    }
		    cout << sum << '\n';
		    memset(p, -1, sizeof(p));
		    cin >> N;
	    }
	    return 0;
    }
