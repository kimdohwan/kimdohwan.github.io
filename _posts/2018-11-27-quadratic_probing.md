---

title:  "5. Hash Table Quadratic Probing in Open Addressing"
tag: DataStructure 
---
이차조사 방식  
collision 발생 시 +x^2 으로 제곱수를 증가시킴

-   문제점  
    second clustering 발생(2차 군집화)  
    비어있는 공간이 있음에도 건너뛰어 저장실패

```
class QuadProbing:
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
        j = 0
        while True:
            if self.a[i] == None:
                self.a[i] = key
                self.d[i] = data
                self.N += 1
                return print(f'저장완료 {data}')
            if self.a[i] == key:
                self.d[i] = data
                return print(f'수정완료 {data}')
            j += 1
            i = (initial_position + j * j) % self.M

# -----------------얘는 잘못된 코드(책)-----------
            if self.N > self.M: # 저장항목 수가 테이블 크기를 넘으면 저장실패
                print(f'저장한도 초과 {key}, {data}')
                # 뭐지? N 은 M 보다 클 수가 없다, list index 에러나기때문에
                # 그래서 '>' 이게 아닌 '=' 이게 들어가야한다.
                # 라고 생각해서 = 을 붙였더니 data 를 수정하는 경우,
                # 루프를 돌다가 여기서 막혀서 저장 실패가 된다 
                # 따라서 이 구문은 맞지 않은 구문인듯
                # 애초에 self.N 은 self.M 을 초과할 수 없다 
                # 왜냐하면 a[i] 의 i에는 0~11 까지의 수밖에 들어갈 수 없고
                # 모든 table 에 value 가 채워지는 순간 self.N += 1 이 불가능
                # 그러므로 저장한도 초과를 위해서는 다른 제약조건이 필요하다
                break # linear probing 처럼 본래자리로 언제 돌아올지 알수없어서 self.N 사용
# -----------------------------------------------
            # 내가 추가함 
            if j == 1000: # 루프 제한(저장한도초과의 의미)
                print(f'돌만큼 돌았잔아, {data}, 저장실패')
                break

    def get(self, key):
        initial_position = self.hash(key)
        i = initial_position
        j = 1
        while self.a[i] != None:
            if self.a[i] == key:
                return self.d[i]
            i = (initial_position + j * j) % self.M
            j += 1
            # quadratic probing 은 linear probing 처럼 인덱스가 제자리에 
            # 돌아오는 방식으로 검색루프를 종료시킬 수 없기 때문에 따로 루프제어를
            # 해줄 수 있는 구문이 필요하다. 이 코드에서는 생략 됨
        return None

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