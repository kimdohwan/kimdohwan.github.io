---
layout: post
title:  "[Magic Method] 클래스와 함수에 들어가는 method의 차이와 용도 알아보기"
categories: Python
---
### 테스트 클래스 작성

```
class TestClass:
    '''
    이것은 docstring 입니다
    '''
    def __init__(self, name):
        self.name = name

    def method_function(self):
        '''
        함수안의 docstring
        '''
        print('this is method function')

    def __call__(self, x, y):
        return x + y

    def __getattr__(self, attribute):
        return f'{attribute} 는 없다 이놈아'


def common_function():
    print('this is common function')

a = TestClass('안녕')
b = common_function
```

### 클래스와 함수에 들어가는 method의 차이와 용도 알아보기

인스턴스의 메소드와 일반함수를 dir() 해본 결과

-   호출 시 () o  
    둘다 같은 attribute 가짐, 따라서 인스턴스의 메소드나 일반 함수나 같은 방식으로 동작한다는 것을 알 수 있다.
-   호출 없음 () x
    -   일반 함수만 가지는 attribute:  
        annotations, closure, code, default, dict, globals, kwdefault, module, name, qualname,
    -   인스턴스 메소드만 가지는 attribute:  
        func, self,

```
print(
    dir(TestClass),
    '\n',
    dir(a), # 인스턴스
    '\n',
    dir(a.method_function), # 인스턴스의 메소드(호출포함)
    '\n',
    dir(b), # 일반함수
)

'''
결과 값
['__call__', '__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattr__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'method_function'] 
 ['__call__', '__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattr__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'method_function', 'name'] 
 ['__call__', '__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__func__', '__ge__', '__get__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__self__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__'] 
 ['__annotations__', '__call__', '__class__', '__closure__', '__code__', '__defaults__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__get__', '__getattribute__', '__globals__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__kwdefaults__', '__le__', '__lt__', '__module__', '__name__', '__ne__', '__new__', '__qualname__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__']
'''

```