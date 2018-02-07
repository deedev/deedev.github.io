---
layout: post
title: "Proxy Pattern"
description: "Proxy pattern"
comments: true
keywords: "Design pattern, Proxy pattern, Java"
---
Proxy Pattern là Design pattern thuộc nhóm cấu trúc (structural pattern).
Proxy (đại diện) được sử dụng khi: 
1. Muốn quản lý hoặc bảo vệ một đối tượng, không để cho Client truy cập trực tiếp tới đối tượng đó, mọi truy cập phải qua Proxy, do Proxy quản lý. 
2. Tiết kiệm bộ nhớ, băng thông, tăng hiệu năng: Chỉ tạo đối tượng khi thật sự cần thiết, chỉ tạo 1 lần

#### Ví dụ :
![Proxy](/assets/images/Proxy.PNG)


Class Demo sẽ sử dụng ProxyImage để load Image và hiển thị nó khi cần thiết.
```java
public interface Image{
    public void ShowImage();
}

public class RealImage implements Image{
    private String fileName;
    public RealImage(String fileName){
        fileName = name;
        LoadImage(fileName)
    }
    @Override
    public void ShowImage(){
        System.out.println("Showing Image"+fileName);
    }
    public void LoadImage(String Name){
        System.out.println("Loading Image");
    }
}

public class ImageProxy implements Image{
    private String fileName;
    private RealImage realImage;

    public ImageProxy(String fileName){
        fileName = fileName;
    }

    @Override
    public void ShowImage(){
        if(realImage == null){
            realImage  = new RealImage(fileName);
        }
        realImage.ShowImage();
    }
}
public class Demo{
    public static void main(String[] args){
        Image img = new ImageProxy("ahihi.png");
        img.ShowImage();
        //từ lần tới khi sử dụng sẽ không load lại Image nữa
    }
}
```
--->Ahihi