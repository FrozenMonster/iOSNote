* present一个VC后，该VC的nextResponder是它的presentingViewController

* 在horizontally compat(eg,iPhone portrait)下, modalPresentationStyle只能是Full Screen Style，其他style无效

* 在不知道VC在层级里具体位置时，可以用`showViewController:sender:`或者 `showDetailViewController:sender:`方法展示VC，这两个方法会找到合适的方式去做transition

* 可以通过访问VC的`transitionCoordinator`属性去在present的过程中做同步动画,具体方法是调用 `animateAlongsideTransition:completion:`或者`animateAlongsideTransitionInView:animation:completion:`这两个方法

* 在dismiss一个VC的时候,应该是该VC的presentingViewController作为caller.当然，也可以直接让该VC作为caller,UIKit会自动转发给该VC的presentingViewController去dismiss

* If you present several view controllers in succession, thus building a stack of presented view controllers, calling this method on a view controller lower in the stack dismisses its immediate child view controller and all view controllers above that child on the stack. When this happens, only the top-most view is dismissed in an animated fashion; any intermediate view controllers are simply removed from the stack. The top-most view is dismissed using its modal transition style, which may differ from the styles used by other view controllers lower in the stack.
eg : VC1 present VC2, VC2 present VC3, VC3 present VC4, 在调用VC2的dismiss时，会dismiss掉VC3和VC4，但是只有VC4有dismiss动画

-

####VC1的presentingViewController值有两种情况:
* VC2 直接 present VC1, VC1.presentingViewController = VC2
* VC3 是 VC1的最远的一个被present出来的祖先, VC2 present VC3,VC1.presentingViewController = VC2

####VC1的presentedViewController值有两种情况:
* VC1 直接 present VC2, VC1. presentedViewController = VC2
* VC3 是 VC1的最近一个present过VC的祖先, VC3 prsent VC2, VC1.presentedViewController = VC2