> 수정 날짜 (없음) / 작성 날짜 : (2020-05-20)

# Stack - 스택

## 정의

**스택**는 먼저 입력 된 데이터가 나중에 나가는 자료구조로
`후입선출 (Last in First out)` 구조다.

즉 스택이란 반대의 개념이다

![](https://images.velog.io/images/jwn4492/post/4de551b8-0a1f-4860-b18a-ae54e2696595/image.png)

위 사진이랑 같은 구조이다.

생활 속에서는 **뷔페 접시**가 있다.

> 스택을 책으로 비교하기도 하는데
책을 세로로 쌓다 보면 밑에 있는 책을 보고 싶어도 뺄 수 는 없기 때문이다.

## 용어 정리
- `pop()` 스택에 자료 삭제 및 반환
- `push()` 스택에 자료 삽입
- `top()` 스택의 가장 위 데이터 반환


## 구현  (python)
파이썬에서는 리스트를 이용해서 간단하게 스택을 구현 할 수 있다.
```py
data = []

for i in range(0, 5):
	data.append(1)
for i in range(0, 5):
	print(data.pop())
   
print(len(data))
```
## 활용
대표적으로 DFS 에서 사용 할 수도 있다.
