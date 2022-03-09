<img align="right"
src="https://user-images.githubusercontent.com/99466907/157395662-2ceb1e46-529b-460a-8fce-12dadcf1d53a.png"
alt="Amity Social Cloud SDKs" width="50%" />

<img align="left"
src="https://user-images.githubusercontent.com/100549875/155947065-3cc4291c-d600-40a1-bc49-4ff5e9b9d1be.svg"
alt="Amity logo" width="120px" />

<br />
<br />

# Amity Social Cloud SDKs
Boost app engagement and grow your user base with plug-and-play social
features. Amity modules are <b>ready to use</b> — the only things left
to do are integration and frontend.

Learn more about Amity here: [amity.co→](https://amity.co/)

<br />
<br />

# VideoLiveChat Demo App

## Stack Used
- Swift [Official Document](https://developer.apple.com/swift/resources/)
- Amity UIKit and Chat SDK [Official Document](https://docs.amity.co/uikit/ios-1/overview)
- MenuItemKit [Official Document](https://github.com/cxa/MenuItemKit)

## How to Run Project Locally
### Do a pod install
```
pod install
```
### Open .xcworkspace file on Xcode, build, and run the project

## Amity Chat SDK Installation Guide
### Setup steps for the SDK
1. Add the SDK to your repository

Add the SDK to your repository via **Carthage** or **Cocoapods**
```
# Example for Cocoapods. Add this line to your podfile
pod 'UpstraUIKit', '1.11.4', :source => "https://github.com/EkoCommunications/EkoMessagingSDKUIKit.git"
```
More details on installation method, please refer to here: [Getting Started on iOS](https://docs.amity.co/uikit/ios-1/getting-started)

2. Create new SDK Instance with your API Key

Before using the Chat SDK, you will need to create a new SDK instance with your API key (find it via the Admin Panel under settings). For this project, we have set it up where you only need to put your API key in **ClientManager.swift**. LoadingViewController will use the method to setup everything.

```swift
import UpstraUIKit
import EkoChat

enum Credentials {
    static let ascApiKey = "YOUR_API_KEY"
    ...
}
```
File: Managers/ClientManager.swift

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    navigationController?.navigationBar.isHidden = true // hide navigation bar
    
    ClientManager.shared.setupUpstraUIKit()
    ClientManager.shared.completionHandler = { [weak self] in
        // This block get called after connection completed
        print("EkoClient successfully connected")
        ...
    }
}
```
File: Modules/Loading/ViewController/LoadingViewController.swift

3. Register a session for your device with User ID

ClientManager already set it up for you, you only need to provide your desired User ID

```swift
import UpstraUIKit
import EkoChat

enum Credentials {
    static let ascApiKey = "YOUR_API_KEY"
    static let userId = "sampleiOSUser"
    static let displayName = UIDevice.current.name
    ...
}
```
File: Managers/ClientManager.swift

4. Join a channel
Before the user can see, send, and receive messages, the user needs to be in a channel first. For this project, you can either specify the channel ID in the **ClientManager.swift** file or pass in the channel ID directly in **LiveStreamViewController.swift**. If you decide to pass in the channel ID directly, be sure to pass that into **initializeRepositories** function in the **ClientManager.swift** file as well.

```swift
// MARK: - Variables
private let messageListManager = MessageListManager(channelId: CHANNEL_ID_HERE)
```
File: LiveStreamViewController.swift

```swift
// MARK: - Private methods
    
    private func initilizeRepositories() {
        channelRepository = EkoChannelRepository(client: UpstraUIKitManager.client)
        joinChannel(channelId: Credentials.liveChannelId)
    }

    private func joinChannel(channelId: String) {
        guard let channelRepository = self.channelRepository else { return }
        let channelObject: EkoObject<EkoChannel> = channelRepository.joinChannel(channelId)

        token = channelObject.observe { channelObject, error in
            print("Channel joined successfully.")
        }
    }
```
File: ClientManager.swift

5. Retrieve messages in a channel
We have setup the project where **MessageListManager.swift** will fetch the messages in **fetchMessages** function.

```swift
func fetchMessages() {
    messagesCollection = messageRepository.messages(withChannelId: channelId, filterByParentId: false, parentId: nil, reverse: true)
    messagesNotificationToken = messagesCollection?.observe { [weak self] (collection, _, error) in
        self?.prepareData(with: collection)
    }
}
```
File: Managers/MessageListManager.swift

LiveStreamViewController will setup everything by using the methods inside the above file. The setup is highly straightforward, please feel free to jump around by looking at its definition.

## UI Modularize

Live Chat UI is divided into three main UI classes as follows: `MessageTableView`, `MessageComposeBarView` and `MessageReplyView`.
Please see image below for further reference.

![image](https://user-images.githubusercontent.com/74768384/118767327-c50dd780-b8a7-11eb-8358-75d277cf1e02.png)

### How to use them
1. Create the view you desire on the storyboard
2. Link the respective view to the class you want to use (Select the view > identity inspector > put the name of the class inside the class input field under custom class)
3. Each class will populate every element, including the constraints automatically to your view on the storyboard

For more example on how you may set this up, please see Main.storyboard and take a look at LiveStreamViewController or SampleViewController.

### Chat message options
1. Send message
2. Reply
3. React
4. Flag/Report

<br />
<br />

<img align="left"
src="https://user-images.githubusercontent.com/100549875/156137190-46c08727-042b-4f3d-858b-d50868ebb0b3.png"
alt="Amity Social Cloud SDKs" width="50%" />

## Resources

**Developer Portal** <br />
Learn about building, deploying, and managing Amity Social Cloud. <br />
[Portal→](https://www.amity.co/developer-portal)

**Documentation** <br />
Everything you need to integrate Amity Social Cloud. <br />
[Docs→](https://docs.amity.co/)

**Developer Kits** <br />
Explore Amity Social Cloud UI Kits and Template Apps. <br />
[Developer Kits→](https://www.amity.co/developer-kits)

**Community** <br />
Join the community of Amity Social Cloud developers. <br />
[Community→](https://community.amity.co/)

**FAQ** <br />
Get the answers to the most asked questions. <br />
[FAQ→](https://www.amity.co/faq)

<br />
<br />
<br />
<br />
<br />
<br />

## Amity Chat SDK
Amity Chat SDK is an easy-to-integrate solution that enables
high-performing chat services on your app. From one-on-one to
large-scale group messaging, power them with <b>Amity Chat SDK</b>,
built with <b>messaging service APIs</b> to ignite connections and
open discussions.

Learn more about Amity Chat on [our
website→](https://www.amity.co/products/amity-chat) or view the Amity
Chat [Docs→](https://docs.amity.co/chat)

<br />

## Amity Social SDK
Get in-app communities up and running using Amity Social SDK. Enable
<b>plug-and-play social features using supercharged social APIs</b>
and see preference-based groups thrive within your platform.

Learn more about Amity Chat on [our
website→](https://www.amity.co/products/amity-social) or view the
Amity Social [Docs→](https://docs.amity.co/social)

<br />

## Amity Video SDK
The Amity Video SDK, powered by <b>video APIs</b>, elevates your
application's user experience by adding interactive features such as
<b>in-app Stories and Live Streaming</b>. Engage your users with
captivating, memorable virtual events to participate in along with
other viewers from around the world.

Learn more about Amity Chat on [our
website→](https://www.amity.co/products/amity-video) or view the Amity
Video [Docs→](https://docs.amity.co/video)

<br />

## About Amity
The future is social — and we at Amity are on a mission to make social
experiences more accessible than ever. Amity Social Cloud allows
companies to easily integrate plug-and-play social features into their
apps and digital channels to drive engagement, build communities, and
grow revenue.

<b>🟢 We're hiring!</b> View all [open positions→](https://www.amity.co/careers)

<br />

## License
Public Framework. Copyright © 2022 Amity.
