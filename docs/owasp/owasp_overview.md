---
layout: default
title: OWASP overview
parent: OWASP
nav_order: 1
has_toc: true
---

# OWASP Top 10 for Mobile  
{: .no_toc }

## Table of contents  
{: .no_toc .text-delta }

1. TOC
{:toc}

[**OWASP (Open Web Application Security Project)**](https://owasp.org/) là dự án xây dựng một nền tảng hướng đến việc tăng cường bảo mật cho phần mềm. Bắt nguồn là cho các dự án Web app, Back end. Tuy nhiên hiện nay thì bảo mật cho các dự án Mobile cũng đã trở nên cấp thiết hơn rất nhiều, và series bài viết này sẽ hướng đến tìm hiểu về các vấn đề về security, phương thức tấn công, các phương pháp phòng tránh, các checklist để tạo ra một project mobile an toàn hơn.

### 1. Improper Platform Usage

Improper Platform Usage tức là sử dụng các chức năng (features) của nền tảng mobile (iOS/Android) một cách không phù hợp.

Cụ thể, từng OS system sẽ cung cấp cho developers các tính năng (capabilities, features) có thể sử dụng để phát triển ứng dụng. Nếu developers không sử dụng các tính năng này để phát triển, hoặc sử dụng chúng một cách không chính xác, thì sẽ được gọi là `improper use` (sử dụng không đúng cách).

Ví dụ: với iOS thì lưu các thông tin quan trọng của user như passworld hay token thì không được sủ dựng UserDefault để lưu mà cần lưu bằng Keychain, tuy nhiên, việc sử dụng sai config của Keychain cũng tiềm ẩn rủi ro bị tấn công lộ mật khẩu. Hoặc với những quyền truy cập vào thông tin user như Location, HealthKit, Photos cũng không được sử dụng một cách thiếu cân nhắc.

### 2. Insecure Data Storage

Insecure data storage, lưu trữ dữ liệu một cách không an toàn, không đơn thuần chỉ là cách lưu trữ data (ảnh, text, sql data...) một cách không an toàn. Mà còn có thể là log file, cookie file, cached file (URL, browser, third party data...). Một ví dụ điển hình là các developer rất hay sử dụng log trong quá trình phát triển phần mềm, rất dễ tiềm ẩn nguy cơ "lỡ tay" log ra các thông tin nhạy cảm mà quên không loại bỏ, dẫn đến việc bị lọt ra môi trường product.

Những item cần cân nhắc khi nghĩ đến nguy cơ lộ data:

- Cách OS cache data, cache image, có thể là cả logging, buffer, thậm chí key-press (đặc biệt là OS mở như Android)
- Cách các framework được sử dụng trong dự án cache data
- Cách các thư viện open source cache data, gửi / nhận data

### 3. Insecure Communication

Các ứng dụng mobile thì luôn cần phải transfer data, có thể là giữa client - server, giữa các devices với nhau. Và trong quá trình gửi nhận data, nếu không tuân thủ các quy tắc bảo mật thì có thể bị kẻ xấu tấn công ăn cắp các thông tin nhạy cảm. Đó có thể là password, thông tin account, hoặc các thông tin private của user.

Việc tấn công ăn cắp thông tin có thể thông qua wifi mà devices đang truy cập, các router nhà mạng hoặc các thiết bị ngoại vi BLE, NFC...

### 4. Insecure Authentication

Insecure Authentication mô tả việc thực hiện xác thực user kém bảo mật dẫn đến người khác có thể lợi dụng để tấn công vào backend nhằm ăn cắp thông tin hoặc thực hiện các request tổn hại đến hệ thống.
Vấn đề này có thể do việc thiết kế backend thiếu security như việc request không có access token, hoặc từ bản thân mobile app đã thực hiện việc authenticate offline sơ sài (ví dụ như set passcode ngắn để truy cập vào chức năng quan trọng, hoặc lưu password của user) dẫn đến việc kẻ tấn công có thể ăn cắp thông tin và truy cập vào server.

### 5. Insufficient Cryptography

Việc bảo mật dữ liệu trong mobile app thường áp dụng mã hóa. Tuy nhiên, trong một vài trường hợp thì mã hõa vẫn sẽ tiềm ân rủi ro bị tấn công ăn cắp dữ liệu. Một vài khả năng có thể kể đến như:

- Quá ỷ lại vào mã hóa của OS (Built-In Code Encryption Processes), ví dụ iOS bản thân nó cũng sẽ mã hóa ứng dụng, tuy nhiên nếu devices bị jail break thì cũng có khả năng decode ứng dụng để lấy dữ liệu.
- Sử dụng một cơ chế mã hóa đã lỗi thời hoặc tự viết lại thuật toán mã hóa.
- Lưu trữ encrytion key một cách thiếu bảo mật.

### 6. Insecure Authorization

- Authentication: xác thực user
- Authorization: xác thực quyền của user

Việc thiếu sót trong việc xác thực quyền hạn của user trong hệ thống có thể dẫn đến sai sót trong việc cho phép user được truy cập vào những tài nguyên không được cho phép, hoặc có tính bảo mật cao. Dẫn đến việc mất mát các thông tin nhạy cảm của server hoặc bị thực thi những quyền có tính chất ảnh hưởng lớn đến hệ thống.
Các vấn đề có thể xảy ra trong trường hợp này như:

- Server không check quyền của user khi request lên, mà hoàn toàn dựa vào request của client, ví dụ người tấn công có thể lợi dụng bằng cách thêm các param vào GET/POST request để lừa server rằng "tao là admin" và có thể truy cập vào các thông tin nhạy cảm.
- Một số trường hợp có thể server cung cấp các API dành riêng cho "admin" và nghĩ rằng các user bình thường sẽ không biết các API này nên không cần phải check quyền, tuy nhiên việc này không hề đảm bảo đến việc sẽ bảo mật được các API này mà không lộ ra ngoài cho các user không có quyền khác.

### 7. Poor Code Quality

Poor code quality là các lỗi bảo mật liên quan đến bản thân ngôn ngữ lập trình, có thể kể đến như lỗi:

- Buffer overflows: các lỗi buffer overflows thì hay xảy ra với các thư viện viết bởi C, C++, và iOS cùng Android đề sử dụng các thư viện C, C++ trong hệ thống, do đó kẻ xấu có thể tấn công vào các thư viện này và qua đó thực thi các đoạn lệnh hoặc thậm chí cài mã độc vào ứng dụng.
- Format string vulnerabilities: những lỗi dạng này thường là kẻ tấn công cố tình input các đoạn string mã hóa hoặc format đặc biệt vào ứng dụng, hoặc các câu lệnh để tấn công vào ứng dụng. Qua đó có thể gây ra các lỗi như crash ứng dụng, view data trong stack, view thông tin trong memory, thậm chí thực thi source code
- Tấn công vào third party hoặc tấn công thông qua webview của OS: sử dụng các thư viện kém bảo mật cũng tiềm ẩn nguy cơ bị tấn công. Ngoài ra, trong các version OS cũ, cả iOS và Android đều bộc lộ rất nhiều lỗi liên quan đến webview như thực thi các đoạn javascript nhằm ăn cắp thông tin user.

### 8: Code Tampering

Các ứng dụng mobile, về cơ bản là sẽ nằm trên máy của user, do đó nên ứng dụng mobile rất dễ bị tấn công thay đổi nội dung source code để thực hiện các mục đích xấu.
Có thể kể đến các kịch bản tấn công như: kẻ xấu sẽ cài đặt ứng dụng của bạn lên device đã bị jail, qua đó có thể xem được nội dung bên trong của ứng dụng như assets, resource. Thậm chí thay đổi source code hoặc các API được call.
Mục đích của việc tấn công này có thể là unlock các chức năng ẩn, hoặc trả phí, hoặc ăn cắp thông tin.
Một vài trường hợp có thể là cài mã độc vào, thay đổi asset, resource, sau đó lại distribute ứng dụng lên các kho ứng dụng lậu nhằm ăn cắp thông tin của người tải về.
Trước đây thì mình hay jail device để cheat game, hoặc các ứng dụng ngân hàng cũng có thể là mục tiêu yêu thích để bị tấn công dạng này.
-> Hãy check Jail/Root khi khởi động ứng dụng.

### 9. Reverse Engineering

Kẻ tấn công sẽ down ứng dụng và sử dụng các tool để tấn con giải mã source code nhằm các mục đích như ăn cắp thông tin trong string table/plist, đọc source code, tìm hiểu thuật toán hoặc các thư viện mà app sử dụng.
Với kiểu tấn công này thì mục tiêu tấn công thường là

- Ăn cắp thông tin về backend server, như url, path, cert nếu có
- Ăn cắp các key của thuật toán mã hóa
- Ăn cắp các thông tin liên quan đến sở hữu trí tuệ

### 10. Extraneous Functionality

Các chức năng không liên quan đến ứng dụng?

Khi ứng dụng được phát triển, trên thực tế nó có rất nhiều các chức năng không thuộc functional requirement và non-functional requirement, nhưng là không thể thiếu trong quá trình phát triển ứng dụng như:

- Log file, log console để debug những thông tin API, thông tin user hoặc thông tin của ứng dụng để debugging trong quá trình phát triển
- Các switch flag để on off các chức năng nào đó trong ứng dụng, có thể là chức năng của admin hoặc chức năng liên quan đến A-B testing...
- Các API path nhằm debug nhưng quên không loại bỏ khi lên môi trường production.

Các chức năng này nếu bị khai thác có thể làm lộ thông tin về ứng dụng cũng như thông tin về user.
