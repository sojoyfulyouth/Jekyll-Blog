---
layout: post
title: "Socket Programming - File Modifier"
date: 2023-06-19
categories: Computer-Network
---

<!-- prettier-ignore-start -->

#### File Modifier: 사용자가 파일의 이름과 read/write 여부를 서버에게 전달하면 file read 내용 반환하는 코드
#### - write를 선택한 경우 사용자는 추가할 content를 함께 입력함
#### - 단, 파일은 코드 파일과 같은 폴더 내에 존재함
##### 서버 코드

<!-- prettier-ignore-end -->

```python
import socket
from os.path import exists
import sys

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

fullname = clientConnection.recv(1024)
fullname = fullname.decode()

rORw = fullname[0]
if rORw == "R":
    filename = fullname[1:]
elif rORw == "W":
    contentIdx1 = fullname.find('"')
    contentIdx2 = fullname.find('"', contentIdx1+1)
    filename = fullname[1:contentIdx1]
    content = fullname[contentIdx1+1:contentIdx2]
else:
    sys.exit()


print(f"filename: {filename}; Equation is received")
data_transferred = 0

if not exists(filename):
    print("no file")
    sys.exit()

print(f"sending file content: {filename}")
if rORw == "R":
    with open(filename, 'rb') as f:
        try:
            data = f.read(1024)
            while data:
                data_transferred += clientConnection.send(str(data).encode())
                data = f.read(1024)
        except Exception as ex:
            print(ex)
elif rORw == "W":
    # readlines 써보기
    with open(filename, 'r+') as f:
        try:
            data = f.read(1024)
            # 기존 코드
            # f.write(content)
            # new line에 내용 추가
            f.write(f"\n{content}")
            data += content
            data_transferred += clientConnection.send(str(data).encode())
            data = f.read(1024)
        except Exception as ex:
            print(ex)
print(f"Sending Completed: {filename}\n Data capacity: {data_transferred}")


clientConnection.close()
```

<!-- prettier-ignore-start -->

##### 클라이언트 코드

<!-- prettier-ignore-end -->

```python
import socket
from os.path import exists
import sys

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

fullname = clientConnection.recv(1024)
fullname = fullname.decode()

rORw = fullname[0]
if rORw == "R":
    filename = fullname[1:]
elif rORw == "W":
    contentIdx1 = fullname.find('"')
    contentIdx2 = fullname.find('"', contentIdx1+1)
    filename = fullname[1:contentIdx1]
    content = fullname[contentIdx1+1:contentIdx2]
else:
    sys.exit()


print(f"filename: {filename}; Equation is received")
data_transferred = 0

if not exists(filename):
    print("no file")
    sys.exit()

print(f"sending file content: {filename}")
if rORw == "R":
    with open(filename, 'rb') as f:
        try:
            data = f.read(1024)
            while data:
                data_transferred += clientConnection.send(str(data).encode())
                data = f.read(1024)
        except Exception as ex:
            print(ex)
elif rORw == "W":
    # readlines 써보기
    with open(filename, 'r+') as f:
        try:
            data = f.read(1024)
            # 기존 코드
            # f.write(content)
            # new line에 내용 추가
            f.write(f"\n{content}")
            data += content
            data_transferred += clientConnection.send(str(data).encode())
            data = f.read(1024)
        except Exception as ex:
            print(ex)
print(f"Sending Completed: {filename}\n Data capacity: {data_transferred}")


clientConnection.close()
```
