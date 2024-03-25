---
title: 使用JavaNIO编写一个HelloWorld程序
date: 2024-03-17 10:52:45
tags:
---

以下是一个简单的 Java NIO 客户端和服务端的 "Hello World" 程序：

**服务端程序（Server.java）：**

```java
import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.ServerSocketChannel;
import java.nio.channels.SocketChannel;

public class Server {
    public static void main(String[] args) {
        try {
            // 创建 ServerSocketChannel
            ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
            
            // 绑定端口
            serverSocketChannel.bind(new InetSocketAddress(9999));
            System.out.println("Server started, listening on port 9999...");
            
            while (true) {
                // 接受客户端连接
                SocketChannel socketChannel = serverSocketChannel.accept();
                
                // 读取数据
                ByteBuffer buffer = ByteBuffer.allocate(1024);
                socketChannel.read(buffer);
                buffer.flip();
                System.out.println("Received message from client: " + new String(buffer.array()).trim());
                
                // 发送响应
                String response = "Hello Client!";
                ByteBuffer responseBuffer = ByteBuffer.wrap(response.getBytes());
                socketChannel.write(responseBuffer);
                
                // 关闭连接
                socketChannel.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

**客户端程序（Client.java）：**

```java
import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.SocketChannel;

public class Client {
    public static void main(String[] args) {
        try {
            // 创建 SocketChannel
            SocketChannel socketChannel = SocketChannel.open();
            
            // 连接到服务器
            socketChannel.connect(new InetSocketAddress("localhost", 9999));
            System.out.println("Connected to server...");
            
            // 发送数据
            String message = "Hello Server!";
            ByteBuffer buffer = ByteBuffer.wrap(message.getBytes());
            socketChannel.write(buffer);
            
            // 接收响应
            ByteBuffer responseBuffer = ByteBuffer.allocate(1024);
            socketChannel.read(responseBuffer);
            responseBuffer.flip();
            System.out.println("Received response from server: " + new String(responseBuffer.array()).trim());
            
            // 关闭连接
            socketChannel.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

这个程序包括一个简单的客户端和服务器端。服务器监听端口9999，并在接收到客户端消息后，返回 "Hello Client!"。客户端连接到服务器，发送消息 "Hello Server!"，并打印出从服务器接收到的响应。