---
layout: default
title: Swift recommended implementations
parent: Swift Tips
nav_order: 1
---

Swift là ngôn ngữ sinh sau đẻ muộn nên nó học tập rất nhiều cái hay từ các ngôn ngữ khác.  
Nếu các bạn dev iOS chuyển từ Objective-C sang Swift mà vẫn giữ style code của Objective-C thì sẽ rất không tốt.  
Do đó, hãy cố gắng code Swift sao cho nó `Swift` nhất có thể.

# Navigation Structure
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


## `for case`  

Trong trường hợp có 1 array, ta không cần phải `for` từng object và check type, mà có thể check type luôn trong `for case` luôn.

```swift
let obj1 = ClassA()
let obj2 = ClassB()
let obj3 = ClassC()

let listObjs: [Any] = [obj1, obj2, obj3]

// Instead of:
for obj in listObjs {
    if let obj = obj as? ClassA {
        // do something
    }
}

// Do:
for case let obj as ClassA in listObjs {
    // do something
}
```

Note: khi test thử thì cách bên trên có performance tốt hơn, code chạy xong nhanh hơn tuy nhiên không đáng kể.
Sample: [sample.swift](sample-forcase.swift)

Note: Update 2023, cách bên dưới đã nhanh hơn. XCode 14.2, swift 5.7.2, Apple M1

## `enumerated()`  

Khi muốn lấy index và value của object trong array, sử dụng `enumerated()`

```swift
let obj1 = ClassA()
let obj2 = ClassA()
let obj3 = ClassA()

let listObjs: [ClassA] = [obj1, obj2, obj3]

// Instead of:
for i in 0..<listObjs.count {
    let obj = listObjs[i]
    // Do something
}

// Do:
for (i, obj) in listObjs.enumerated() {
    // Do something with obj
}
```

## `first(where: )`

Khi muốn tìm object đầu tiên trong array thỏa mãn điều kiện nào đó thì sử dụng `fist(where:)`  

```swift
// Instead of:
if let element = listObjs.filter { $0.title.contains(searchString) }.first {
    // do something
}

// Do:
if let element = listObjs.first(where: { $0.title.contains(searchString) }) {
    // do something
}
```  

## `forEach`  

Dùng for each khi muốn apply logic đơn giản nào đó cho các phần tử trong array

```swift
// Instead of:
for obj in listObjs {
    doSomething(with: obj)
}

// Do:
listObjs.forEach {
    doSomething(with: $0)
}
```

## `keyPaths`  

Cân nhắc việc sử dụng `keyPaths` với các closure phổ biến như filter, maps, compactMaps...

```swift
// Instead of:
let filteredArray = array.filter { $0.isActive }
let titles = filteredArray.map { $0.title }

// Do:
let filteredArray = array.filter(\.isActive)
let titles = filteredArray.map(\.title)

// Or even:
let titles = array
    .filter(\.isActive)
    .map(\.title)
```

Note: `keyPaths` chỉ có thể sử dụng luôn với property của struct/class, không thể apply điều kiện.
Ví dụ: `.map(\.title.count > 5)` sẽ compile error  

## `guard`  

Sử dụng `guard` hợp lý sẽ khiến đoạn code đẹp hơn và dễ đọc hơn.

```swift
// Instead of:
func sayHi(name: String?) {
    if let name = name else {    
        print("Hello \(name)")
    }        
}

// Do:
func sayHi(name: String?) {
    guard let name = name else {    
        return
    }    
    print("Hello \(name)")
}
```

## `defer`  

Source code trong `defer` block sẽ được call trước khi method kết thúc.  
Khi dùng defer, không cần biết logic của function bạn viết là gì, thời điểm nó return ra sao, đoạn code nằm trong `defer` sẽ luôn luôn được gọi.

```swift
// We can do this
func writeLog(_ log: String) {
    let file = openFile()

    if log != nil {
        file.write(log)
    }

    if conditionX {
        file.write("conditionX enable")
        closeFile(file)
        return
    }

    if conditionY {
        file.write("conditionY enable")
        closeFile(file)
        return
    }    
}

// Or this
func writeLog(_ log: String) {
    let file = openFile()
    
    defer {
        closeFile(file)
    }

    if log != nil {
        file.write(log)
    }

    if conditionX {
        file.write("conditionX enable")
        return
    }

    if conditionY {
        file.write("conditionY enable")
        return
    }    
}
```

Source: https://medium.com/geekculture/when-you-write-code-in-swift-write-code-in-swift-abdac43d44fa  
