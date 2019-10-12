~~~python
# 리스트 딥 카피
import copy

arr_org = [1, 2, 3, 4]
arr_cpy = copy.deepcopy(arr_org)

# 이차원 리스트
n = 5
arr = [[0] * n for _ in range(n)]

# 리스트 소팅 (원래 기준)
lst = ['conor', 'mcgregor', 'sexy', 'boy']
sorted_lst = sorted(lst)

lst.sort()

# 리스트 소팅 (리버스)
lst.sort(reverse=True)

# 리스트 소팅 (길이 기준대로)
lst.sort(key=len)

# 리스트 인덱스 알아보기
lst.index('mcgregor')

# 리스트 특정 원소 갯수 체크
lst.count('sexy')

# 리스트 특정 인덱스 삽입
lst.insert(1, 'cain')

# 리스트 특정 인덱스에 대해 삭제
del lst[1]

# 리스트 특정 값 삭제
lst.remove('boy')

# 딕셔너리 키, val, 아이템 순회
dic = {'a': 10, 'b': 9, 'c': 9}

for key in dic:
    print(key)

for val in dic.values():
    print(val)

for key, val in dic.items():
    print(key, val)

# 딕셔너리 삭제
del dic['a']

~~~