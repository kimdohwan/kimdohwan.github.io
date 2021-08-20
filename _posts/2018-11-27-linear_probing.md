---

title:  "5. Hash Table - Linear Probing in Open Addressing"
tag: DataStructure 
---
Linear Probing in Open Addressing  
개방주소방식의 선형조사  
colision 발생 시 인덱스 +1 방식으로 해결

-   문제점  
    prime clustering(1차 군집화)  
    '1차'는 다른 해쉬방법에서 벌어지는 clustering 현상을 구별하기위한 단어일뿐이다

```
class LinearProbing:
    def __init__(self, size):
        self.M = size # hash value 를 구하기 위한 값으로 사용(hash table 의 크기)
        self.a = [None] * size # key 를 저장하는 hash table
        self.d = [None] * size # data 를 저장하는 hash table

    def hash(self, key): # hash function
        return key % self.M # 나머지 값이 hash value(hash table index)

    def put(self, key, data):
        initial_position = self.hash(key) # hash value
        i = initial_position
        j = 0
        while True:
            if self.a[i] == None: # 해당 위치가 비어있다면 저장
                self.a[i] = key
                self.d[i] = data
                return '저장 완료'
            if self.a[i] == key: # 이미 key 존재하므로 data 만 수정/갱신
                self.d[i] = data
                return '수정/갱신 완료(data, key is aleady existed)'
            # self.a[i] 에 비어있지않고 다른 key 가 이미 존재하는 경우
            j += 1 # 순차적으로 빈 공간을 탐색하기위해 인덱스 증가
            i = (initial_position + j) % self.M # hash value(+1)
            if i == initial_position: # 한바퀴 돌아서 제자리로 돌아올 경우(저장 실패)
                break

    # 함수에서는 return 이 나올경우 어떤 스코프에 해당하던(들여쓰기가 몇번 되었건간에)
    # return 으로 인해 함수가 즉시 종료된다 

    def get(self, key):
        initial_position = self.hash(key)
        i = initial_position
        j = 1 # 위에서는 j를 0 으로 해놓고 여기서는 1로 시작하네. 변태인가?
        while self.a[i] != None:
            if self.a[i] == key:
                return self.d[i]
            i = (initial_position + j) % self.M
            j += 1 # put 은 0, get 은 1부터 시작이라 j += 1 코드 순서가 다름
            if i == initial_position: # 한바퀴 돌아서 제자리로 돌아옴
                return None # 존재하지 않는 key 를 검색한 경우
        return None # 존재하지 않는 key 검색(while 문의 None 은 한바퀴 돌은 경우)

    def print_table(self):
        for i in range(self.M): # 인덱스 출력 
            print('{:4}'.format(str(i)), ' ', end='')
        print('')
        for i in range(self.M): # key 출력
            print('{:4}'.format(str(self.a[i])), ' ', end='')
        print('')
        for i in range(self.M):
            print(f'{self.d[i]}', ' ', end='')
```