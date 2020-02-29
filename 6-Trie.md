## Trie（字典树）

### 要点

* 字典树节点定义及操作
  * 是否是结束节点: `bool isEnd;`
    * set/get
  * 是否存在边连接子节点: `Node* link[26]`
    * set/get  by index
* 字典树：新增
  * 初始root节点
  * 新增时使用for循环
    * 不存在连接，需新增子节点并连接
    * 存在则更新当前节点
  * 设置为结束节点
* 字典树：查询
  * 同新增， 查询并判断结束状态



```c++
class Trie {
public:
    /** Initialize your data structure here. */

    struct Node
    {
        bool isEnd;
        Node* link[26];
        Node(){
            isEnd = false;
            for (int i = 0; i < 26; ++i)
            {
                link[i] = NULL;
            }
        }

        void SetLink(int index, Node* node)
        {
            link[index] = node;
        }

        Node* GetLink(int index)
        {
            return link[index];
        }

        void SetEnd(bool value)
        {
            isEnd = value;
        }
        bool GetEnd()
        {
            return isEnd;
        }
    };

    Node* root;
    Trie() {
        root = new Node();
    }
    
    /** Inserts a word into the trie. */
    void insert(string word) {
        Node* node = root;
        for (int i = 0; i < word.length(); ++i)
        {
            char ch = word[i];
            int index = ch - 'a';

            if (node->GetLink(index) == NULL)
            {
                Node* newNode= new Node();
                node->SetLink(index, newNode);
            }

            node = node->GetLink(index);
        }
        
        node->SetEnd(true);
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        Node* node = root;
        for (int i = 0; i < word.length(); ++i)
        {
            char ch = word[i];
            int index = ch - 'a';
            if (node->GetLink(index) == NULL)
            {
                return false;
            }

            node = node->GetLink(index);
        }

        return node->GetEnd();
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        Node* node = root;
        for (int i = 0; i < prefix.length(); ++i)
        {
            char ch = prefix[i];
            int index = ch - 'a';
            if (node->GetLink(index) == NULL)
            {
                return false;
            }

            node = node->GetLink(index);
        }

        if (node->GetEnd())
        {
            return true;
        }

        for (int i = 0; i < 26; ++i)
        {
            if (node->GetLink(i))
            {
                return true;
            }    
        }

        return false;
    }
};

/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * bool param_2 = obj->search(word);
 * bool param_3 = obj->startsWith(prefix);
 */
```

