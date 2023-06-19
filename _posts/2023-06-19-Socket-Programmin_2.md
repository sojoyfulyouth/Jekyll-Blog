---
layout: post
title: "Socket Programming - String Transfer"
date: 2023-06-19
categories: ComputerNetwork
---

<!-- prettier-ignore-start -->

#### 1. Calculator: client가 문자열 형태로 수식을 입력하면 결과값을 반환하는 코드
##### 서버 코드

<!-- prettier-ignore-end -->

```python
import socket

SERVER = "127.0.0.1"
PORT = 8080

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

client.connect((SERVER, PORT))
while True:
    print("Example : 4 + 5")
    inp = input("Enter the operation in the form oprenad oparator oprenad : ")
    if inp == "Over":
        break
    client.send(inp.encode())

    answer = client.recv(1024)
    print("Answer is "+answer.decode())
    print("Type 'Over' to terminate")
client.close()
```
