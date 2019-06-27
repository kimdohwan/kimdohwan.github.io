---
layout: post
title:  "6. Sort - Bubble, Selection, Insertion"
categories: DataStructure 
---
### selection sort, bubble sort

두 정렬의 차이점은 selection 의 존재 유무도 있지만 사실상 값 교환을 언제 하느냐에 따라서 sort 의 정체성이 나뉜다

```
def selection_sort(a):
    for i in range(0, len(a) - 1):
        minimum = i
        for j in range(i, len(a)):
            if a[minimum] > a[j]:
                minimum = j
        a[i], a[minimum] = a[minimum], a[i]
    print(a, 'selection')


# selection sort 와 비교해서 어떻게 다른지 볼 것 
# bubble sort 가 인접한 두 값 비교라는 의미가 중요하다면 
# 이 코드는  bubble sort 라고 할 수 있는건가?
def my_bubble_sort(a):
    for i in range(len(a) - 1):
        for j in range(i, len(a)):
            if a[i] > a[j]:
                a[i], a[j] = a[j], a[i]
    print(a, 'my bubble')
```

### 코드 작동 정리

selection sort 의 minimum 은 2번째 for loop 밖에서 값 교환을 해주기위한 수단일 뿐이다

두 정렬의 코드를 작성할 때 가장 관건은 for loop 에 들어가는 range() 의 index 설정이라고 할 수 있다

첫번째 for loop 는 list 의 첫번쨰 항목부터 '마지막 항목 한개 전'까지 설정해준다. 예를 들어 len(a) == 10, a\[8\]까지 loop 를 돌고나면 a\[9\](마지막 항목)은 저절로 가장 큰 값으로 정렬되기 떄문이다

두번째 for loop 는 list 에서 정렬된 부분을 제외한 나머지 인덱스들이다. 정렬이 된 항목들은 첫번째 for loop 에 의해 앞쪽으로 배치된다. 예를 들어 i = 3 까지 첫번쨰 for loop 가 진행된 경우,이는 a\[0\], a\[1\], a\[2\], a\[3\] 은 정렬이 완료 됫음을 의미, 따라서 정렬이 되야하는 나머지 항목의 index 를 for loop 에 설정해준다.위의 예대로 라면 다음에 실행될 두번째 for loop는 a\[4\] ~ a\[9\] 가 될 것이다.첫번째 for loop 는 len(a) - 1 이지만 두번째 for loop는 모든 값을 비교해야하기 떄문에 마지막 값을 제외할수 없다. 따라서 range(i, len(a)) 와 같이 된다.

### bubble index

```

'''
--------------bubble index 원리---------------------
최대값을 한쪽으로 몰아가는 형식
첫번째 for loop 를 '1st', 두번쨰를 '2nd' 라고 표기하겠다

일단 for loop 는 본질적으로 몇번 반복되냐가 핵심.
1st for loop 에서는 a(len) == 10 인 경우, 
크기비교를 실행한 숫자를 제위치잡기위해 9번의 loop 가 필요 

2nd for loop 는 정렬이 되지 않은 항목을 연산해야하므로 
정렬이 되는 항목들을 하나씩 제외해가며 실행하므로 
range(len(a) - 1 - i) 번이 필요하다 
'''

def bubble_sort(a):
    for i in range(len(a) - 1):
        for j in range(len(a)- 1 - i): # 이렇게 range 범위를 설정한경우 뒤에서부터 정렬된다
            if a[j] > a[j + 1]:
                a[j], a[j + 1] = a[j + 1], a[j]
    print(a, '버블 정석')
```

### insertion sort

범위를 1, 2, 3, 4, 5..., 10 순서대로 늘려가며  
범위안에서 크기비교와 순서 정렬을 이룬다  
이 때 범위가 한 개씩 커져가는 과정에서  
새로들어오는 값(범위의 마지막 인덱스)이  
제자리를 찾아가(마지막 값부터 순차적으로 크기 비교 후)삽입되게 된다

버블과 유사해 보이는데 차이점은?  
버블의 경우, 최대값을 한쪽끝으로 몰아가며 정렬  
삽입의 경우, 범위를 list 크기로 증가시키며 범위안에서 정렬  
\*\*\* 중요  
또한 insertion 의 경우 범위내에서 값이 제자리를 찾아 간다면  
그 이후에 값 할당 작업은 이루어지지 않게 된다(if 문 밑의 교환작업)  
따라서 bubble 이나 insertion 에 비해 값 할당 작업이  
더 적게 이루어질 수 있어서 정렬이 잘되어있다면  
더 빠른속도를 보일 수도 있다.

```

def insertion_sort(a):
    for i in range(len(a)):
        for j in range(i, 0, -1):
            if a[j - 1] > a[j]:
                a[j - 1], a[j] = a[j], a[j - 1]
    print(a, 'insertion')
```