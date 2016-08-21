---
title: Java nio 实现文件复制
id: 108
categories:
  - Java复习
date: 2015-06-04 12:35:06
tags:
    - Java nio
    - Java
---

[code lang="java"]
/**
 * Created by lei on 15-6-4.
 */
import java.io.*;
import java.nio.*;
import java.nio.channels.FileChannel;

public class MyCopy1 {
    public static void main(String[] args) throws IOException {
        File file1 = new File(args[0]);
        File file2 = new File(args[1]);
        FileOutputStream fileOutputStream = new FileOutputStream(file2);
        FileInputStream fileInputStream = new FileInputStream(file1);
        FileChannel fileChannel = fileInputStream.getChannel();
        FileChannel fileChannel1 = fileOutputStream.getChannel();
     //   fileChannel.transferTo( 0, fileChannel.size(),fileChannel1);
        fileChannel1.transferFrom(fileChannel, 0, fileChannel.size());
        fileChannel.close();
        fileChannel1.close();

    }
}

[/code]