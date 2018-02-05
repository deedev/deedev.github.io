---
layout: post
title: "Builder pattern"
description: "There is no one who loves pain itself, who seeks after it and wants to have it, simply because it is pain..."
comments: true
keywords: "Builder pattern, Design pattern, Java"
---
Design pattern hiểu đơn giản là các mẫu thiết kế, được đúc và kết từ bao kinh nghiệm xương máu của các tiền bối đi trước. Hiểu Design pattern và biết áp dụng hợp lý vào code sẽ giúp code của cu-der dễ bảo trì, mở rộng hơn.
Design pattern gồm 3 nhóm chính: Nhóm khởi tạo(Creational), nhóm cấu trúc(Structural) và Hành vi(Behavior).
Có khoảng 23 mấu Design pattern được sử dụng phổ biến.
Hôm nay, ta sẽ tìm hiểu về Builder.
### Builder pattern là cái khỉ gì?
Theo Tutorials Point:
>Builder pattern builds a complex object using simple objects and using a step by step approach. This type of design pattern comes under creational pattern as this pattern provides one of the best ways to create an object.

Tóm lại thì, Builder pattern là 1 design pattern thuộc nhóm creational, sử dụng nguyên lý của Nested Class [[^1]] , dùng để tạo các Object phức tạp.
### Khi nào dùng Builder?
Như trên, khi cần tạo các Object phức tạp, các Object có nhiều thuộc tính, trong có có những thuộc tính bắt buộc, có những thuộc tính không bắt buộc, hoặc các Object có tính tùy biến cao.
Ví dụ thực tế: Muốn pha 1 cốc nước để uống, thì có người thích cho đường + chanh, có người thích chanh muối, người thích đường + chanh + đá, ... rất nhiều lựa chọn. Khi đó, muốn xây dượng 1 lớp thực hiện công việc pha 1 cốc nước uống, ta sẽ dùng Builder.
### Ví dụ: <--tạo 1 cốc nước uống -->
```java
    ///Để đơn giản, dễ hiểu, lớp này chỉ có 3 thộc tính
    public class Demo{
        //bắt buộc
        private int Chanh;
        //tùy chọn
        private boolean Duong;
        private boolean Muoi;
    }
```
Bình thường, nếu chưa biết Builder thì sẽ có 2 cách giải quyết vấn đề này: Dùng getter/setter (ai cũng biết cả kaka), hoặc dùng 1 kĩ thuật gọi là telescoping constructor [[^2]] (tạo nhiều hàm khởi tạo ).
Cả 2 cách này đều có vấn đề là code trông chán ứ chịu được, nhìn vào code tạo một đối tượng Demo, ta không biết nó gồm những gì, phải nhớ các thuộc tính bắt buộc (telescoping constructor).

```java
    //Cách dùng getter/setter
    Demo demo = new Demo();
    demo.setChanh(2);
    demo.Duong(false);
    demo.Muoi(true);
    ...
    ///Ví dụ này chỉ có 3 thuộc tính nên trông vẫn đơn giản,
    /// nhưng trong thực tế khi có tới 13 thuộc tính chẳng hạn thì trông code rất phức tạp...
```
```java
    //Cách dùng telescoping
    Demo demo = new Demo(2, false, true);
    ...
    ///Khi có nhiều tham số, nhìn vào code này sẽ không biết 
    ///nó là những gì - ví dụ này là chanh, đường, muối..
    ví dụ: new Vidu(p1, p2, p3, p4, p5, p6, ...)
```
Khi dùng Builder:
```java
    public class Demo{
        //bắt buộc
        private int Chanh;
        //tùy chọn
        private boolean Duong;
        private boolean Muoi;
    public int getChanh() {
        return Chanh;
    }
    public boolean isMuoi() {
        return Muoi;
    }
    public boolean isDuong() {
        return Duong;
    }
    //Bạn có thể tách Builder ra lớp riêng, nhưng mình thích cách viết này hơn :))
    public static class  Builder{
        //bắt buộc
        private int Chanh;
        //tùy chọn
        private boolean Muoi = false;
        private boolean Duong = false;
        
        public Builder(int chanh){
            Chanh = chanh;
        }
        public Builder coMuoi(){
            Muoi = true;
            return this;
        }
        public Builder coDuong(){
            Duong = true;
            return this;
        }
        public Demo build(){
            return new Chicken(this);
        }
    }
    //để private để bắt buộc phải tạo Ob qua Builder, không cho tạo trực tiếp
    private Demo(Builder builder){
        Chanh = builder.Chanh;
        Muoi = builder.Muoi;
        Duong = builder.Duong;
    }
}
```
Bây giờ sẽ tạo mới 1 đối tượng nhé:
```java
    //2 quả chanh + muối
    Demo Chanh_muoi = new Demo.Builder(2).coMuoi().build();
    Demo chanh_thap_cam = new Demo.Builder(2).coMuoi().coDuong().build();
    //WOW, trông code dễ hiểu hơn hẳn, nhìn là biết thành phần gồm những gì :))
```
BYEBYE-DEEDEE

---
Footnote:
[^1]: 1: Nested Class  [click here!]("https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html")

[^2]: 2: Telescoping Constructor [click here!]("http://www.captaindebug.com/2011/05/telescoping-constructor-antipattern.html#.WnfgDOf7LDc")