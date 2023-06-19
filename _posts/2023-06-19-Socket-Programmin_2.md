---
layout: post
title: "Socket Programming - String Transfer"
date: 2023-06-19
categories: ComputerNetwork
---

<!-- prettier-ignore-start -->

#### 2. String Transfer: client가 입력한 문자열을 서버에서 ASCII 코드로 반환하는 코드
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

    ascii_values = [ord(character) for character in msg]
    ascii_str = str(ascii_values)

    print("Send the result to client")
    clientConnection.send(ascii_str.encode())

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
    inp = input("Enter the string : ")
    if inp == "Over":
        break
    client.send(inp.encode())

    answer = client.recv(1024)
    print("Answer is "+answer.decode())
    print("Type 'Over' to terminate")

client.close()
```
