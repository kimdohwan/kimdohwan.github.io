---

title:  "3. Tree - Bynary Heap"
tag: DataStructure 
---
```
class BHeap:
    def __init__(self, a):
        self.a = a
        self.N = len(a) - 1 # a[0] = None, it needs '-1'

    def create_heap(self):
        for i in range(self.N // 2, 0, -1):
            print(i, 'downheap 실행')
            self.downheap(i) # downheap(), 인자로 i 전달(i부터 내려가거라)
# 첫 self.n//2 는 최하단에서 두번째 층의 마지막 노드를 의미
# (7개의 node가 있다면 
# level 1 = 최상위노드 1개  <--두번째(i=1) self.n//2 노드
# level 2 = level 1에서 뻗어나온 2개  <--첫번째(i=2)로 self.n//2 노드 시작
# level 3 = level 2에서 뻗어나온 4개)

    def insert(self, key_value):
        self.N += 1
        print('append 전 tree:', a)
        self.a.append(key_value) # a 의 뒷 쪽에 key_value 삽입
        print('append 후 tree:', a, self.N)
        self.upheap(self.N) # upheap(), 인자로 self.N 을 전달(n부터 올라가거라)

    def delete_min(self):
        if self.N == 0:
            print('self.N == 0')
            return None
        minimum = self.a[1]
        self.a[1], self.a[-1] = self.a[-1], self.a[1]

        del self.a[-1]
        self.N -= 1
        self.downheap(1) # downheap(), 인자로 1 전달(1부터 내려가거라)
        return minimum

    # i부터 올라가거라(값 비교 후 교체 한 후 올라가며 진행)
    def downheap(self, i): 
        while 2 * i <= self.N:
            k = 2 * i # k 는 i 의 child node 를 의미(2를 곱한 위치,인덱스)
            if k < self.N and self.a[k][0] > self.a[k + 1][0]:
                k += 1
                print('k + 1', k)
            if self.a[i][0] < self.a[k][0]:
                print('break')
                break
            self.a[i], self.a[k] = self.a[k], self.a[i]
            i = k
            print('i = k', k)

    def upheap(self, j):
        while j > 1 and self.a[j // 2][0] > self.a[j][0]:
            print('j:', j, self.a[j // 2][0], self.a[j][0])
            self.a[j], self.a[j // 2] = self.a[j // 2], self.a[j]
            j = j // 2
            print(a, '\n')

    def print_heap(self):
        for i in range(1, self.N + 1):
            print('[%2d' % self.a[i][0], self.a[i][1], ']', end='')
        print(self.N)



# 책에있는 코드(위)에서 heap item 을 [90, apple] 과 같은 방식이 아닌
# 숫자만(90) 넣은 코드로 다시 작성해보았다 
class Binary_Heap:
    def __init__(self, item):
        self.heap_array = item
        self.item_num = len(item) - 1

    def create_heap(self):
        for i in range(self.item_num // 2, 0 , -1):
            self.downheap(i)

    def delete_min(self):
        self.item_num -= 1
        deleted_min = self.heap_array[1]
        self.heap_array[1], self.heap_array[-1] = self.heap_array[-1], self.heap_array[1]
        del self.heap_array[-1]
        self.downheap(1)
        return deleted_min

    def downheap(self, i):
        while 2 * i <= self.item_num:
            k = 2 * i
            if k < self.item_num and self.heap_array[k] > self.heap_array[k + 1]:
                print(f'downheap if \n {k} < {self.item_num} and {self.heap_array[k]} > {self.heap_array[k+1]}')
                k += 1
            if self.heap_array[i] < self.heap_array[k]:
                print(f'downheap if(break) {self.heap_array[i]} < {self.heap_array[k]}')
                break
            self.heap_array[i], self.heap_array[k] = self.heap_array[k], self.heap_array[i]
            i = k

    def print_heap(self):
        print(self.heap_array[1:]) # heap 만 (앞 None 제외)

    def insert(self, value): # 새로운 트리 item 추가
        self.item_num += 1 # 1. item 갯수를 추가시켜주고
        self.heap_array.append(value) # 2. item 을 append 해주고
        self.upheap(self.item_num) # upheap 을 이용해서 트리 재편

    def upheap(self, heap_num):
        while heap_num > 1 and self.heap_array[heap_num // 2] > self.heap_array[heap_num]:
            self.print_heap()
            print(f'upheap while loop \n{self.heap_array[heap_num // 2]} > {self.heap_array[heap_num]}')
            self.heap_array[heap_num // 2], self.heap_array[heap_num] = self.heap_array[heap_num], self.heap_array[heap_num // 2]
            heap_num = heap_num // 2
```