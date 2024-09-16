# LRU 缓存

[146. LRU缓存](https://leetcode.cn/problems/lru-cache/description/)

## 题目简述 
设计一个 *最近最少使用缓存（LRU）* ，要求`put`操作和`get`操作，并可以控制缓存大小

## 方法一：双向链表

### 思路
1. 使用双向链表存储缓存
2. 双向链表方便移动和删除，本题要求删除指定节点，单向链表很难做到
3. 这个链表的功能：要找到调用最远的节点，要提取节点，要将节点放在最上面

### 构造
1. 为了实现`get`方法，需要构造`key->node`哈希表
2. 为了实现`put`方法，需要：删除节点，节点头插

### 整体代码
```
class LRUCache {
public:
    struct Node{
        int key, value;
        Node* prev, *next;
        // 构造函数
        Node(int k, int v): key(k), value(v){}
    };

    int capacity;
    Node *dummy;
    unordered_map<int, Node*> key_to_node; //key对应node

    void remove(Node *n){ //删除节点
        n->prev->next = n->next;
        n->next->prev = n->prev;
        return;
    }

    void push_front(Node *n){ //在表头插入节点
        n->next = dummy->next;
        n->prev = dummy;
        n->prev->next = n;
        n->next->prev = n;
        return;
    }

    Node* get_node(int key){
        auto find = key_to_node.find(key); //find返回pair
        if(find == key_to_node.end()) return nullptr; //没有找到
        Node* n = find->second;
        remove(n);
        push_front(n);
        return n;
    }

    LRUCache(int capacity) : capacity(capacity) {
        dummy = new Node(0, 0);
        dummy->prev = dummy;
        dummy->next = dummy;
    }

    int get(int key) {
        Node *n = get_node(key);
        if(!n) return -1;
        else return n->value;
    }
    
    void put(int key, int value) {
        Node* n = get_node(key);
        if(n){ //存在
            n->value = value;
            return;
        }
        else{ //新书
            Node *node = new Node(key, value);
            key_to_node[key] = node;
            push_front(node);
            if(key_to_node.size() > capacity){ //超出
                Node* end = dummy->prev;
                key_to_node.erase(end->key); //表中删除
                remove(end); //链表中删除
                delete end; //内存释放
            }
        }
        return;
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```

### 局部细节
1. `auto find = key_to_node.find(key);`，`hashmap.find()`返回值是`<,>`
2. `if(find == key_to_node.end())`代表没有找到
