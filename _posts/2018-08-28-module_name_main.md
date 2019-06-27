---
layout: post
title:  "[Magic Method] __module__, __name__, __main__"
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

### `__module__, __name__, __main__`

`__module__` 해당 객체가 속한 스크립트의 module name(파일명)을 보여준다.  
이 때 스크립트는 당연히 .py(파이썬 파일).  
단 해당 스크립트가 현재 "실행중"일 경우 module name 은 `"__main__"`이 된다.

`if __name__ = "__main__":` 을 자주 보게되는데  
이 경우 실행중인 스크립트의 module name 은 `__main__` 이 되므로  
해당 스크립트가 import 되어 사용되는지 여부를 알기 위해서  
또는  
프로그램의 시작점일 때만 특정 코드를 실행하기위해 사용된다.

```
print(
    TestClass.__module__,
    a.__module__, 
    a.method_function.__module__,
    b.__module__
)

import random

# a는 __main__.TestClass random 은 Random.choice
print(a, '\n', random.choice)
# module 과 name 을 헷갈리말자 
# 함수나 클래스에 사용하는 __name__과 스크립트 실행여부를 알기위한 용도의 __name__은 
# 같은 원리이지만 용도가 다른느낌이니까 헷갈리지말자
print(random.choice.__module__, random.choice.__name__)

# 현재 실행중이므로 __main__ 리턴)
print(__name__)

'''결과값
__main__ __main__ __main__ __main__
<__main__.TestClass object at 0x7f553564eef0> 
 <bound method Random.choice of <random.Random object at 0x55eed7cfa668>>
random choice
__main__'''

```