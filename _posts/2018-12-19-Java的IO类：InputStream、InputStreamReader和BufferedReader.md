---
title: Java的IO类：InputStream、InputStreamReader和BufferedReader
category: Java
---

# Java的IO类：InputSteam、InputStreamReader和BufferedReader

### 对比

![三种IO类对比](https://github.com/inter18099/articles/blob/master/img/2018-12-19-Java的IO类：InputStream、InputStreamReader和BufferedReader/2018-12-19-Java的IO类：InputStream、InputStreamReader和BufferedReader-1.png?raw=true "三种IO类对比")

### InputStream

InputStream是读取byte的stream。它通过read()方法，一个接一个读取byte。

一个byte就是简单的一个数。存在硬盘上的数据就是以byte形式储存的。Byte能被解释成characters, numbers...

### InputStreamReader

Reader是关于character stream的类。它通过read()方法，一个接一个地读取character。

InputStreamReader以InputStream为参数，并且将bytes转换characters。但还是不方便，因为只能一次读取一个character。

```
new InputStreamReader(stream)
```

### BufferedReader

BufferedReader缓冲了一个character stream，所以你能一次读取一行。

```
–String readLine()

new BufferedReader(
   new InputStreamReader(System.in));
```

### User Input

```
InputStreamReader ir = new
   InputStreamReader(System.in);
BufferedReader br = new BufferedReader(ir); br.readLine();
```

### FileReader

FileReader以file为参数，将file转换成character stream。

```
– FileReader(“PATH TO FILE”);
```

用FileReader + BufferedReader 去读文件

```
FileReader fr = new FileReader(“readme.txt”); 
BufferedReader br = new BufferedReader(fr);
```

### FileReader Code

```
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
public class ReadFile {
} }
FileReader Code
public static void main(String[] args) throws IOException{
    // Path names are relative to project directory (Eclipse Quirk ) FileReader fr = new FileReader("./src/readme");
    BufferedReader br = new BufferedReader(fr);
    String line = null;
    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }
    br.close();
    } 
}
```

### more

http://java.sun.com/docs/books/tutorial/essential/io/

