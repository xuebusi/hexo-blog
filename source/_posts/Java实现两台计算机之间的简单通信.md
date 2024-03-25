---
title: Java实现两台计算机之间的简单通信
date: 2023-11-09 20:41:22
categories:
- Java
tags:
- Socket
- ServerSocket
---
要在Java中实现从一台计算机（A）向另一台计算机（B）发送数据，你可以使用Java的套接字编程。这通常涉及到创建服务器端和客户端程序。服务器在B计算机上运行，监听一个端口等待连接；客户端运行在A计算机上，连接到服务器并发送数据。

这里是一个简单的示例：

### 服务器端（在B计算机上运行）

1. 创建`ServerSocket`对象并指定一个端口。
2. 调用`accept`方法等待客户端连接。
3. 通过建立的连接读取数据。
4. 关闭连接。

```java
import java.net.*;
import java.io.*;

public class Server {
    public static void main(String[] args) throws IOException {
        int port = 1234; // 选择一个端口
        ServerSocket serverSocket = new ServerSocket(port);

        System.out.println("等待连接...");
        Socket clientSocket = serverSocket.accept();
        System.out.println("连接成功！");

        BufferedReader in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));
        String inputLine;
        while ((inputLine = in.readLine()) != null) {
            System.out.println("收到: " + inputLine);
        }

        in.close();
        clientSocket.close();
        serverSocket.close();
    }
}
```

### 客户端（在A计算机上运行）

1. 创建`Socket`对象，指定服务器地址和端口。
2. 获取输出流并发送数据。
3. 关闭连接。

```java
import java.net.*;
import java.io.*;

public class Client {
    public static void main(String[] args) throws IOException {
        String serverAddress = "服务器的IP地址"; // 替换为B计算机的IP地址
        int port = 1234;
        Socket socket = new Socket(serverAddress, port);

        PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
        out.println("你好，服务器！");

        out.close();
        socket.close();
    }
}
```

注意：
- 请确保服务器和客户端使用相同的端口号。
- 需要处理网络异常和I/O异常。
- 根据你的网络设置，你可能需要配置防火墙允许这些端口的通信。
- 在实际应用中，还需要考虑安全性和效率等问题。