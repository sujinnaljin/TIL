# addSubview(_:)
Adds a view to the end of the receiver’s list of subviews.
```swift
func addSubview(_ view: UIView)
```
## view

The view to be added. After being added, this view **appears on top of any other subviews.**
## Discussion
This method establishes a **strong reference** to view and sets its next responder to the receiver, which is its new superview.

**Views can have only one superview. If view already has a superview and that view is not the receiver, this method removes the previous superview before making the receiver its new superview.**
