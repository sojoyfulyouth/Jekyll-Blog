---
layout: post
title: "Socket Programming"
date: 2023-06-19
categories: ComputerNetwork
---

## 1. Calculator: client가 문자열 형태로 수식을 입력하면 결과값을 반환하는 코드

### 서버 코드

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

```javascript
const Razorpay = require("razorpay");

let rzp = Razorpay({
  key_id: "KEY_ID",
  secret: "name",
});

// capture request
rzp.capture(payment_id, cost).then(function (data) {
  return 2;
});
```

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]: https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
