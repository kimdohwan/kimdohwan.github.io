---
layout: post
title:  "1. Linked List - Doubly Linked List"
categories: DataStructure 
---
-   previout node 와 next node 가 존재(서로 연결, chaining)
-   탐색/참조가 필요없다면 singly linked list 사용하지만 그 외에는 dobouly 가 good
-   장점:
    -   singly linked list 보다 탐색/참조에 유연(앞,뒤 에서 모두 검색 시작 가능)
-   단점:
    -   prev 로 인해 singliy 보다 메모리 조오금 더 먹는 것?

```
 이 코드의 경우(책에나온), head 와 tail 을 빈 Node 로 셋팅 해두었음 
# head 와 tail 이 data 를 입력받아 size 에 포함되는 코드도 찾아볼 것
# search 같은 경우는 singly linked list 와 비슷해서 생략된듯 하니 찾아볼 것
class DList:
    class Node:
        def __init__(self, item, prev, link):
            self.item = item
            self.prev = prev
            self.next = link


    def __init__(self):
        self.head = self.Node(None, None, None)
        self.tail = self.Node(None, self.head, None)
        self.head.next = self.tail
        self.size = 0

    def size(self):
        return self.size

    def is_empty(self):
        return self.size == 0

    def insert_before(self, p, item): # (a - b) Node b 앞에 insert
        t = p.prev # b 앞의 Node(a)
        n = self.Node(item, t, p) # prev 에 b.prev(a), next 에 b
        p.prev = n # b.prev 에 새로운 Node insert
        t.next = n # 원래 b 앞의 Node 였던 a 의 next 에 새로운 Node insert
        self.size += 1

    def insert_after(self, p, item): # (a - b) Node a 다음에 insert
        t = p.next # a 다음 Node(b)
        n = self.Node(item, p, t) # prev 에 a, next 에 a.next(b)
        t.prev = n # b.prev 에 새로운 Node insert
        p.next = n # a.next 에 새로운 Node insert
        self.size += 1

    def delete(self, x):
        # x 의 prev 와 next Node 를 연결
        f = x.prev 
        r = x.next
        f.next = r
        r.prev = f
        self.size -= 1
        return x.item

    def print_list(self):
        if self.is_empty():
            print('Empty list')
        else:
            p = self.head.next # 첫 노드가 head.next. 작성자에따라 다른듯
            while p != self.tail:
                if p.next != self.tail:
                    print(p.item, '<=>', end=' ')
                else:
                    print(p.item)
                p = p.next
                ```

```