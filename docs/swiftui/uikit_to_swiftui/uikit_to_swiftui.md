---
layout: default
title: UIKit to SwiftUI
parent: SwiftUI
nav_order: 2
---

**UIKit to SwiftUI equivalent**
> Note: iOS 14+/15+

SwiftUI sẽ không còn khái niệm ViewController mà thay thế bằng View, và việc xử lý hiển thị, update, handle data trên View sẽ được thực hiện bởi Combine framework, mô hình MVVM.

# Views & Controls

| UIKit                   | SwiftUI | Note |
| :---                    | :---      | :--- |
| 1. UILabel                 | Text, Label     | Text thì tương tự label bên UIKit, còn Label thì có thể hiển thị text và image cạnh nhau    |
| UITabbar                | TabView       |       |
| UITabBarItem            |TabView| .tabItem của TabView |
| UITextField             | TextField | (isSecureTextEntry) -> SecureField |
| UITextView              | TextEditor |       |
| UITableView             | List | có thể sử dụng VStack & Form |
| UINavigationBar         | NavigationView |       |
| UINavigationItem        | ToolbarItem |       |
| UIBarButtonItem         | NavigationView | .navigationBarItems của NavigationView |
| UICollectionView        | LazyVGrid and LazyHGrid |       |
| UIStackView             | HStack, LazyHStack | .axis == .Horizontal |
| UIStackView             | VStack, LazyVStack | .axis == .Vertical |
| UIScrollView            | ScrollView |       |
| UIActivityIndicatorView | ProgressView | CircularProgressViewStyle |
| UIImageView             | Image |       |
| UIPickerView            | Picker |       |
| 17. UIButton            | [Button, Link](/docs/swiftui_components/swiftui_buttons/swiftui_button_index.html) | Link sẽ mở app khác hoặc default web browser |
| UIDatePicker            | DatePicker |       |
| UIPageControl           | TabView | PageTabViewStyle style, .indexViewStyle |
| UIProgressView          | ProgressView |       |
| UISegmentedControl      | Picker | SegmentedPickerStyle |
| UISlider                | Slider |       |
| UIStepper               | Stepper |       |
| UISwitch                | Toggle |       |
| UIToolBar               | NavigationView | .toolbar |
| MKMapView               | import MKMapView |       |
| Modal view controller   | sheet(isPresented:onDismiss:content:) | sheet, cover, or popover |
| Alert view              | alert(_:isPresented:actions:) | alert(_:isPresented:actions:message:) |
