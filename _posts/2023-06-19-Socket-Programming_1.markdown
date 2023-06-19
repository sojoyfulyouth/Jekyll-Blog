---
layout: post
title: "Socket Programming - Calculator"
date: 2023-06-19 14:03:36
categories: Computer-Network
---

<!-- prettier-ignore-start -->

#### Calculator: client가 문자열 형태로 수식을 입력하면 결과값을 반환하는 코드
##### 서버 코드

<!-- prettier-ignore-end -->

```python
import socket

LOCALHOST = "127.0.0.1"
PORT = 8080

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind((LOCALHOST, PORT))
server.listen()
print("Server started")
print("Waiting for client request..")

clientConnection, clientAddress = server.accept()
print("Connected client: ", clientAddress)
msg = ''

while True:
    data = clientConnection.recv(1024)
    msg = data.decode()
    if msg == 'Over':
        print("Connection is Over")
        break
    print("Equation is received")
    result = 0
    operation_list = msg.split()
    oprnd1 = operation_list[0]
    operation = operation_list[1]
    oprnd2 = operation_list[2]

    num1 = int(oprnd1)
    num2 = int(oprnd2)

    if operation == "+":
        result = num1+num2
    elif operation == "-":
        result = num1-num2
    elif operation == "x":
        result = num1*num2
    elif operation == "/":
        result = num1/num2

    print("Send the result to client")
    output = str(result)
    clientConnection.send(output.encode())
clientConnection.close()
```

<!-- prettier-ignore-start -->

##### 클라이언트 코드

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
