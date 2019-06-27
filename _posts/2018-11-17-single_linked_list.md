---
layout: post
title:  "1. Linked List - Singly Linked List"
categories: DataStructure 
---
singly linked list

-   한 방향으로 노드가 노드를 가리키는 구조
-   노드는 data 와 link 를 가진다
-   보통 que(fifo) 를 구현 할 때 이런 방법을 많이 쓴다
-   첫번째 데이터 추가/삭제 시에 O(1), 데이터 검색은 O(n)
-   단점: 검색할 때 무조건 앞쪽부터 노드를 쭉 거쳐야 하므로  
    index 를 사용하는 array 보다 비효율적
-   장점: 데이터 수정 시 이동이 필요없다  
    (link 만 바꿔주면 ok, 배열의 경우 데이터 삽입, 삭제 시 모든 데이터가 한칸씩 이동해야함)

```
class SList:
    # linked list 안쪽에 Node 를 정의해주었다.
    # 경우에 따라 다르겠지만 이런 방식도 잘 익혀두자.
    class Node:
        def __init__(self, item, link):
            self.item = item
            self.next = link

    def __init__(self):
        self.head = None
        self.size = 0

    def size(self):
        return self.size

    def is_empty(self):
        return self.size == 0

    # list 의 가장 앞에 노드 생성
    def insert_front(self, item):
        if self.is_empty():
            # 첫 노드가 생성, next 는 None 이 된다.
            self.head = self.Node(item, None)
        else:
            # head Node 를 새로 생성 및 설정, 
            # previous head 는 새로운 head 의 next 로 설정
            self.head = self.Node(item, self.head)
        self.size += 1

    def insert_after(self, item, p): # p 는 Node object
        # p.next 에 생성될 Node 를 연결시켜준 뒤 
        # 생성될 Node 는 p.next 를 link(next)
        p.next = SList.Node(item, p.next)
        self.size += 1

    def delete_front(self):
        if self.is_empty():
            raise EmptyError('Underflow')
        else:
            self.head = self.head.next
            self.size -= 1

    def delete_after(self, p):
        if self.is_empty():
            raise EmptyError('Underflow')
        t = p.next # a -> b -> c 중에서 b.next = c
        p.next = t.next # b.next = c.next -> Node c 가 사라지고 다음 노드를 연결시킨다
        self.size -= 1

    def search(self, target):
        p = self.head # 첫번째 노드부터 검색 시작 
        for k in range(self.size): # list size 만큼 loop
            if target == p.item: # target 과 node item 일치 검사
#                 return k # 몇 번째 노드인지 숫자로 리턴
                return p # 노드 객체 자체를 리턴
            p = p.next # 다음 노드로 변경
        return None

    def print_list(self):
        p = self.head
        while p:
            if p.next != None:
                print(p.item, '->', end=' ')
            else:
                print(p.item)
            p = p.next


class EmptyError(Exception):
    pass
    ```

```