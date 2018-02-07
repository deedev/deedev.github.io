---
layout: post
title: "Factory Pattern"
description: "Factory Pattern"
comments: true
keywords: "Factory pattern, Design pattern, Java"
---
Factory Pattern là một trong những Design Pattern được sử dụng phổ biến nhất.
Factory Pattern thuộc nhóm khởi tạo (creational pattern), nó cung cấp 1 trong những cách tốt nhất để tạo ra đối tượng ( Theo Tutorials point ).

Theo quan điểm cá nhân của mình thì nên sử dụng Factory Pattern trong trường hợp muốn giảm sự phụ thuộc, giúp dễ dàng mở rộng hệ thống - ta chưa biết hệ thống có cụ thể các Object nào.

####Ví dụ:
Vào Điện Máy Xanh mua điện thoại, chỉ cần bảo, ví dụ, "Iphone 10", nhân viên sẽ mang Iphone 10 ra cho chúng ta xem.
Lúc này, Điện Máy Xanh chính là 1 Factory, dựa vào yêu cầu từ khách hàng, sẽ lấy ra đối tượng tương ứng.
OK, CODE.
![Factory](/assets/images/Factory.PNG)
```java
public interface Mobile{
    public void Info();
}

public class Iphone implements Mobile{
    @Override
    public void Info(){
        System.out.println("Iphone");
    }
}
public class Samsung implements Mobile{
    @Override
    public void Info(){
        System.out.println("Samsung");
    }
}

public class DMXBosss{

    //Tùy vào yêu cầu sẽ tạo đối tượng(Lấy ra điện thoại) tương ứng
    public Mobile getMobile(String Type){

        if(Type.equalsIgnoreCase()){
            return null;
        }
        if(Type.equalsIgnoreCase("Iphone")){
         return new Iphone();
         
      } else if(Type.equalsIgnoreCase("Samsung")){
         return new Samsung();
    }
    return null;
}

public class Demo{
    public static void main(String[] args){

        Mobile iphone = new DMXBoss().getMobile("Iphone");
        iphone.Info();///kết quả:Iphone
    }
}
```
