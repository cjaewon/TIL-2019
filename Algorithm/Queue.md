> 수정 날짜 (없음) / 작성 날짜 : (2020-05-20)

# Queue - 큐

## 정의

**큐**는 먼저 입력 된 데이터가 먼저 나가는 자료구조로
`선입선출 (First in First out)` 구조다.

![](https://images.velog.io/images/jwn4492/post/0ad568a3-e153-4391-996b-837bb260f139/image.png)

위 사진이랑 같은 구조이다.

생활속에서는 **마트 계산대**, **현금 인출기 기다리는 사람**들 이  있다.

## 용어 정리
- `Front()` / `head()` 데이터의 가장 앞부분
- `Rear()` / `tail()` 데이터의 가장 뒷부분
- `Enqueue()` / `put()` / `insert()` 큐에 자료를 삽입
- `Dequeue()` / `get()` / `delete()` 큐에 자료를 삭제 및 반환

## 구현  (python)
파이썬에서는 기본 모듈에 포함되어 있기 때문에
알고리즘 테스트에서는 직접 구현하기보다는 가져오는 게 훨씬 나을 것 같다.

```py
import queue

q = queue.Queue()

for i in range(0, 10):
	q.put(i)
    
for i in range(0, 5):
	print(q.get()) # 원소 제거 + 가져오기

print(q.qsize()) # 큐 크기
```
## 활용
대표적으로 BFS 에서 쓰인다.
