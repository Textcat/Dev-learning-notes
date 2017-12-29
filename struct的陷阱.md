# Struct 中的陷阱
#Programming/Swift
```swift
import Foundation
import Cocoa

class Printer:NSCopying {
var line = "Hello world"
var line2 = "Hello the great world"

}

struct PrinterManager {
var printer1 = Printer()

}

var manager = PrinterManager()
var newManager = manager

manager.printer1.line = "Hello the shitty world"
newManager.printer1.line
//"Hello the shitty world"
```
我们都知道，Swift 中，Struct 和 Class 的不同之处在于，Struct 不是 Reference类型。但需要注意的是，struct 的属性却可以是 Reference 类型，比如上面的例子，将 manager 赋值给 newManager 后，对 manager 中的 printer1属性进行更改，仍然会影响到 newManager。因此，在实际的开发中，必须要意识到这一问题，或是尽量不要讲 Class 和 Struct 混合使用，以减少不可控的麻烦。









