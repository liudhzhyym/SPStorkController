# SPStorkController

<p float="left">
    <img align="left" src="https://github.com/IvanVorobei/SPStorkController/blob/master/Resources/Preview.gif" width="440"/>
</p>

<p><h3>About</h3>
Transition controller <b>as in Apple Music, Podcasts and Mail</b> apps. Customizable height. Simple adding close button and centering arrow indicator.
<br><br>
You can download example <a href="https://itunes.apple.com/app/id1446635818">Debts - Spending tracker</a> app from AppStore. Alert you can find in <a href="https://github.com/IvanVorobei/SPAlert">SPAlert</a> project. If you want to <b>buy source code</b> of app in preview, please, go to <a href="https://xcode-shop.com">xcode-shop.com</a>.
<br><br>
If you like the project, do not forget to <b>put star ★</b> or donate:<br><br>

[![Patron](https://github.com/IvanVorobei/SPPermission/blob/master/Resources/Patron%20Button.svg)](https://patreon.com/ivanvorobei)

See project's backers in <a href="https://github.com/IvanVorobei/SPStorkController#sponsors">Sponsors</a> section.</p>

## Navigate

- [Requirements](#requirements)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Usage](#usage)
    - [Light StatusBar](#light-statusbar)
    - [Custom Height](#custom-height)
    - [Close Button](#close-button)
    - [Indicator](#indicator)
    - [Dismissing](#dismissing)
    - [Corner Radius](#corner-radius)
    - [Haptic](#haptic)
    - [Snapshots](#snapshots)
    - [Add Navigation Bar](#add-navigation-bar)
    - [Working with UIScrollView](#working-with-uiscrollview)
    - [UITableView & UICollectionView](#working-with-uitableview--uicollectionview)
    - [Delegate](#delegate)
- [Video Tutorial](#video-tutorial)
- [Sponsors](#sponsors)
- [Other Projects +gif](#my-projects)
    - [SPAlert](#spalert)
    - [SPLarkController](#splarkcontroller)
    - [SPPermission](#sppermission)
- [License](#license)
- [Contact or Order Develop](#contact)

## Requirements

Swift 4.2 & **5.0**. Ready for use on iOS 10+

## Installation

### CocoaPods:

[CocoaPods](https://cocoapods.org) is a dependency manager for Cocoa projects. For usage and installation instructions, visit their website. To integrate `SPStorkController` into your Xcode project using CocoaPods, specify it in your `Podfile`:

```ruby
pod 'SPStorkController'
```

### Carthage

[Carthage](https://github.com/Carthage/Carthage) is a decentralized dependency manager that builds your dependencies and provides you with binary frameworks. To integrate `SPStorkController` into your Xcode project using Carthage, specify it in your `Cartfile`:

```ogdl
github "IvanVorobei/SPStorkController"
```

### Manually

If you prefer not to use any of the aforementioned dependency managers, you can integrate `SPStorkController` into your project manually. Put `Source/SPStorkController` folder in your Xcode project. Make sure to enable `Copy items if needed` and `Create groups`.

## Quick Start

Create controller and call func `presentAsStork`:

```swift
import UIKit
import SPStorkController

class ViewController: UIViewController {
    
    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated)

        let controller = UIViewController()
        self.presentAsStork(controller)
    }
}
```

If you want customize controller (remove indicator, set custom height and other), create controller and set `transitioningDelegate` to `SPStorkTransitioningDelegate` object. Use `present` or `dismiss` functions:

```swift
let controller = UIViewController()
let transitionDelegate = SPStorkTransitioningDelegate()
controller.transitioningDelegate = transitionDelegate
controller.modalPresentationStyle = .custom
controller.modalPresentationCapturesStatusBarAppearance = true
self.present(controller, animated: true, completion: nil)
```

Please, do not init `SPStorkTransitioningDelegate` like this:

```swift
controller.transitioningDelegate = SPStorkTransitioningDelegate()
```

You will get an error about weak property.

## Usage

### Light StatusBar

To set light status bar for presented controller, use `preferredStatusBarStyle` property. Also set `modalPresentationCapturesStatusBarAppearance`. See example:

```swift
import UIKit

class ModalViewController: UIViewController {
    
    override var preferredStatusBarStyle: UIStatusBarStyle {
        return .lightContent
    }
}
```

### Custom Height

Property `customHeight` sets custom height for controller. Default is `nil`:

```swift
transitionDelegate.customHeight = 350
```

### Close Button

Property `showCloseButton` added circle button with dismiss action. Default is `false`:
```swift
transitionDelegate.showCloseButton = false
```

### Indicator

On the top of controller you can add arrow indicator with animatable states. It simple configure.
Property `showIndicator` shows or hides top arrow indicator. Default is `true`:

```swift
transitionDelegate.showIndicator = true
```

Property Parameter `indicatorColor` for customize color of arrow. Default is `gray`:

```swift
transitionDelegate.indicatorColor = UIColor.white
```

Property `hideIndicatorWhenScroll` shows or hides indicator when scrolling. Default is `false`:

```swift
transitionDelegate.hideIndicatorWhenScroll = true
```

### Dismissing

You can also configure events that will dimiss the controller.
Property `swipeToDismissEnabled` enables dismissal by swipe gesture. Default is `true`:

```swift
transitionDelegate.swipeToDismissEnabled = true
```

Property `translateForDismiss` sets how much need to swipe down to close the controller. Work only if `swipeToDismissEnabled` is true. Default is `240`:

```swift
transitionDelegate.translateForDismiss = 100
```

Property `tapAroundToDismissEnabled` enables dismissal by tapping parent controller. Default is `true`:

```swift
transitionDelegate.tapAroundToDismissEnabled = true
```

### Corner Radius

Property `cornerRadius` for customize corner radius of controller's view. Default is `10`:

```swift
transitionDelegate.cornerRadius = 10
```

### Haptic

Property `hapticMoments` allow add taptic feedback for some moments. Default is `.willDismissIfRelease`:

```swift
transitionDelegate.hapticMoments = [.willPresent, .willDismiss]
```

### Snapshots

The project uses a snapshot of the screen in order to avoid compatibility and customisation issues. Before controller presentation, a snapshot of the parent view is made, and size and position are changed for the snapshot. Sometimes you will need to update the screenshot of the parent view, for that use static func:

```swift
SPStorkController.updatePresentingController(modal: controller)
```

and pass the controller, which is modal and uses `SPStorkTransitioningDelegate`.

### Add Navigation Bar

You may want to add a navigation bar to your modal controller. Since it became impossible to change or customize the native controller in swift 4 (I couldn’t even find a way to change the height of the bar), I had to recreate navigation bar from the ground up. Visually it looks real, but it doesn’t execute the necessary functions:

```swift
import UIKit
import SPFakeBar

class ModalController: UIViewController {
    
    let navBar = SPFakeBarView(style: .stork)
        
    override func viewDidLoad() {
        super.viewDidLoad()

        self.view.backgroundColor = UIColor.white

        self.navBar.titleLabel.text = "Title"
        self.navBar.leftButton.setTitle("Cancel", for: .normal)
        self.navBar.leftButton.addTarget(self, action: #selector(self.dismissAction), for: .touchUpInside)

        self.view.addSubview(self.navBar)
    }
}
```

You only need to add a navigation bar to the main view, it will automatically layout. Use style `.stork` in init of `SPFakeBarView`. Here is visual preview with Navigation Bar and without it:

<img src="https://github.com/IvanVorobei/SPStorkController/blob/master/Resources/Navigation%20Bar.jpg"/>

To use it, you need to install [SPFakeBar](https://github.com/IvanVorobei/SPFakeBar) pod: 

```ruby
pod 'SPFakeBar'
```

### Working with UIScrollView

If you use `UIScrollView` (or UITableView & UICollectionView) on controller, I recommend making it more interactive. When scrolling reaches the top position, the controller will interactively drag down, simulating a closing animation. Also available close controller by drag down on `UIScrollView`. To do this, set the delegate and in the function `scrollViewDidScroll` call:

```swift
func scrollViewDidScroll(_ scrollView: UIScrollView) {
    SPStorkController.scrollViewDidScroll(scrollView)
}
```

### Working with UITableView & UICollectionView

Working with a collections classes is not difficult. In the `Example` folder you can find an implementation. However, I will give a couple of tips for making the table look better.

Firstly, if you use `SPFakeBarView`, don't forget to set top insets for content & scroll indicator. Also, I recommend setting bottom insets (it optional):

```swift
tableView.contentInset.top = self.navBar.height
tableView.scrollIndicatorInsets.top = self.navBar.height
```

Please, also use `SPStorkController.scrollViewDidScroll` function in scroll delegate for more interactiveness with your collection or table view.

### Delegate

You can check events by implement `SPStorkControllerDelegate` and set delegate for `transitionDelegate`:

```swift
transitionDelegate.storkDelegate = self
```

Delagate has this functions: 

```swift
protocol SPStorkControllerDelegate: class {
    
    optional func didDismissStorkBySwipe()
    
    optional func didDismissStorkByTap()
}
```

### Modal presentation of other controller

If you want to present modal controller on SPStorkController, please set:

```swift
controller.modalPresentationStyle = .custom
```

It’s needed for correct presentation and dismissal of all modal controllers.

### Stop scroll

`SPStorkController` use snapshots. If the parent controller scrollings and you try to show `SPStorkController`, you will see how it froze, and in a second its final position is updated. I recommend to stop scrolling force:

```swift 
scrollView.setContentOffset(self.contentOffset, animated: false)
```

## Video Tutorial

You can see how to use `SPStorkController` and how to customize it [in this video](https://youtu.be/wOTNGswT2-0). For English speakers I’ve added subtitles, don’t forget to turn them on:

[![Tutorial on YouTube](https://github.com/IvanVorobei/SPStorkController/blob/master/Resources/YouTube.jpg)](https://youtu.be/wOTNGswT2-0)

## Sponsors

Support me with a monthly donation and help me continue activities: [Become a sponsors](https://www.patreon.com/ivanvorobei)

<a href="https://github.com/zubara21/" target="_blank"><img src="https://github.api.ivanvorobei.by/sponsors/zubara21.jpg" width="100"></a>
<a href="https://github.com/sparrowganz/" target="_blank"><img src="https://github.api.ivanvorobei.by/sponsors/sparrowganz.jpg" width="100"></a>
<a href="https://github.com/shatk0vskiy/" target="_blank"><img src="https://github.api.ivanvorobei.by/sponsors/shatk0vskiy.jpg" width="100"></a>
<a href="https://www.patreon.com/ivanvorobei" target="_blank"><img src="https://github.api.ivanvorobei.by/sponsors/add.jpg" width="100"></a>

## My projects

<p float="left">
    <img align="left" src="https://github.com/IvanVorobei/SPAlert/blob/master/Resources/Preview-Done.gif" width="220"/>
</p>

### SPAlert

<p><a href="https://github.com/IvanVorobei/SPAlert" target="_blank">SPAlert</a> is <b>popup from Apple Music & Feedback in AppStore</b>. Contains `Done` & `Heart` presets. `Done` preset present with animation. I clone Apple's alerts as much as possible. You can find this alerts in AppStore after feedback, after added song to library in Apple Music. I am also add alert without icon, as simple message.
<br><br>
You can download example <a href="https://itunes.apple.com/app/id1446635818">Debts - Spending tracker</a> app from AppStore. If you want to <b>buy source code</b> of app in preview, please, go to <a href="https://xcode-shop.com">xcode-shop.com</a>.</p>

---

<p float="left">
    <img align="left" src="https://github.com/IvanVorobei/SPLarkController/blob/master/Resources/Preview.gif" width="220"/>
</p>

### SPLarkController

<p><a href="https://github.com/IvanVorobei/SPLarkController" target="_blank">SPLarkController</a> make <b>settings screen</b> for application. You can add <b>buttons and switches</b>. The amount cells is not limited. You can start using project with just two lines of code and easy customisation. For implement settings as in preiew, see section <a href="https://github.com/IvanVorobei/SPLarkController#settings-controller">Settings Controller</a>.<br><br>
You can download example app <a href="https://itunes.apple.com/app/id1453325619" target="_blank">Code - Learn Swift & Design</a> from AppStore. If you want to <b>buy source code</b> of app this app, please, go to <a href="https://xcode-shop.com" target="_blank">xcode-shop.com</a><p>

---

<p float="left">
    <img align="left" src="https://github.com/IvanVorobei/SPPermission/blob/master/Resources/Preview.gif" width="220"/>
</p>

### SPPermission

<p><a href="https://github.com/IvanVorobei/SPPermission" target="_blank">SPPermission</a> allow request permissions <b>with native dialog</b> UI and interactive animations. Also you can request permissions without dialog. Check state any permission. You can start using this project with just two lines of code and easy customisation.
<br><br>
If your app uses SPPermission, write me. I am adding you app as example of usage.

## License
`SPStorkController` is released under the MIT license. Check `LICENSE.md` for details.

## Contact
If you need any application or UI to be developed, message me at hello@ivanvorobei.by or via [telegram](https://t.me/ivanvorobei). I develop iOS apps and designs. I use `swift` for development. To request more functionality, you should create a new issue. You can see my [apps in AppStore](https://itunes.apple.com/developer/id1446635817).
