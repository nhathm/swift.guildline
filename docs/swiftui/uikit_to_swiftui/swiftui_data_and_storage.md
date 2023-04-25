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
