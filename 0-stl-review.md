## C++ STL

```c++
// pair
std::pair<int, int> dir[4] = {
    std::make_pair(-1, 0),
    std::make_pair( 1, 0),
    std::make_pair(0, -1),
    std::make_pair(0,  1),
};

auto p = dir[0];
pair.first;
pair.second;

// swap and compare
std::swap(a, b);
std::less<int>;
std::greater<int>;

// sort
int numbers[] = {20,40,50,10,30};
std::sort(numbers, numbers+5, std::greater<int>());

// string
std::string str ("Test string");
str.length();

// list
std::list<int> l = {1, 2, 3};
l.front();
l.back()
l.push_front(0);
l.push_back(4);
l.pop_front();
l.pop_back();

// deque
dq.push_front();
dq.push_back();
dq.pop_front();
dq.pop_back();

// priority_queue as heap
std::priority_queue<std::pair<int, int>> heap;
std::priority_queue<int,  std::vector<int>, std::greater<int>> min_heap;

// unordered_map as hash
std::unordered_map<int, int> hash;
for (auto iter = hash.begin(); iter != hash.end(); ++iter)
{
	std::cout << iter->first << "," << iter->second << std::endl;
}

// unordered_set as hash
std::unordered_set<int> hash;
hash.erase();
hash.insert();
hash.count();
```



```c++

```

