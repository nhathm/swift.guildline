---
layout: default
title: SwiftUI View Fundamentals
parent: SwiftUI
nav_order: 5
---

## Table of contents  
{: .no_toc }

1. TOC
{:toc}

## SwiftUI's View overview

Trong SwiftUI thì tất cả view được khai báo dưới dạng struct, tức là mỗi khi có sự thay đổi data thì View luôn được vẽ lại. Và việc vẽ lại view hoàn toàn do SwiftUI tự xử lý, nó khác biệt khá nhiều so với UIKit trước đây.

Khi thay đổi từ UIKit sang SwiftUI, có sự thay đổi lớn về cách mà view được vẽ. Do đó, với những người đã từng code UIKit, khi chuyển sang SwiftUI sẽ có những khó khăn nhất định về mặt tư duy. Tuy nhiên với những người đã làm quen với RxSwift, RxCocoa thì có khả năng sẽ thấy quen thuộc.

Với UIKit, cách UIKit vẽ view là sử dụng phương pháp "imperative - mệnh lệnh". Cụ thể, để khởi tạo 1 view thì ta thường khởi tạo view đó, ví dụ UIButton, set frame, set title, set color, add vào parent view. Trong trường hợp sử dụng auto layout thì sẽ phải set auto layout config cho nó thông qua UI hoặc source code. Cùng xem ví dụ bên dưới.

```swift
let button = UIButton(type: .system)
button.setTitle("Login", for: .normal)
button.frame = CGRect(x: 50, y: 50, width: 100, height: 50)

view.addSubview(button)
```

Ở đoạn code trên, ta khởi tạo button bằng cách init nó, set title cho nó, cho nó frame và tọa độ mong muốn trên parent view, sau đó add vào parent view.  
Còn khi muốn update view, thì ta phải select view ra (thông qua outlet) rồi update trạng thái, state cho view.

Tuy nhiên, với SwiftUI thì khác. SwiftUI là framework vẽ view bằng phương pháp "declarative - khai báo". Có thể hiểu rẳng, developer chỉ cần khai báo cách mà họ muốn view hiển thị, SwiftUI sẽ tự vẽ và xử lý layout theo những gì đã được khai báo.

struct HomeView: View {
    var body: some View {
        Text("Home View")
    }
}
