## C++ STL

### pair

```c++
std::pair<int, int> dir[4] = {
    std::make_pair(-1, 0),
    std::make_pair( 1, 0),
    std::make_pair(0, -1),
    std::make_pair(0,  1),
};

auto p = dir[0];
pair.first;
pair.second;
```



### swap and compare

```c++
std::swap()
std::less<int>
std::greater<int>
```



### priority_queue as Heap

```c++
std::priority_queue<std::pair<int, int>> heap;
std::priority_queue<int,  std::vector<int>, std::greater<int>> min_heap;
```



### unordered_map as Hash

```c++
std::unordered_map<int, int> hash;
for (auto iter = hash.begin(); iter != hash.end(); ++iter)
{
	std::cout << iter->first << "," << iter->second << std::endl;
}
```



### sort

```c++
int numbers[] = {20,40,50,10,30};
std::sort(numbers, numbers+5, std::greater<int>());
```



### string

```c++
std::string str ("Test string");
str.length();

```

