---
layout: post
title:  "[Magic Method] __eq__, __ne__, __ge__, __gt__, __le__, __lt__"
categories: Python
---


### operator(연산자) 기능

"rich comparison" method 라고 한다

```
c = 1
print(
    c.__eq__(2),
    c.__ne__(2),
    c.__ge__(2),
    c.__gt__(2),
    c.__le__(2),
    c.__lt__(2),
)

# eq 를 customizing 해보았다 
# 절대 건들진 말자.
# other 이 부호 뒤에오는 None 을 받아서 처리한다
class Eq:
    def __eq__(self, other):
        if self is other:
            return print(f'{other} 레퍼런스가 같다')
        else: 
            return print(f'{other} 레퍼런스가 다르다')

e = Eq()
e == None

'''결과값
False True False False True True
None 레퍼런스가 다르다'''

```