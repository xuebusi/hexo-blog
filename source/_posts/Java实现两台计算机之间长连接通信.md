---
title: Java实现两台计算机之间长连接通信
date: 2023-11-09 20:54:10
categories:
- Java
tags:
- Socket
- ServerSocket
- BufferedReader
- InputStreamReader
- PrintWriter
---
要实现客户端和服务器端的长连接，即一直保持连接直到程序显式关闭，你可以在客户端和服务器端都使用循环来持续地发送和接收消息。下面是相应的代码示例：

### 服务器端（B计算机）

服务器端持续监听客户端发送的消息。使用`while(true)`确保服务器始终在运行，等待和处理客户端的消息。

```java
import java.net.*;
import java.io.*;

public class Server {
    public static void main(String[] args) throws IOException {
        int port = 1234;
        ServerSocket serverSocket = new ServerSocket(port);
        System.out.println("等待连接...");
        Socket clientSocket = serverSocket.accept();
        System.out.println("连接成功！");

        BufferedReader in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));

        String inputLine;
        while (true) { // 一直保持监听状态
            inputLine = in.readLine();
            if (inputLine != null) {
                System.out.println("收到: " + inputLine);
            }
        }
        // 注意：实际应用中需要添加适当的退出条件和异常处理
    }
}
```

### 客户端（A计算机）

客户端持续发送消息到服务器。在这个例子中，我们仅以发送时间作为消息的示例。

```java
import java.net.*;
import java.io.*;
import java.util.Date;

public class Client {
    public static void main(String[] args) throws IOException, InterruptedException {
        String serverAddress = "服务器的IP地址"; // 替换为B计算机的IP地址
        int port = 1234;
        Socket socket = new Socket(serverAddress, port);

        PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
        
        while (true) { // 一直发送消息
            out.println("当前时间: " + new Date());
            Thread.sleep(1000); // 每秒发送一次
        }
        // 注意：实际应用中需要添加适当的退出条件和异常处理
    }
}
```

在这两个示例中，我没有添加退出条件和异常处理，但这在实际应用中是非常重要的。你需要确保代码能够优雅地处理网络异常、I/O异常以及如何安全地关闭连接。记住，长时间保持连接会增加资源消耗，确保服务器有足够的资源来处理这些长连接。