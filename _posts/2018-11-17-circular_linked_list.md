---

title:  "1. Linked List - Circular Linked List"
tag: DataStructure 
---
-   원형으로 순환하는 구조
-   singly linked list 에서 head 와 tail 을 이어줌
-   장점:
    -   head 포인터 변수가 필요없다(tail 만 있으면 된다. head=tail.next)
    -   하나의 Node 에서 모든 Node 를 접근 가능
-   단점:
    -   노드 검색 시 결과가 없다면 무한 loop(밑 코드는 range(size)로 작성)

```

# 책에 나온 insert, delete 는 LIFO(후입선출) 방식이다.
# 인터넷에 보니 특정 노드 삭제도 가능할 수 있더라.
# 중요한 것은 link 부분이고 자료 insert,delete 방식은 의도에 따라 다르다.
class CList:
    class _Node:
        def __init__(self, item, link):
            self.item = item
            self.next = link


    def __init__(self):
        self.last = None
        self.size = 0

    def no_items(self):
        return self.size

    def is_empty(self):
        return self.size == 0

    def insert(self, item):
        n = self._Node(item, None) # 생성 Node
        if self.is_empty(): 
            n.next = n # 순환구조이므로 자신의 next 를 자신으로 설정
            self.last = n # self.last 로 설정
        else:
            n.next = self.last.next # last.next Node 를 생성 Node의 next 로 설정
            self.last.next = n # last.next 에 생성 Node 설정
        self.size += 1

    def first(self): # 첫번쨰 Node 리턴해주는 함수
        if self.is_empty():
            raise EmptyError('Underflow')
        f = self.last.next # last 의 next 가 첫번째 Node
        return f.item

    def delete(self): # last 의 link Node 를 삭제하게된다(first()에 해당)
        if self.is_empty():
            raise(EmptyError)
        x = self.last.next # 삭제할 Node(last.next)
        if self.size == 1:
            self.last = None # 마지막 Node 일 경우 self.last 해제 필요
        else:
            self.last.next = x.next # link 를 변경함으로써 Node 삭제
        self.size -= 1
        return x.item

    def print_list(self):
        if self.is_empty():
            print('Empty list')
        else:
            f = self.last.next
            p = f
            while p.next != f:
                print(p.item, '->', end=' ')
                p = p.next
            print(p.item)

    # 내가 search 작성(책에 없음)        
    def search(self, item):
        val = self.last.next # head 객체
        for i in range(self.size): # Node 갯수만큼 loop
            if item == val.item:
                return val
            val = val.next
        return f"Can't find {item}"


class EmptyError(Exception):
    pass
    ```

```