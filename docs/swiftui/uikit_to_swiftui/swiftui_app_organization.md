---
layout: default
title: SwiftUI App Organization
parent: SwiftUI
nav_order: 3
---

## Table of contents

{: .no_toc .text-delta }

1. TOC
{:toc}

## App protocol

[App protocol](https://developer.apple.com/documentation/swiftui/app) là protocol cho phép khai báo cấu trúc và hành vi của ứng dụng.

Tất cả các ứng dụng SwiftUI đều chỉ có duy nhất 1 entry point và được bắt đầu với keyword `@main` và conform to protocol `App`

```swift
import SwiftUI

@main
struct HelloWorldApp: App {
    var body: some Scene {
        WindowGroup {
            Text("Hello, world!")
        }
    }
}
```

Một ứng dụng SwiftUI có thể có nhiều Scene, tuy nhiên việc ứng dụng có nhiều Scene thì thường dành cho iPadOS hoặc MacOS.

Có thể khai báo các state object để share data giữa các scene (hoặc trong toàn app)

```swift
@main
struct Mail: App {
    @StateObject private var model = MailModel()

    var body: some Scene {
        WindowGroup {
            MailViewer()
                .environmentObject(model) // Passed through the environment.
        }
        Settings {
            SettingsView(model: model) // Passed as an observed object.
        }
    }
}
```

## App delegate

[UIApplicationDelegateAdaptor](https://developer.apple.com/documentation/swiftui/uiapplicationdelegateadaptor)

Trong trường hợp muốn sử dụng UIKit app delegate, thì tạo class conform to protocol [UIApplicationDelegate](https://developer.apple.com/documentation/uikit/uiapplicationdelegate) và sử dụng [UIApplicationDelegateAdaptor](https://developer.apple.com/documentation/swiftui/uiapplicationdelegateadaptor) để khai báo instance của UIKit app delegate. Ví dụ sử dụng UIKit delegate để lấy remote notification device token:

Define custom class conform to UIApplicationDelegate

```swift
final class HelloWorldAppDelegate: NSObject, UIApplicationDelegate, ObservableObject {
    func application(
        _ application: UIApplication,
        didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data
    ) {
        // Record the device token.
    }
}
```

Khai báo delegate adaptor trong `App`

```swift
@main
struct HelloWorldApp: App {
    @UIApplicationDelegateAdaptor private var appDelegate: HelloWorldAppDelegate

        var body: some Scene {
        WindowGroup {
            Text("Hello, world!")
        }
    }
}
```

> App delegate adaptor phải được define trong `App`, chỉ được khai báo duy nhất 1 lần.

Trong trường hợp app delegate có conform protocol `ObservableObject` thì SwiftUI sẽ tự động đưa instance của app delegate vào Environtment, do đó thì từ bất cứ scene hoặc view bất kì đều có thể truy cập vào app delegate object thông qua EnvirontmentObject như dưới

```swift
@EnvironmentObject private var appDelegate: HelloWorldAppDelegate
```

> Note: trong trường hợp muốn quản lý app life cycle event thì sử dụng [ScenePhase](https://developer.apple.com/documentation/swiftui/scenephase) thay cho app delegate.

## Scene Delegate

Đăng ký scene delegate thông qua scene environment object

```swift
@Environment(\.scenePhase) private var scenePhase
```

Sample source

```swift
struct MyScene: Scene {
    @Environment(\.scenePhase) private var scenePhase

    var body: some Scene {
        WindowGroup {
            ContentView()
        }
        .onChange(of: scenePhase) { phase in
            switch phase {
            case .background:
                Logger.info("App Background State")
            case .inactive:
                Logger.info("App Inactive State")
            case .active:
                Logger.info("App Active State")
            @unknown default:
                Logger.info("Unknow App State")
            }
        }
    }
}
```

ScenePhase có 3 trạng thái là `active`, `inactive` và `background`. Lưu ý rằng đây là scene delegate, khác với app delegate, tức là áp dụng với từng scene trong app.

## Navigation

Từ iOS 16 thì SwiftUI có `NavigationStack` hoạt động tương tự như `UINavigationController` trong UIKit, có thể add thêm view vào Navigation stack hiện tại thông qua `NavigationLink` và remove view mới nhất được add vào stack thông qua Back Button hoặc Swipe gesture. Root view của Navigation Stack thì không thể remove được.

Vì navigation stack được sử dụng với iOS 16+ cho nên tạm thời không đào sâu hơn.

## Model presentations

> Noted: Có sự khác biệt giữa phiên bản iOS 14 và iOS 15.

**Showing sheet, cover, popover**

Sử dụng:

- `sheet(isPresented:onDismiss:content:)`
- `fullScreenCover(isPresented:onDismiss:content:)`
- `popover(isPresented:attachmentAnchor:arrowEdge:content:)`

Có thể config size, height, background...

**Alert**

Sử dụng (iOS15+):

- `alert(_:isPresented:actions:)`
- `alert(_:isPresented:actions:message:)`
- `confirmationDialog(_:isPresented:titleVisibility:actions:)`
- `confirmationDialog(_:isPresented:titleVisibility:actions:message:)`

> **Noted**: Trong trường hợp version iOS14 trở về trước thì dùng:

- `ActionSheet`
- `Alert`

## Toolbars
## Search
## App extensions