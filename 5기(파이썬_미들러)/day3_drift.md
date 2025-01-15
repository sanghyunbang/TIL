input_data = """5 5
1 3 10 20 30
1 10
20 60
3 30
2 15
4 8"""

data = input_data.strip().split("\n")

# 입력 처리
num_dot, num_lines = map(int, data[0].split())
dot_location = list(map(int, data[1].split()))

list_lines =[]

for i in range(num_lines):
  int_list = list(map(int, data[2+i].split()))
  list_lines.append(int_list)


print(num_dot)
print(num_lines)
print(dot_location)
print(list_lines)

for i in range(num_lines):
  start_index=0
  end_index= num_dot-1
  while list_lines[i][0]> dot_location[start_index]:   # 시작부터
    start_index += 1
    if start_index > num_dot:
      break

  while list_lines[i][1]< dot_location[end_index]:    # 끝자리부터
    end_index -= 1
    if start_index <0:
      break

  print(end_index-start_index+1)



### 문제점 : break가 for문 자체를 멈춰버릴 수 있음
---> break 사용은 항상 주의
