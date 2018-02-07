---
layout: post
title: "Decorator Pattern"
description: "Decorator pattern"
comments: true
keywords: "Design pattern, Decorator pattern, Java"
---
Decorator pattern là một mẫu thiết kế thuộc nhóm cấu trúc (Structural pattern).
Theo Tutorials Point:
>Decorator pattern allows a user to add new functionality to an existing object without altering its structure. This type of design pattern comes under structural pattern as this pattern acts as a wrapper to existing class.

Decorator pattern được sử dụng khi ta muốn thêm mới 1 chức năng cho 1 đối tượng nào đó mà không phải sửa Class.
Ví dụ thế này: 
<img src="/assets/images/Decorator.png" />

```java
    public interface Shape{
        public void Draw();
    }

    public class Circle implements Shape{
        @Override
        public void Draw(){
            System.out.println("Shape:Circle");
        }
    }

    public class Retangle implements Shape{
        @Override
        public void Draw(){
            System.out.println("Shape:Retangle");
        }
    }
```
Các Class Circle, Retangle thực hiện chức năng vẽ hình tròn và hình chữ nhật. Giả sử bây giờ bạn muốn thay đổi như thế này, sau khi vẽ xong hình tròn, hình chữ nhật thì tiến hành tô màu (giả sử là màu đỏ) cho hình đó.
Để giải quyết vấn đề này thì ta có 2 cách:
1. Viết các Class RedCircle, RedRetangle kế thừa Circle và Retangle
2. Dùng Decorator pattern

Cách thứ 1 nhìn có vẻ cũng ổn ổn, nhưng thật sự thì không ổn. Nếu có tới 100 Class thì sẽ phải viết 100 Class mới kế thừa, rất tốn công Code -> không hiệu quả.

Bây giờ ta sẽ xem Decorator pattern giải quyết vấn đề này như thế nào:
```java
    public abstract class AbstractDecorator implements Shape {
        protected Shape _shape;
        public AbstractDecorator(Shape shape){
            _shape = shape;
            }
    }

    public class Decorator extends AbstractDecorator {
        public Decorator(Shape shape) {
            super(shape);
        }
        @Override
        public void Draw() {
            _shape.Draw();
            setColor();
            }
        private void setColor(){
            System.out.println("Color:RED");
            }
    }
```
Hàm main của chúng ta:
```java
    public class Main {
        public static void main(String[] args) {
            Decorator decorator = new Decorator(new Circle());
            decorator.Draw();
            System.out.println("---------------");
            Decorator decorator1 = new Decorator(new Retangle());
            decorator1.Draw();
            }
    }
    //kết quả:
    ///Shape:Circle
    ///Color:RED
    ///-------------
    ///Shape:Retangle
    ///Color:RED
```

