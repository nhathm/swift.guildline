---
layout: default
title: OWASP overview
parent: OWASP
nav_order: 1
---

# Navigation Structure
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

[**OWASP (Open Web Application Security Project)**](https://owasp.org/) là dự án xây dựng một nền tảng hướng đến việc tăng cường bảo mật cho phần mềm. Bắt nguồn là cho các dự án Web app, Back end. Tuy nhiên hiện nay thì bảo mật cho các dự án Mobile cũng đã trở nên cấp thiết hơn rất nhiều, và series bài viết này sẽ hướng đến tìm hiểu về các vấn đề về security, phương thức tấn công, các phương pháp phòng tránh, các checklist để tạo ra một project mobile an toàn hơn.

## OWASP Top 10 for Mobile

### 1. Improper Platform Usage

Improper Platform Usage tức là sử dụng các chức năng (features) của nền tảng mobile (iOS/Android) một cách không phù hợp.

Cụ thể, từng OS system sẽ cung cấp cho developers các tính năng (capabilities, features) có thể sử dụng để phát triển ứng dụng. Nếu developers không sử dụng các tính năng này để phát triển, hoặc sử dụng chúng một cách không chính xác, thì sẽ được gọi là `improper use` (sử dụng không đúng cách).

Ví dụ: với iOS thì lưu các thông tin quan trọng của user như passworld hay token thì không được sủ dựng UserDefault để lưu mà cần lưu bằng Keychain, tuy nhiên, việc sử dụng sai config của Keychain cũng tiềm ẩn rủi ro bị tấn công lộ mật khẩu. Hoặc với những quyền truy cập vào thông tin user như Location, HealthKit, Photos cũng không được sự dụng một cách thiếu cân nhắc.

### 2. Insecure Data Storage

Insecure data storage, lưu trữ dữ liệu một cách không an toàn, không đơn thuần chỉ là cách lưu trữ data (ảnh, text, sql data...) một cách không an toàn. Mà còn có thể là log file, cookie file, cached file (URL, browser, third party data...). Một ví dụ điển hình là các developer rất hay sử dụng log trong quá trình phát triển phần mềm, rất dễ tiềm ẩn nguy cơ "lỡ tay" log ra các thông tin nhạy cảm mà quên không loại bỏ, dẫn đến việc bị lọt ra môi trường product.

Những item cần cân nhắc khi nghĩ đến nguy cơ lộ data:

- Cách OS cache data, cache image, có thể là cả logging, buffer, thậm chí key-press (đặc biệt là OS mở như Android)
- Cách các framework được sử dụng trong dự án cache data
- Cách các thư viện open source cache data, gửi / nhận data

### 3.Insecure Communication

Your app transports data from point A to point B. If this transport is insecure, the risk increases. Here, too, the main mobile application penetration testing tools will help you. They support you in detecting faulty app-to-server or mobile-to-mobile communication. The biggest problem is the transfer of sensitive data from one device to another. This could be encryption, passwords, account details or private user information. If the necessary security measures are missing at this point, it is easy for hackers to access your data.

Các ứng dụng mobile thì luôn cần phải transfer data, có thể là giữa client - server, giữa các devices với nhau. Và trong quá trình gửi nhận data, nếu không tuân thủ các quy tắc bảo mật thì có thể bị kẻ xấu tấn công ăn cắp các thông tin nhạy cảm.
