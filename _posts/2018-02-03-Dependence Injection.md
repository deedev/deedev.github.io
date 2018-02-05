---
layout: post
title: "Dependence Injection"
description: "Dependence Injection"
comments: true
keywords: "DI, Dependence Injection, Java"
---
Theo như trên Google : 
>Dependence Injection là một mẫu lập trình (Design pattern), được sử dụng để cố gắng đạt được sự không phụ thuộc giữa các Objeect với nhau khi có quan hệ phụ thuộc giữa Object này với một Object khác.

Đọc cái định nghĩa này thì, haizz, có thánh mới hiểu được. Nó quá trừu tượng.
Áp dụng DI vào code, mặc dù làm cho code của ta trông phức tạp hơn, nhưng ngược lại sẽ làm cho hệ thống trở nên mềm dẻo, dễ nâng cấp hơn. 

Bây giờ ta sẽ vào một ví dụ cụ thể : Giả sử chúng ta làm một hệ thống quản lý 1 trung tâm mua sắm chẳng hạn và cần tính năng thông báo các chương trình khuyến mãi tới khách hàng qua Email của họ.
Với yêu cầu này, ta sẽ xây dựng 1 class MailSender để gửi mail thông báo tới khách hàng:
```java
    public class MailSender{
        public void Notify(String email){
            //code xử lý...
        }
    }
```
Và Class Account lưu thông tin khách hàng:
```java
    public class Account{
        private String Name;
        private String Email;

        //code xử lý cái gì gì đó
        public void NotifyService(){
            MailSender mailsender = new MailSender();
            mailsender.Notify(Email);
        }
    }
```
Đoạn code trên thì có vấn đề gì không? Câu trả lời là có.
Trong lớp Account, ta khởi tạo 1 đối tượng MailSender, tức là lớp Account phụ thuộc cứng vào lớp MailSender. Khi ta không muốn thông báo bằng  Email nữa, chuyển sang bằng SMS chẳng hạn, ta phải tiến hành sửa Class Account, và các Class khác - các Class cũng sử dụng MailSender.

Và để giải quyết được vấn đề này, ta sẽ sử dụng Dependence Injection.
Đầu tiên ta sẽ tạo 1 Interface ISender, các class như SMSSender, EmailSender...sẽ Implement Interface này.
```java
    public interface ISender{
        public void Sender(String text);
    }
```
```java
    public class EmailSender implements ISender{
        @Overide
        public void Sender(String text){
            ///xử lý gì gì đó
        }
    }
```
```java
    public class SMSSender implements ISender{
        @Overide
        public void Sender(String text){
            ///xử lý gì gì đó
        }
    }
```
OK, và class Account:
```java
    public class Account{
        private String Name;
        private String Email;
        private ISender _isender;
        //Bạn có thể truyền ISender qua setter, nhưng mình thích cách này hơn :))
        public Account(ISender sender){
            _isender = sender;
        }
        public void NotifyService(String text){
            _isender.Sender(text);
        }
    }
```
Bạn thấy đó, Class Account không hệ phụ thuộc vào các class SMSSender, EmailSender..mà chỉ phụ thuộc vào Interface.
Bây giờ ta sẽ tạo 1 class gọi là Config, để cấu hình dịch vụ thông báo nào sẽ được sử dụng (Email, SMS, Facebook...)
```java
    public class Config{
        public ISender getConfig(){
            //return new EmailSender();
            return new SMSSender();
            ///hoặc có thể load từ 1 file cấu hình .xml chẳng hạn
            ///khi cần thay đổi, ta chỉ cần sửa dòng code trên
            ///không cần chỉnh sửa gì ở Account hay bất kỳ
            ///class nào khác
        }
    }
```
Hàm Main trong chương trình:
```java
    ///code gì gì...
    Account acc = new Account(new Config.getConfig());
    acc.NotifyService("DI");
    ///...
```
Trên đây là ý hiểu của mình sau khi tìm hiểu về DI, chắc là có nhiều sai sót. :))
