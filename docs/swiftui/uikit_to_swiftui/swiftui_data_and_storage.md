---
layout: default
title: SwiftUI Data & Storage
parent: SwiftUI
nav_order: 4
---

Trong SwiftUI thì có sử dụng khái niệm **single *source of truth***

## Managing user interface

Trong trường hợp cần sử dụng data trong nội bộ của View, thường là data dùng để hiển thị lên View hoặc trong các View có mối liên hệ với nhau, thì sử dụng State, StateObject hoặc Binding (gọi là Property Wrapper).

Những property được mark với các property wrapper trên thì về cơ bản là vòng đời của property sẽ gắn liền với vòng đời của View (hoặc Scene, App) mà nó được khai báo. Nên vòng đời của property sẽ tương ứng với vòng đời của View.

Chú ý, với những data có thể thay đổi thì mới define dưới dạng state, còn với những data chỉ hiển thị thì có thể define với `let`

Trong trường hợp muốn share data hoặc state từ View sang child view thì có thể sử dụng @Binding ở child view.

Ví dụ:

```swift
struct PlayButton: View {
  @Binding var isPlaying: Bool

  var body: some View {
    Button(action: {
      self.isPlaying.toggle()
    }) {
      Image(systemName: isPlaying ? "pause.circle" : "play.circle")
    }
  }
}
```

`PlayButton` là view mà sẽ thay đổi dựa vào property là `isPlaying`. Ở đây thì `PlayButton` không quản lý trạng thái của `isPlaying` mà sẽ phụ thuộc vào parent view.

```swift
struct PlayerView: View {
  ...    
  @State private var isPlaying: Bool = false
  
  var body: some View {      
      VStack {
        ...
        PlayButton(isPlaying: $isPlaying)
      }
  }
}
```

Như vậy, trạng thái của `PlayButton` sẽ phụ thuộc vào State variable `isPlaying` của `PlayerView`. Khi user toggle trạng thái của `PlayButton` thì thực tế là đang thay đổi giá trị `isPlaying` của `PlayerView`

## Managing model data in app

Với UIKit thì việc tạo 1 property để hold data cho màn hình là việc rất bình thường, data property này có thể là một array chứa data, 1 object của Model class, hoặc 1 object của Services class...

Tuy nhiên với SwiftUI thì mọi view của nó đều là struct, và mỗi khi view redraw thì các property khai báo trong view sẽ được reset về giá trị ban đầu, do đó việc hold data cho view trong SwiftUI không thể sử dụng cách khai báo như với UIKit.

Vì vậy, trong SwiftUI chúng ta sử dụng các property wrapper như là `@State`, `@StateObject`, `@Binding`, `@ObservedObject` để khai báo property sử dụng trên view. Khi property data thay đổi thì SwiftUI sẽ tự động vẽ lại view. Tuy nhiên việc sử dụng property wrapper như nào cho đúng thì cần phải nắm được bản chất thì dùng mới chính xác.

Với `@State` và `@Binding` thì đã được mô tả trong section [Managing user interface](#managing-user-interface)

`@StateObject` và `@ObservedObject` về cơ bản thì cách dùng khá giống với `@State` và `@Binding`. Tuy nhiên khác nhau ở chỗ là `@State` và `@Binding` là dùng cho value type trong khi `@StateObject` và `@ObservedObject` dùng cho reference type.

Sử dụng `@StateObject` để define các data model thuộc view, với những property define với `@StateObject` thì sẽ được keep value khi view redraw.

Nhắc lại, `@StateObject` chỉ sử dụng để define data model cho view. Trong trường hợp data được chia sẻ ở nhiều view khác nhau, như được khởi tạo tại một view và các view khác sử dụng dưới dạng reference thì dùng `@ObservedObject`.

`@ObservedObject` dùng để khai báo data model cho view mà bản thân view không quản lý, mà được reference từ view khác. Ví dụ data model được khai báo ở parent view và được share sang child view thì sử dụng `@ObservedObject`. Việc phân biệt mục đích và cách sử dụng `@StateObject`, `@ObservedObject` cần phải được nắm rõ vì nó khác nhau và có thể tiềm ẩn bug.

Ví dụ chúng ta có 2 view là `ParentView` và `ChildView`, với `ChildView` là view con của `ParentView`. Trong trường hợp `ChildView` khai báo property sử dụng `@ObservedObject` thì khi `ParentView` redraw, giá trị property của `ChildView` cũng sẽ bị reset, mặc dù property của `ChildView` không hề liên quan đến `ParentView` Trong trường hợp này, thì cần sử dụng `@StateObject`, khi đó cho dù `ParentView` có reload thì cũng không ảnh hưởng gì đến giá trị property của `ChildView`.

> Bổ sung sample source cho ví dụ này.

**Share an object throughout app**

Khi muốn share data xuyên suốt app, hoặc loại data mà có thể sử dụng ở nhiều màn hình, tuy nhiên các màn hình đấy không liên tục nhau, dẫn đến việc transfer data giữa các màn hình trở nên khó khăn (ví dụ data dùng ở màn hình A, D, trong khi flow màn hình là A -> B -> C -> D thì việc transfer data qua 2 màn hình không liên quan là B, C trở nên bất hợp lý) thì có thể cân nhắc việc sử dụng `@EnvironmentObject`.

Có thể suy nghĩ đến các trường hợp như Application State (dùng để quản lý trạng thái của app, các stack của flow màn hình) hoặc User State để lưu trữ data trạng thái của User, config của User, thì có thể sử dụng `@EnvironmentObject`.

Cách sử dụng:

Với data model sử dụng để làm Environment Object thì chỉ cần conform to protocol `ObservableObject`.

```swift
final class UserState: ObservableObject {
    @Published var loginState: UserLoginState = .notLoggedIn

    static let shared = UserState()

    private init() { }
}
```

Còn khi sử dụng thì có thể khai báo dạng:

```swift
struct HomeView: View {
    @EnvironmentObject var userState: UserState
    ...
}
```

Thông thường thì Environment Object được add vào app tại `App` hoặc tại main `View`
Ví dụ:

```swift
struct SwiftUIApp: App {
    let userState = UserState.shared

    var body: some Scene {
        WindowGroup {
            ContentView().environmentObject(userState)
        }
    }
}
```

hoặc

```swift
struct ContentView: View {
    let userState = UserState.shared
    
    var body: some View {
        MainView().environmentObject(userState)
    }
}
```

Có một vài lưu ý khi dùng Environment Object như sau

- Về cơ bản thì vòng đời của environment object khá là dài, và nó sẽ consume data liên tục trong suốt vòng đời của app, nên việc sử dụng nó sẽ tăng memory -> không lạm dụng environment object khi không thực sự cần thiết
- Environment Object được dùng ở toàn app hoặc trong một phạm vi khá lớn, dẫn đến việc rất dễ xảy ra sai sót logic do nhiều màn hình cùng sử dụng chung một data. Vì vậy khi dùng Environment Object cần phải review kỹ logic tránh sai sót.
- Environment Object nếu chưa được inject vào app thông qua modifier environmentObject() thì nếu sử dụng app sẽ bị crash.
