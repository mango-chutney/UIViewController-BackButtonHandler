# UIViewController+BackButtonHandler

Get a native "back" button icon (chevron) in a presented view controller.

Install with Cocoapods:

```
pod 'UIViewController+BackButtonHandler',
      :git => 'git@github.com:mango-chutney/UIViewController-BackButtonHandler.git',
      :branch => 'cocoapods'
```

If you're using Swift, you're also going to need a bridging header:

```
// BridgingHeader.h
#ifndef BridgingHeader_h
#define BridgingHeader_h

#import <UIViewController+BackButtonHandler/UIViewController+BackButtonHandler.h>

#endif /* BridgingHeader_h */
```

Then you can do something like this:

```
let myViewController = UIViewController()
let myNavigationViewController = UINavigationController(rootViewController: UIViewController())
climbNavigationViewController.pushViewController(myViewController, animated: true)
presentViewController(myNavigationViewController, animated: true, completion: nil)
```

And in `myViewController`, create a function `navigationShouldPopOnBackButton() -> Bool`:

```
func navigationShouldPopOnBackButton() -> Bool {
  self.navigationController?.dismissViewControllerAnimated(true, completion: nil)
  return false
}
```

If you also want to get rid of or overwrite the text for the back button,
consider subclassing the view controller which will be passed as
`rootViewController`:

```
//  PresentedViewControllerWrapper.swift

import UIKit

class PresentedViewControllerWrapper: UIViewController {

  convenience init() {
    self.init(nibName: nil, bundle: nil)
  }

  override init(nibName nibNameOrNil: String?, bundle nibBundleOrNil: NSBundle?) {
    super.init(nibName: nibNameOrNil, bundle: nibBundleOrNil)
    // override back bar button text (what it would say under the back button for the next view controller on the stack)
    self.navigationItem.backBarButtonItem = UIBarButtonItem(title: "", style: .Plain, target: nil, action: nil)
  }

  required init?(coder aDecoder: NSCoder) {
    fatalError("init(coder:) has not been implemented")
  }
}
}
```
