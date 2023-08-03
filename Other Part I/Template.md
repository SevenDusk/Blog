# 取模整数 [Submission #27856679 - Panasonic Programming Contest 2021(AtCoder Beginner Contest 231)](https://atcoder.jp/contests/abc231/submissions/27856679)

```c++
template <int kMod> struct Mod_Int {
	static constexpr int Mod() {return kMod;}
 
	int val;
	Mod_Int() : val(0) {}
	template <typename T> constexpr Mod_Int(const T &x) : val(x) {}
 	//O(1)
	Mod_Int inv() const {return Pow(*this, kMod - 2);} 
 
	Mod_Int operator -() const {
		if (val) return Mod_Int(kMod - val);
		else return Mod_Int(0);
	}
 
	Mod_Int operator + (const Mod_Int &x) const {
		Mod_Int ans(val + x.val);
		if (ans.val >= kMod) ans.val -= kMod;
		return ans;
	}
	Mod_Int operator - (const Mod_Int &x) const {
		Mod_Int ans(val - x.val);
		if (ans.val < 0) ans.val += kMod;
		return ans;
	}
	Mod_Int operator * (const Mod_Int &x) const {return Mod_Int(1LL * val * x.val % kMod);}
	Mod_Int operator / (const Mod_Int &x) const {return *this * x.inv();}
	Mod_Int operator ^ (const Mod_Int &x) const {return Pow(*this, x.val);}
	Mod_Int operator << (const int &x) const {return ((1LL * val) << x) % kMod;}
 
	Mod_Int operator += (const Mod_Int &x) {return *this = *this + x;}
	Mod_Int operator -= (const Mod_Int &x) {return *this = *this - x;}
	Mod_Int operator *= (const Mod_Int &x) {return *this = *this * x;}
	Mod_Int operator /= (const Mod_Int &x) {return *this = *this / x;}
	Mod_Int operator ^= (const Mod_Int &x) {return *this = Pow(*this, x.val);}
	Mod_Int operator <<= (const int &x) {return *this = *this << x;}
 
	Mod_Int operator ++(int) {
		val++;
		if (val >= kMod) val -= kMod;
		return *this;
	}
	Mod_Int operator --(int) {
		val--;
		if (val < 0) val += kMod;
		return *this;
	}
 
	bool operator < (const Mod_Int &x) const {return val < x.val;}
	bool operator > (const Mod_Int &x) const {return val > x.val;}
	bool operator <= (const Mod_Int &x) const {return val <= x.val;}
	bool operator >= (const Mod_Int &x) const {return val >= x.val;}
	bool operator == (const Mod_Int &x) const {return val == x.val;}
	bool operator != (const Mod_Int &x) const {return val != x.val;}
 
	void out() const {printf("%d", val);}
};
 
using Mint = Mod_Int<kMod>;
 
namespace Factorial {
	Mint *f, *inf;
	bool preprocessed_factorial;
    //O(sz)
	void Pre_Factorial(const int &sz) {
		if (preprocessed_factorial) return ;
		preprocessed_factorial = true;
		f = new Mint[sz + 1];
		inf = new Mint[sz + 1];
		f[0] = f[1] = inf[0] = inf[1] = 1;
		for (int i = 2; i <= sz; i++) f[i] = f[i - 1] * i;
		inf[sz] = f[sz].inv();
		for (int i = sz; i > 2; i--) inf[i - 1] = inf[i] * i;
		return ;
	}
    //O(1)
	inline Mint P(const int &n, const int &m) {return f[n] * inf[m];}
	inline Mint C(const int &n, const int &m) {return f[n] * inf[m] * inf[n - m];}
	inline Mint H(const int &n, const int &m) {return f[n + m - 1] * inf[m] * inf[n - 1];}
	inline Mint Catalan(const int &n) {return f[n << 1] * inf[n] * inf[n + 1];}
}
 
namespace Factorial_No_Inf {
	Mint *f;
    //O(sz)
	void Pre_Factorial(const int &sz) {
		f = new Mint[sz + 1];
		f[0] = f[1] = 1;
		for (int i = 2; i <= sz; i++) f[i] = f[i - 1] * i;
		return ;
	}
    //O(1)
	inline Mint P(const int &n, const int &m) {return f[n] / f[m];}
	inline Mint C(const int &n, const int &m) {return f[n] / (f[m] * f[n - m]);}
	inline Mint H(const int &n, const int &m) {return f[n + m - 1] / (f[m] * f[n - 1]);}
	inline Mint Catalan(const int &n) {return f[n << 1] / (f[n] * f[n + 1]);}
}
 
namespace Inverse {
	using namespace Factorial;
	Mint *inv;
    //O(sz)
	void Pre_Inverse(const int &sz) {
		inv = new Mint[sz + 1];
		inv[1] = 1;
		Pre_Factorial(sz);
		for (int i = 1; i <= sz; i++) inv[i] = f[i - 1] * inf[i];
		return ;
	}
};
```

# 数论模块 [Submission #27856679 - Panasonic Programming Contest 2021(AtCoder Beginner Contest 231)](https://atcoder.jp/contests/abc231/submissions/27856679)

```C++
namespace MR32 {
	using ull = unsigned long long int;
	using uint = unsigned int;
    //O(log b)
	ull PowMod(ull a, ull b, ull kMod) {
		ull ans = 1;
		for (; b; b >>= 1, a = a * a % kMod) if (b & 1) ans = ans * a % kMod;
		return ans;
	}
 	//O(1) big constant factor
	bool IsPrime(uint x) {
		static constexpr bool low[8] = {false, false, true, true, false, true, false, true};
		static constexpr uint as = 3, a[3] = {2, 7, 61};
		if (x < 8) return low[x];
 
		uint t = x - 1;
		int r = 0;
		while ((t & 1) == 0) {
			t >>= 1;
			r++;
		}
		for (uint i = 0; i < as; i++) if (a[i] <= x - 2) {
			bool ok = false;
			ull tt = PowMod(a[i], t, x);
			if (tt == 1) continue;
			for (int j = 0; j < r; j++, tt = tt * tt % x) if (tt == x - 1) {
				ok = true;
				break;
			}
			if (!ok) return false;
		}
		return true;
	}
}
 
#ifdef __SIZEOF_INT128__
namespace MR64 {
	using uint128 = unsigned __int128;
	using ull = unsigned long long int;
	using uint = unsigned int;
    //O(log b)
	uint128 PowMod(uint128 a, uint128 b, uint128 kMod) {
		uint128 ans = 1;
		for (; b; b >>= 1, a = a * a % kMod) if (b & 1) ans = ans * a % kMod;
		return ans;
	}
 	//O(1) big constant factor
	bool IsPrime(ull x) {
		static constexpr bool low[8] = {false, false, true, true, false, true, false, true};
		static constexpr uint as = 7, a[7] = {2, 325, 9375, 28178, 450775, 9780504, 1795265022};
		if (x < 8) return low[x];
		ull t = x - 1;
		int r = 0;
		while ((t & 1) == 0) {
			t >>= 1;
			r++;
		}
		for (uint i = 0; i < as; i++) if (a[i] <= x - 2) {
			bool ok = false;
			uint128 tt = PowMod(a[i], t, x);
			if (tt == 1) continue;
			for (int j = 0; j < r; j++, tt = tt * tt % x) if (tt == x - 1) {
				ok = true;
				break;
			}
			if (!ok) return false;
		}
		return true;
	}
}
#endif
//O(1) big constant factor
bool IsPrime(unsigned long long int x) {
#ifdef __SIZEOF_INT128__
	if ((x >> 32) == 0) return MR32::IsPrime(x);
	else return MR64::IsPrime(x);
#endif
	return MR32::IsPrime(x);
}
 
#ifdef __SIZEOF_INT128__
//O(x^{1/3})
uint64_t PollardRho(uint64_t x) {
	static mt19937 rng;
	if (!(x & 1)) return 2;
	if (IsPrime(x)) return x;
  int64_t a = rng() % (x - 2) + 2, b = a;
  uint64_t c = rng() % (x - 1) + 1, d = 1;
  while (d == 1) {
    a = (__int128(a) * a + c) % x;
    b = (__int128(b) * b + c) % x;
    b = (__int128(b) * b + c) % x;
    d = gcd(uint64_t(abs(a - b)), x);
    if (d == x) return PollardRho(x);
  }
  return d;
}
//O(x^{1/3})分解质因数
template <typename T> vector<T> factorize(T x) {
	if (x <= 1) return vector<T>();
	T p = PollardRho(x);
	if (p == x) return vector<T>(1, x);
	vector<T> ans, lhs = factorize(p), rhs = factorize(x / p);
	Merge(lhs, rhs, ans);
	return ans;
}
#endif
 
// vec must be sorted
template <typename T> vector<pair<T, int>> Compress(vector<T> vec) {
	if (vec.empty()) return {};
 
	vector<pair<T, int>> ans;
	int cnt = 1, sz = int(vec.size());
	T lst = vec[0];
	for (int i = 1; i < sz; i++) {
		if (lst != vec[i]) {
			ans.push_back(make_pair(lst, cnt));
			lst = vec[i];
			cnt = 1;
		}
		else cnt++;
	}
	ans.push_back(make_pair(lst, cnt));
	return ans;
}
 
template <typename T> int Divisors(T x) {
	vector<pair<T, int>> fac = Compress(factorize(x));
 
	int ans = 1;
	for (pair<T, int> i : fac) ans *= i.second + 1;
 
	return ans;
}
//O(x^{1/3})
template <typename T> T phi(T x) {
	vector<pair<T, int>> fac = Compress(factorize(x));
 
	T ans = 1;
	for (pair<T, int> i : fac) {
		ans *= i.first - 1;
		for (int j = 1; j < i.second; j++) ans *= i.first;
	}
 
	return ans;
}
```

# 基本函数 [Submission #27856679 - Panasonic Programming Contest 2021(AtCoder Beginner Contest 231)](https://atcoder.jp/contests/abc231/submissions/27856679)

```C++
template <typename T, typename T1> void make_vector_inner(vector<T>& vec, T1 c) {return vec.push_back(T(c));}
template <typename T, typename T1, typename... Targs> void make_vector_inner(vector<T> &vec, T1 c, Targs... targs) {
	vec.push_back(T(c));
	return make_vector_inner(vec, targs...);
}
template <typename T, typename... Targs> vector<T> make_vector(Targs... targs) {
	vector<T> vec;
	make_vector_inner(vec, targs...);
	return vec;
}
 //O(n log n) 排序
template <typename T> inline void sort(vector<T> &v) {return sort(v.begin(), v.end());}
template <typename T> inline void sort_r(vector<T> &v) {return sort(v.begin(), v.end(), greater<T>());}
inline void sort(string &s) {return sort(s.begin(), s.end());}
inline void sort_r(string &s) {return sort(s.begin(), s.end(), greater<char>());}
 //O(n)
template <typename T> inline void reverse(vector<T> &v) {return reverse(v.begin(), v.end());}
inline void reverse(string &s) {return reverse(s.begin(), s.end());}
 //O(len(a)+len(b))
template <typename T> inline void Merge(vector<T> &a, vector<T> &b, vector<T> &c) {
	if (c.size() < a.size() + b.size()) c.resize(a.size() + b.size());
	merge(a.begin(), a.end(), b.begin(), b.end(), c.begin());
	return ;
}
//O(len(a)+len(b))
template <typename T> inline void Concatanate(vector<T> &a, vector<T> &b, vector<T> &c) {
	int a_size = int(a.size()), b_size = int(b.size());
	c.resize(a_size + b_size);
	for (int i = 0; i < a_size; i++) c[i] = a[i];
	for (int i = 0; i < b_size; i++) c[i + a_size] = b[i];
	return ;
}
//O(1)
template <typename T> inline void Append(vector<T> &lhs, vector<T> rhs) {
	int lsz = int(lhs.size()), rsz = int(rhs.size());
	lhs.reserve(lsz + rsz);
	for (int i = 0; i < rsz; i++) lhs.push_back(rhs[i]);
	return ;
}
 //O(n)
template <typename T> inline void Erase(vector<T> &vec, T x) {
	int sz = int(vec.size());
	for (int i = 0; i < sz; i++) if (vec[i] == x) {
		swap(vec[i], vec.back());
		vec.pop_back();
		break;
	}
	return ;
}
 //O(n log n) 离散化
template <typename T> inline void Discrete(vector<T> &v) {sort(v); v.resize(unique(v.begin(), v.end()) - v.begin()); return ;}
template <typename T> inline int Discrete_Id(vector<T> &v, T x) {return lower_bound(v.begin(), v.end(), x) - v.begin();}
 
template <typename T> using PQ = priority_queue<T>;
template <typename T> using PQ_R = priority_queue<T, vector<T>, greater<T>>;
 
template <typename T> inline T ABS(T n) {return n >= 0 ? n : -n;}
template <typename T> __attribute__((target("bmi"))) inline T gcd(T a, T b) {
	if (a < 0) a = -a;
	if (b < 0) b = -b;
	if (a == 0 || b == 0) return a + b;
	int n = __builtin_ctzll(a);
	int m = __builtin_ctzll(b);
	a >>= n;
	b >>= m;
	while (a != b) {
		int m = __builtin_ctzll(a - b);
		bool f = a > b;
		T c = f ? a : b;
		b = f ? b : a;
		a = (c - b) >> m;
	}
	return a << min(n, m);
}
// O(log n)
template <typename T> inline T lcm(T a, T b) {return a * (b / gcd(a, b));}
template <typename T, typename... Targs> inline T gcd(T a, T b, T c, Targs... args) {return gcd(a, gcd(b, c, args...));}
template <typename T, typename... Targs> inline T lcm(T a, T b, T c, Targs... args) {return lcm(a, lcm(b, c, args...));}
// O(1)
template <typename T, typename... Targs> inline T min(T a, T b, T c, Targs... args) {return min(a, min(b, c, args...));}
template <typename T, typename... Targs> inline T max(T a, T b, T c, Targs... args) {return max(a, max(b, c, args...));}
template <typename T, typename... Targs> inline void chmin(T &a, T b, Targs... args) {a = min(a, b, args...); return ;}
template <typename T, typename... Targs> inline void chmax(T &a, T b, Targs... args) {a = max(a, b, args...); return ;}
 // O(sqrt n)
vector<int> Primes(int n) {
	if (n == 1) return {};
	// 2 ~ n
	vector<int> primes;
	vector<bool> isPrime(n + 1, true);
 
	primes.reserve(n / __lg(n));
 
	for (int i = 2; i <= n; i++) {
		if (isPrime[i]) primes.push_back(i);
		for (int j : primes) {
			if (i * j > n) break;
			isPrime[i * j] = false;
			if (i % j == 0) break;
		}
	}
	return primes;
}
 // O(sqrt n)
template <typename T> vector<T> factors(T x) {
	// maybe use factorize would be faster?
	vector<T> ans;
	for (T i = 1; i * i <= x; i++) if (x % i == 0) ans.push_back(i);
 
	int id = int(ans.size()) - 1;
	if (ans[id] * ans[id] == x) id--;
	for (int i = id; i >= 0; i--) ans.push_back(x / ans[i]);
 
	return ans;
}
 //O(n)
int mex(vector<int> vec) {
	int n = int(vec.size());
	vector<bool> have(n, false);
	for (int i : vec) if (i < n) have[i] = true;
	for (int i = 0; i < n; i++) if (!have[i]) return i;
	return n;
}
 
//O(1)
template <typename T> T SQ(T x) {return x * x;}
 
// Euclidean distance
template <typename T> T Dist2(pair<T, T> lhs, pair<T, T> rhs) {return SQ(lhs.F - rhs.F) + SQ(lhs.S - rhs.S);}
template <typename T> T Dist2(T x1, T y1, T x2, T y2) {return SQ(x1 - x2) + SQ(y1 - y2);}
 
// Manhattan distance
template <typename T> T Mdist(pair<T, T> lhs, pair<T, T> rhs) {return ABS(lhs.first - rhs.first) + ABS(lhs.second - rhs.second);}
template <typename T> T Mdist(T x1, T y1, T x2, T y2) {return ABS(x1 - x2) + ABS(y1 - y2);}
 
template <typename T> bool Adj(pair<T, T> lhs, pair<T, T> rhs) {return Mdist(lhs, rhs) == 1;}
 
template <typename T> T LUBound(T LB, T val, T UB) {return min(max(LB, val), UB);}
 
template <typename T, typename Comp> T Binary_Search(T L, T R, Comp f) {
	// L good R bad
	static_assert(is_integral<T>::value, "Binary_Search requires an integral type");
	while (R - L > 1) {
		T mid = (L + R) >> 1;
		if (f(mid)) L = mid;
		else R = mid;
	}
	return L;
}
 //O(\log n f(n))
template <typename Comp> double Binary_Search(double L, double R, Comp f, int loop = 30) {
	for (int i = 1; i <= loop; i++) {
		double mid = (L + R) / 2;
		if (f(mid)) L = mid;
		else R = mid;
	}
	return L;
}
 // O(log n)
template <typename T> T nearest_dist(set<T> &se, T val) {
	static constexpr T kInf = numeric_limits<T>::max() / 2 - 10;
 
	if (se.empty()) return kInf;
	else if (val <= *se.begin()) return *se.begin() - val;
	else if (val >= *prev(se.end())) return val - *prev(se.end());
	else {
		auto u = se.lower_bound(val);
		auto v = prev(u);
		return min(*u - val, val - *v);
	}
}
 // O(log n)
template <typename T> T nearest_elem(set<T> &se, T val) {
	static constexpr T kInf = numeric_limits<T>::max() / 2 - 10;
 
	if (se.empty()) return kInf;
	else if (val <= *se.begin()) return *se.begin();
	else if (val >= *prev(se.end())) return *prev(se.end());
	else {
		auto u = se.lower_bound(val);
		auto v = prev(u);
 
		if (*u - val > val - *v) return *v;
		else return *u;
	}
}
```

# 网络流 [Submission #27857324 - Panasonic Programming Contest 2021(AtCoder Beginner Contest 231)](https://atcoder.jp/contests/abc231/submissions/27857324)

```c++
template <typename Flow, typename Cost>
struct network_simplex {
    // we number the vertices 0,...,V-1, R is given number V
 
    explicit network_simplex(int V) : V(V), node(V + 1) {}
 	//O(1)
    void add(int u, int v, Flow cap, Cost cost) {
        assert(0 <= u && u < V && 0 <= v && v < V);
        edge.push_back({{u, v}, cap, cost}), E++;
    }
 
    void set_supply(int u, Flow supply) { node[u].supply = supply; }
    void set_demand(int u, Flow demand) { node[u].supply = -demand; }
    auto get_supply(int u) const { return node[u].supply; }
 
    auto get_potential(int u) const { return node[u].pi; }
    auto get_flow(int e) const { return edge[e].flow; }
 
    auto reduced_cost(int e) const {
        auto [u, v] = edge[e].node;
        return edge[e].cost + node[u].pi - node[v].pi;
    }
 
    template <typename CostSum = Cost>
    auto circulation_cost() const {
        CostSum sum = 0;
        for (int e = 0; e < E; e++) {
            sum += edge[e].flow * CostSum(edge[e].cost);
        }
        return sum;
    }
 	//O(n) 检查生成树
    void verify_spanning_tree() const {
        for (int e = 0; e < E; e++) {
            assert(0 <= edge[e].flow && edge[e].flow <= edge[e].cap);
            assert(edge[e].flow == 0 || reduced_cost(e) <= 0);
            assert(edge[e].flow == edge[e].cap || reduced_cost(e) >= 0);
        }
    }
 //O(n) 检查最小环
    bool mincost_circulation() {
        static constexpr bool INFEASIBLE = false, OPTIMAL = true;
 
        // Check trivialities: positive cap[e] and sum of supplies is 0
        Flow sum_supply = 0;
        for (int u = 0; u < V; u++) {
            sum_supply += node[u].supply;
        }
        if (sum_supply != 0) {
            return INFEASIBLE;
        }
        for (int e = 0; e < E; e++) {
            if (edge[e].cap < 0) {
                return INFEASIBLE;
            }
        }
 
        // Compute inf_cost as sum of all costs + 1, and reset the flow network
        Cost inf_cost = 1;
        for (int e = 0; e < E; e++) {
            edge[e].flow = 0;
            edge[e].state = STATE_LOWER;
            inf_cost += edge[e].cost<0?-edge[e].cost:edge[e].cost;
        }
 
        edge.resize(E + V); // make space for V artificial edges
        bfs.resize(V + 1);
        children.assign(V + 1, V + 1);
 
        // Add V artificial edges with infinite cost and initial supply for feasible flow
        int root = V;
        node[root] = {-1, -1, 0, 0};
 
        for (int u = 0, e = E; u < V; u++, e++) {
            // spanning tree links
            node[u].parent = root, node[u].pred = e;
            children.push_back(root, u);
 
            auto supply = node[u].supply;
            if (supply >= 0) {
                node[u].pi = -inf_cost;
                edge[e] = {{u, root}, supply, inf_cost, supply, STATE_TREE};
            } else {
                node[u].pi = inf_cost;
                edge[e] = {{root, u}, -supply, inf_cost, -supply, STATE_TREE};
            }
        }
 
        // We want to, hopefully, find a pivot edge in O(sqrt(E)). This can be tuned.
        block_size = max((int)(ceil(sqrt(E + V))), min((int)10, V + 1));
        next_arc = 0;
 
        // Pivot pivot pivot
        int e_in = select_pivot_edge();
        while (e_in != -1) {
            pivot(e_in);
            e_in = select_pivot_edge();
        }
 
        // If there is >0 flow through an artificial edge, the problem is infeasible.
        for (int e = E; e < E + V; e++) {
            if (edge[e].flow > 0) {
                edge.resize(E);
                return INFEASIBLE;
            }
        }
        edge.resize(E);
        return OPTIMAL;
    }
 
  private:
    enum ArcState : int8_t { STATE_UPPER = -1, STATE_TREE = 0, STATE_LOWER = 1 };
 
    struct Node {
        int parent, pred;
        Flow supply;
        Cost pi;
    };
    struct Edge {
        array<int, 2> node; // [0]->[1]
        Flow cap;
        Cost cost;
        Flow flow = 0;
        ArcState state = STATE_LOWER;
    };
 
    int V, E = 0, next_arc = 0, block_size = 0;
    vector<Node> node;
    vector<Edge> edge;
    linked_lists children;
    vector<int> bfs; // scratchpad for downwards bfs and evert
 	//增广
    int select_pivot_edge() {
        // block search: check block_size edges looping, and pick the lowest reduced cost
        Cost minimum = 0;
        int e_in = -1;
        int count = block_size, seen_edges = E + V;
        for (int &e = next_arc; seen_edges-- > 0; e = e + 1 == E + V ? 0 : e + 1) {
            if (minimum > edge[e].state * reduced_cost(e)) {
                minimum = edge[e].state * reduced_cost(e);
                e_in = e;
            }
            if (--count == 0 && minimum < 0) {
                break;
            } else if (count == 0) {
                count = block_size;
            }
        }
        return e_in;
    }
 
    void pivot(int e_in) {
        // Find lca of u_in and v_in with two pointers technique
        auto [u_in, v_in] = edge[e_in].node;
        int a = u_in, b = v_in;
        while (a != b) {
            a = node[a].parent == -1 ? v_in : node[a].parent;
            b = node[b].parent == -1 ? u_in : node[b].parent;
        }
        int lca = a;
 
        // Orient the edge so that we add flow along u_in->v_in
        if (edge[e_in].state == STATE_UPPER) {
            swap(u_in, v_in);
        }
 
        // Let's find the saturing flow and exiting arc
        enum OutArcSide { SAME_EDGE, U_IN_SIDE, V_IN_SIDE };
        OutArcSide side = SAME_EDGE;
        Flow flow_delta = edge[e_in].cap;
        int u_out = -1;
 
        // Go up from u_in to lca, break ties by prefering lower vertices
        for (int u = u_in; u != lca && flow_delta > 0; u = node[u].parent) {
            int e = node[u].pred;
            bool edge_down = u == edge[e].node[1];
            Flow to_saturate = edge_down ? edge[e].cap - edge[e].flow : edge[e].flow;
 
            if (flow_delta > to_saturate) {
                flow_delta = to_saturate;
                u_out = u;
                side = U_IN_SIDE;
            }
        }
 
        // Go up from v_in to lca, break ties by prefering higher vertices
        for (int u = v_in; u != lca; u = node[u].parent) {
            int e = node[u].pred;
            bool edge_up = u == edge[e].node[0];
            Flow to_saturate = edge_up ? edge[e].cap - edge[e].flow : edge[e].flow;
 
            if (flow_delta >= to_saturate) {
                flow_delta = to_saturate;
                u_out = u;
                side = V_IN_SIDE;
            }
        }
 
        // Augment along the cycle if we can push anything
        if (flow_delta > 0) {
            auto delta = edge[e_in].state * flow_delta;
            edge[e_in].flow += delta;
 
            for (int u = edge[e_in].node[0]; u != lca; u = node[u].parent) {
                int e = node[u].pred;
                edge[e].flow += u == edge[e].node[0] ? -delta : delta;
            }
            for (int u = edge[e_in].node[1]; u != lca; u = node[u].parent) {
                int e = node[u].pred;
                edge[e].flow += u == edge[e].node[0] ? delta : -delta;
            }
        }
 
        // Return now if we didn't change the spanning tree. The state of e_in flipped.
        if (side == SAME_EDGE) {
            edge[e_in].state = ArcState(-edge[e_in].state);
            return;
        }
 
        // Basis exchange: Replace out_arc with e_in in the spanning tree
        int out_arc = node[u_out].pred;
        edge[e_in].state = STATE_TREE;
        edge[out_arc].state = edge[out_arc].flow ? STATE_UPPER : STATE_LOWER;
 
        // Put u_in on the same side as u_out
        if (side == V_IN_SIDE) {
            swap(u_in, v_in);
        }
 
        // Evert: Walk up from u_in to u_out, and fix parent/pred/child pointers downwards
        int i = 0, S = 0;
        for (int u = u_in; u != u_out; u = node[u].parent) {
            bfs[S++] = u;
        }
        for (i = S - 1; i >= 0; i--) {
            int u = bfs[i], p = node[u].parent;
            children.erase(p); // remove p from its children list and add it to u's
            children.push_back(u, p);
            node[p].parent = u;
            node[p].pred = node[u].pred;
        }
        children.erase(u_in); // remove u_in from its children list and add it to v_in's
        children.push_back(v_in, u_in);
        node[u_in].parent = v_in;
        node[u_in].pred = e_in;
 
        // Fix potentials: Visit the subtree of u_in (pi_delta is not 0).
        Cost current_pi = reduced_cost(e_in);
        Cost pi_delta = u_in == edge[e_in].node[0] ? -current_pi : current_pi;
 
        bfs[0] = u_in;
        for (i = 0, S = 1; i < S; i++) {
            int u = bfs[i];
            node[u].pi += pi_delta;
            FOR_EACH_IN_LINKED_LIST (v, u, children) { bfs[S++] = v; }
        }
    }
};
```

