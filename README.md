```C++
#include <iostream>
#include <vector>
#include <map>
#include <string>
#include <algorithm>
#include <set>
#include <cassert>
using namespace std;

struct Node{
   Node* next;
   Node* prev;
   int value;
   int key;
   Node(Node* p, Node* n, int k, int val):prev(p),next(n),key(k),value(val){};
   Node(int k, int val):prev(NULL),next(NULL),key(k),value(val){};
};

class Cache{
   
   protected: 
   map<int,Node*> mp; //map the key to the node in the linked list
   int cp;  //capacity
   Node* tail; // double linked list tail pointer
   Node* head; // double linked list head pointer
   virtual void set(int, int) = 0; //set function
   virtual int get(int) = 0; //get function

};
class LRUCache : public Cache {
    void clearCache() {
        std::map<int, Node*>::iterator it;
        it = this->mp.begin();
        while (it != mp.end()) {
            delete it->second;
            it++;
        }
    }
    public:
    static int count;
    LRUCache(int capacity) {
        this->cp = capacity;
        this->head = nullptr;
        this->tail = nullptr;
    }
    ~LRUCache() {
        this->clearCache();
    }
    void set(int key, int value) override {
        if (this->mp.empty()) {
            Node *newNode = new Node(key, value);
            head = newNode;
            tail = newNode;
            this->mp[key] = newNode;
            count++;
            return;
        } else {
            if (this->mp.find(key) != this->mp.end()) {
                Node* selectedNode = this->mp[key];
                if (selectedNode == head) {
                    head->value = value;
                    return;
                }
                (selectedNode->prev)->next = selectedNode->next;
                selectedNode->prev = nullptr;
                selectedNode->next = head;
                head->prev = selectedNode;
                head = selectedNode;
                head->value = value;
            } else {
                Node *newNode = new Node(key, value);
                mp[key] = newNode;
                head->prev = newNode;
                newNode->next = head;
                head = newNode;
                if (count < this->cp) {
                    count++;
                } else {
                    this->mp.erase(tail->key);
                    Node* forDel = tail;
                    tail = forDel->prev;
                    delete forDel;
                }
                return;
            }
        }
    }
    int get(int key) override {
        int value = -1;
        if (!this->mp.empty() && this->mp.find(key) != this->mp.end()) {
            return this->mp[key]->value;
        }
        return value;
    }
};
int LRUCache::count = 0;

int main() {
   int n, capacity,i;
   cin >> n >> capacity;
   LRUCache l(capacity);
   for(i=0;i<n;i++) {
      string command;
      cin >> command;
      if(command == "get") {
         int key;
         cin >> key;
         cout << l.get(key) << endl;
      } 
      else if(command == "set") {
         int key, value;
         cin >> key >> value;
         l.set(key,value);
      }
   }
   return 0;
}
```
