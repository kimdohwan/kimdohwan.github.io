---

title:  "5. Hash Table Random Probing in Open Addressing"
tag: DataStructure 
---
점프 시퀀스를 무작위화  
파이선의 딕셔너리가 이 방식을 취함  
문제점

-   3차 군집화(tertiary clusting) 2차 군집화와 유사

```
import random

class RandProbing:

    def __init__(self, size):
        self.M = size
        self.a = [None] * size
        self.d = [None] * size
        self.N = 0 # 저장된 항목 수 

    def hash(self, key):
        return key % self.M

    def put(self, key, data):
        initial_position = self.hash(key)
        i = initial_position
        o = 0
        random.seed(1)
        while True:
            if self.a[i] == None:
                self.a[i] = key
                self.d[i] = data
                self.N += 1
                return print(f'저장완료 {data}')
            if self.a[i] == key:
                self.d[i] = data
                return print(f'수정완료 {data}')
            j = random.randint(1, 99)
            i = (initial_position + j) % self.M
            o += 1
            if self.N > self.M: # 무쓸모 
                break
            if o == 1000:
                print('1000번 초과, 저장공간 부족', data)
                break

    def get(self, key):
        initial_position = self.hash(key)
        i = initial_position
        j = 1
        random.seed(1)
        o = 0
        while self.a[i] != None:
            if self.a[i] == key:
                return self.d[i]
            i = (initial_position + random.randint(1, 99)) % self.M
            o += 1
            if o == 1000:
                print(f'존재하지 않는 key: {key}')
                break

    def print_table(self):
        for i in range(self.M): # 인덱스 출력 
            print(f'{i} - {self.a[i]}: {self.d[i]}')
#         for i in range(self.M): # key 출력
#             print('{:4}'.format(str(self.a[i])), ' ', end='')
#         for i in range(self.M):
#             print(f'{self.d[i]}', ' ', end='')
```