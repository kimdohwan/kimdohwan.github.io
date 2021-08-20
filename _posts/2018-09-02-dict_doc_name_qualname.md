---

title:  "[Magic Methods]__dict__, __doc__, __name__, __qualname__"
tag: Python
---
### 사용할 테스트 클래스 작성

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

### `__dict__, __doc__, __name__, __qualname__`

```
'''__dict__
A dictionary or other mapping object used 
to store an object’s (writable) attributes. <- 공식문서
Keep 메모 참고 할 것(설명 깃 주소)
__dict__ 는 단지 인스턴스의 'local' attr 을 보여줄 뿐이다.
하지만 dir() 은 더 확장시킨 전체적인 attr 을 알 수 있다.
'''
a.__dict__

'''결과값
{'name': '안녕'}'''

''' ___doc___
docstring 은 클래스나 함수 내부의 apostrophe(')에 담긴 
내용들을 볼 수 있다. 만약 docstring 이 없는 경우에는 
리턴값이 없거나 __getattr__ 이 실행된다.(정의된 경우)
'''
print(a.__doc__)
print(a.method_function.__doc__)
print(b.__doc__)

'''결과값
    이것은 docstring 입니다


        함수안의 docstring

None'''

''' __name__, __qualname__
정의된 이름과 qualified name 을 보여준다.
qualname 은 단순히 name 뿐만아니라 global 하게 알 수 있다
두 속성은 colon 과 같이 정의된 객체들에게 부여되어있다
변수라던가 인스턴스의 경우 두 속성을 가지고 있지 않다
'''
print(a.__name__, a.__qualname__)
print(TestClass.__name__, TestClass.__qualname__)

print(a.method_function.__name__, a.method_function.__qualname__, a.method_function.__class__)
print(b.__name__, b.__qualname__, b.__class__)

'''결과값
__name__ 는 없다 이놈아 __qualname__ 는 없다 이놈아
TestClass TestClass
method_function TestClass.method_function <class 'method'>
common_function common_function <class 'function'>'''

```