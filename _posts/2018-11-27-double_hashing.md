---
layout: post
title:  "5. Hash Table - Double Hashing in Open Addressing"
categories: DataStructure 
---
두개의 hash function 을 사용  
점프 시퀀스가 일정치 않아서 군집화 현상 발생하지 않음  
table size 와 2차 hash function d(key) 값이  
서로소 관계일 때 좋은 성능을 보이는데  
table size 가 소수이면 자연히 만족된다고 한다. 뭔소린지는 모르겠다  
아니다 생각해보니까 소수의 의미를 대충은 알겠다  
서로 공약수를 갖지 않는 수 2개가 key 값을 나누고  
그 나머지 값들로 연산을 하게된다  
예를 들어  
key = 10, table size = 13, d(key) = 7-(key%7)  
이라면 1차 점프 값은 (10 + 1 \* 4) % 13 = 1  
2차 점프 값은 (1 + 2 \* 4) % 13 = 9, 3차는 (9 + 3 \* 4) % 13 = 8  
이런식으로 자기 멋대로 설정된다  
음 그런데 왜 소수여야하는지는 잘모르겠다  
아무튼 이렇게 참 재미있는 친구이다

```

class DoubleHashing:
    def __init__(self, size):
        self.M = size
        self.a = [None for i in range(size + 1)]
        self.d = [None for i in range(size + 1)]
        self.N = 0

    def hash(self, key):
        return key % self.M

    def put(self, key, data):
        initial_position = self.hash(key)
        i = initial_position
        d = 7 - (key % 7) # 이 부분이 2차 해시 함수, 0이 나오면 안됨
        j = 0
        while True:
            if self.a[i] == None:
                self.a[i] = key
                self.d[i] = data
                self.N += 1
                return 
            if self.a[i] == key:
                self.d[i] = data
                return 
            j += 1 # j를 통해 2차 해시 함수를 증가시킨다
            i = (initial_position + j * d) % self.M
#             print(i) # 얘를 통해서 jump sequence 를 확인 가능(뒤죽박죽이다)
            if j == 1000:
                print('1000번 점프! 공간이없는듯!', key, data)
                break

    def get(self, key):
        initial_position = self.hash(key)
        i = initial_position
        d = 7 - (key % 7)
        j = 0
        while self.a[i] != None:
            if self.a[i] == key:
                return self.d[i]
            j += 1
            i = (initial_position + j * d) % self.M
            if j == 1000:
                break
        return print(f'{key} 값이 존재하지 않습니다')

    def print_table(self):
        for i in range(self.M):
            print(i, self.a[i], self.d[i])

```