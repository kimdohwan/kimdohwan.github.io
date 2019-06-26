---
layout: post
title:  "generator 동작"
categories: Python
---

```
# generator 함수 생성
def square_numbers(nums):
    for i in nums:
        yield i * i

# genarator 함수
my_generator = square_numbers([1, 2, 3])

# genarator라는 오브젝트가 리턴됨을 볼 수 있다.
print(my_generator)

# generator는 자신이 리턴할 모든 값을 메모리에 저장하지 않기에
# 리스트로 보이지 않는다. 한번 호출할 때 마다 하나의 값만을 yield(생산, 전달) 한다

'''
print로 인해 리턴되는 객체
<generator object square_numbers at 0x7effac93da98>
'''


'''만약 위 코드에서 next() 로 generator 값 1, 4, 9 를 
모두 출력했다면 for문의 my_generator 는 가진 값이 없어서 
아무것도 출력하지 않게 된다'''
for num in my_generator:
    print(num)
'''
print되는 num
1 4 9
'''
```

```
'''
list comprehension 과 같은 방식으로 generator 생성 가능
'''
my_nums = (x*x for x in [1, 2, 3])
print(my_nums)

for num in my_nums:
    print(num)
# list(my_nums) 를 해주면 한번에 데이터를 볼 수 있다
```

```
from __future__ import division
import os
import psutil
import random
import time

names = ['최용호', '지길정', '진영욱', '김세훈', '오세훈', '김민우']
majors = ['컴퓨터 공학', '국문학', '영문학', '수학', '정치']

process = psutil.Process(os.getpid())
memory_before = process.memory_info().rss / 1024 / 1024

def people_list(num_people):
    result = []
    for i in range(num_people):
        person = {
            'id': i,
            'name': random.choice(names),
            'major': random.choice(majors),
        }
        result.append(person)
    return result

def people_generator(num_people):
    for i in range(num_people):
        person = {
            'id': i,
            'name': random.choice(names),
            'major': random.choice(major),
        }
        yield person
        

'''process_time() 은 현재 코드가 실행되는데 걸리는 시간이다 
time.clock 과 같은 역할이지만 python3 에서는 이 방법을 권장한다'''
t1 = time.process_time()
people = people_list(100000)
t2 = time.process_time()
memory_after = process.memory_info().rss / 1024 / 1024
total_time = t2 - t1

print(memory_before)
print(memory_after)
print(total_time)

''' 
print 결과
45.62109375
72.8203125
0.2674934950000001
'''

t1 = time.process_time()
people = people_generator(100000)
t2 = time.process_time()
memory_after = process.memory_info().rss / 1024 / 1024
total_time = t2 - t1

print(memory_before)
print(memory_after)
print(total_time)

'''
print 결과
45.62109375
47.30078125
0.011788205999999968
'''

```