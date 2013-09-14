![UAAppReviewManager](https://github.com/coneybeare/UAAppReviewManager/blob/master/Screenshots/iOS.png?raw=true "iOS")
![UAAppReviewManager](https://github.com/coneybeare/UAAppReviewManager/blob/master/Screenshots/OSX.png?raw=true "OS X")

UAAppReviewManager is a simple and lightweight App review prompting tool for iOS and Mac App Store apps. It's Appirater all grown up, ready for primetime.

## Why UAAppReviewManager?

The average end-user will only write a review if something is wrong with your App. This leads to an unfairly negative skew in the ratings, when the majority of satisfied customers don’t leave reviews and only the dissatisfied ones do. In order to counter-balance the negatives, UAAppReviewManager prompts the user to write a review, but only after the developer knows they are satisfied. For example, you may only show the popup if the user has been using it for more than a week, and has done at least 5 significant events (the core functionality of your App). The rules are fully customizable for your App and easy to setup.

Here are just a few of the things that make UAAppReviewManager better than the other rating frameworks and repos:

##### iOS and OS X Support

Many developers publish apps for both iOS and OS X. Out of the box, UAAppReviewManager supports iOS and OS X apps that are sold through the Mac App Store. The API is the same for both with the exception of a few iOS specific methods, described in Usage.

##### Fully Configurable at Runtime.

UAAppReviewManager is fully configurable, even at runtime. This means that the prompt you display can be dynamic, based on the end-user's score or status. The rules that govern how and when it should be shown can all be set the same way, allowing you to have the most control over the presentation and timing of your review prompt.

##### Default Localizations for 27 Languages

If you choose to use the default UAAppReviewManager strings for your app, you will get the added benefit of localization in 27 languages. Otherwise, customization is easy, and overriding the localization strings is a piece of cake, simply by including your own strings files and letting UAAppReviewManager know.

##### iTunes Affiliate Codes

If you are an iTunes Affiliate, you can easily setup UAAppReviewManager to use your code and campaign. Full disclosure: If you aren't an iTunes Affiliate, the default code used in the app is the author's. It is better to have somebody's code rather than nobody's, so please leave it at the default setting if you aren't going to set one yourself. Think of it as a tiny tip for creating and open-sourcing UAAppReviewManager.

##### Ready For Primetime

UAAppReviewManager is clean code, well documented and well organized. It is easy to understand the logic flow and the purpose of each method. It doesn't mix logic up randomly between Class methods and Instance methods. It's API is clean and predictable.


## Installation

Installation is made simple with [Cocoapods](http://cocoapods.org/). If you want to do it the old fashioned way, just add `UAAppReviewManager.h`, `UAAppReviewManager.m` and the Localization folder into your project.

    pod 'UAAppReviewManager', '~> 0.1.1'

Then, simply place this line in any file that accesses UAAppReviewManager.

    #import <UAAppReviewManager.h>

UAAppReviewManager works on iOS 5.1 and up, and OS X 10.7 and up.
   
## Usage

### Basic Configuration

UAAppReviewManager includes sensible defaults as well as reads data from your localized, or unlocalized `info.plist` to set itself up. While everything is configurable, the only **required** item to configure is your App Store ID. This call is the same for iOS and Mac apps.

    [UAAppReviewManager setAppID:@"12345678"];
    
Aside from the `appID` configuration, you also have to let UAAppReviewManager know when the app is being used as this determines when the rating prompt can fire.

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        [UAAppReviewManager appLaunched:YES];
	    return YES;
    }
    
    - (void)applicationWillEnterForeground:(UIApplication *)application {
        [UAAppReviewManager appEnteredForeground:YES];
    }
    
### Custom Configuration

Optionally, if you are using significant events in your app to track when the user does something of significance, add this line to any place where this event happens, such as a `levelDidFinish` method, or `userDidUploadPhoto` method.
 
    [UAAppReviewManager userDidSignificantEvent:YES]

In order for this to mean anything to UAAppReviewManager, you also have to set the threshold for significant events. Typically, this, and other logic configuration settings, should be done before the `appLaunched:` call in your Application Delegate

    [UAAppReviewManager setSignificantEventsUntilPrompt:5];
    
As mentioned above, the `appID` is the only required item to configure. It is used to generate the URL that will link to the page. Most often, this is configured to the App that is currently running, but there may be an instance where you want to set it to another app, such as in an App that reviews other Apps.

    + (NSString *)appID;
    + (void)setAppID:(NSString *)appId;


##### Display Strings

The `appName` is used in several places on the review prompt popup. It can be configured here to customize your message without losing any of the default localizations. By default, UAAppReviewManager will read the value from your localized, or unlocalized `info.plist`, but you can set it specifically if you want.

    + (NSString *)appName;
    + (void)setAppName:(NSString *)appName;

The `reviewTitle` is the title to use on the review prompt popup. It's default value is a localized "Rate <appName>", but you can set it to anything you want.

    + (NSString *)reviewTitle;
    + (void)setReviewTitle:(NSString *)reviewTitle;

The `reviewMessage` is the message to use on the review prompt popup. It's default value is a localized "If you enjoy using <appName>, would you mind taking a moment to rate it? It won't take more than a minute. Thanks for your support!", but you can change it specifically if you want. However, if you do change it, you will need to provide your own localization strings as shown farther down below.

    + (NSString *)reviewMessage;
    + (void)setReviewMessage:(NSString *)reviewMessage;

The `cancelButtonTitle` is the button title to use on the review prompt popup for the "Cancel" action. Its default value is a localized "No, Thanks"

    + (NSString *)cancelButtonTitle;
    + (void)setCancelButtonTitle:(NSString *)cancelButtonTitle;

The `rateButtonTitle` is the button title to use on the review prompt popup for the "Rate" action. Its default value is a localized "Rate <appName>"

    + (NSString *)rateButtonTitle;
    + (void)setRateButtonTitle:(NSString *)rateButtonTitle;

The `remindButtonTitle` is the button title to use on the review prompt popup for the "Remind" action. Its default value is a localized "Remind me later"

    + (NSString *)remindButtonTitle;
    + (void)setRemindButtonTitle:(NSString *)remindButtonTitle;

##### Logic

The `daysUntilPrompt` configuration determines how many days the users will need to have the same version of your App installed before they will be prompted to rate it. Its default is 30 days.

    + (NSUInteger)daysUntilPrompt;
    + (void)setDaysUntilPrompt:(NSUInteger)daysUntilPrompt;

The `usesUntilPrompt` configuration determines how many times the user will need to have 'used' the same version of you App before they will be prompted to rate it. Its default is 20 uses.


    + (NSUInteger)usesUntilPrompt;
    + (void)setUsesUntilPrompt:(NSUInteger)usesUntilPrompt;

An example of a 'use' would be if the user launched the app, or brings it to the foreground. You tell UAAppReviewManager about these events using the two methods:
    
    [UAAppReviewManager appLaunched:]
    [UAAppReviewManager appEnteredForeground:]
 

As discussed briefly above, the `significantEventsUntilPrompt` configuration determines how many "significant events" the user will need to have before they will be prompted to rate the App. It defaults to 0 significant events.

    + (NSUInteger)significantEventsUntilPrompt;
    + (void)setSignificantEventsUntilPrompt:(NSInteger)significantEventsUntilPrompt;

A significant event can be anything you want to be in your app. In a telephone app, a significant event might be placing or receiving a call. In a game, it might be beating a level or a boss. This is just another layer of filtering that can be used to make sure that only the most loyal of your users are being prompted to rate you on the app store. If you leave this at a value of 0 (default), then this won't be a criterion used for rating. To tell UAAppReviewManager that the user has performed a significant event, call the method:

    [UAAppReviewManager userDidSignificantEvent:];

The `daysBeforeReminding` configuration determines how many days UAAPpReviewManager will wait before reminding the user to rate again, should they select the "Remind Me Later" option on the first alert. It defaults to 1 day.

    + (NSUInteger)daysBeforeReminding;
    + (void)setDaysBeforeReminding:(NSUInteger)daysBeforeReminding;

The `tracksNewVersions` configuration determines whether or not UAAppReviewManager should track a new app version if detected. By default, UAAppReviewManager tracks **all** new bundle versions. When it detects a new version, it resets the values saved for usage, significant events, popup shown, user action etc... By setting this to `NO`, UAAppReviewManager will **only** track the version it was initialized with, or the one it last knew about. If this setting is set to `YES`, UAAppReviewManager will reset itself after each new version detection. Its default value is `YES`.

    + (BOOL)tracksNewVersions;
    + (void)setTracksNewVersions:(BOOL)tracksNewVersions;

The `shouldPromptIfRated` configuration determines whether or not to show the review prompt to users who have rated the app once before. This setting is a little like the `tracksNewVersions` setting, but a little less nuclear. Setting this to `NO` will cause new users of the app to get the popup, but won't ask users who have already been asked for a popup in the past. This is useful if you release small bugfix versions and don't want to pester your existing users with popups for every minor version, but want to ensure new users get prompted for a review. For example, you might set this to NO for every minor build, then when you push a major version upgrade, leave it as YES to ask for a rating again. Its default value is `YES`.

    + (BOOL)shouldPromptIfRated;
    + (void)setShouldPromptIfRated:(BOOL)shouldPromptIfRated;

The `useMainAppBundleForLocalizations` configuration is a way to tell UAAppReviewManager that you are providing your own translations for the review prompt popup strings. This may be because you are just customizing them, or that you have set your own text for the popup. If set to `YES`, the main bundle will always be used to load localized strings. If set to `NO` UAAppReviewManager will look in its own translation bundle for the translating strings. It's default value is `NO`.

    + (BOOL)useMainAppBundleForLocalizations;
    + (void)setUseMainAppBundleForLocalizations:(BOOL)useMainAppBundleForLocalizations;

##### Affiliate Codes

The `affiliateCode` configuration is optional and is used to configure with the review URL. If you are an Apple Affiliate, enter your code here. If none is set, the author's code will be used as it is better to be set as something rather than nothing. If you want to thank me for making UAAppReviewManager, feel free to leave this value at its default.

    + (NSString *)affiliateCode;
    + (void)setAffiliateCode:(NSString*)affiliateCode;

The `affiliateCampaignCode` configuration is optional and is used to configure the review URL. It provides context to the affiliate code and defaults to "UAAppReviewManager".

    + (NSString *)affiliateCampaignCode;
    + (void)setAffiliateCampaignCode:(NSString*)affiliateCampaignCode;

##### Debug Mode

The `debug` configuration is useful for testing how your review prompt popup looks and for testing. Setting it to `YES` will show the UAAppReviewManager alert every time. Calling this method in a production build (determined when `DEBUG` preprocessor macro is *not* defined) has no effect. In App Store builds, you don't have to worry about accidentally leaving `setDebug` to `YES`. The default value of `debug` is `NO`.

    + (BOOL)debug;
    + (void)setDebug:(BOOL)debug;
    
##### iOS Only Configuration

These configuration methods only make sense for iOS builds due to their dependency on iOS-only frameworks and methods.

The `usesAnimation` configuration determines whether or not UAAppReviewManager uses animation when presenting a modal `SKStoreProductViewController`. Its default value is `YES`.

    + (BOOL)usesAnimation;
    + (void)setUsesAnimation:(BOOL)usesAnimation;

The `opensInStoreKit` configuration determines if UAAppReviewManager will open the App Store link inside the App using a `SKStoreProductViewController`. By default, this is `NO`.

    + (BOOL)opensInStoreKit;
    + (void)setOpensInStoreKit:(BOOL)opensInStoreKit;
    
There are 2 reasons why the default is `NO`.

- The SKStoreProductViewController __does not allow the user to write a review__ (as of iOS 7 GM)!
- iTunes affiliate codes do not work (as of the iOS 7 GM) inside SKStoreProductViewController.

### UAAppReviewManager Methods

`appLaunched:` tells UAAppReviewManager that the app has launched and that the 'uses' count should be incremented. You should call this method at the end of your Application Delegate's `application:didFinishLaunchingWithOptions:` method. If the App has been used enough to be rated (and enough significant events), you can suppress the rating alert by passing `NO` for `canPromptForRating`. The rating alert will simply be postponed until it is called again with `YES` for `canPromptForRating`. The rating alert can also be triggered by `appEnteredForeground:` and `userDidSignificantEvent:` (as long as you pass `YES` for `canPromptForRating` in those methods).

    + (void)appLaunched:(BOOL)canPromptForRating;


`appEnteredForeground:` tells UAAppReviewManager that the app was brought to the foreground. You should call this method from the Application Delegate's `applicationWillEnterForeground:` method. If the app has been used enough to be rated (and enough significant events), you can suppress the rating alert by passing `NO` for `canPromptForRating`. The rating alert will simply be postponed until it is called again with `YES` for `canPromptForRating`. The rating alert can also be triggered by `appLaunched:` and `userDidSignificantEvent:` (as long as you pass `YES` for `canPromptForRating` in those methods).

    + (void)appEnteredForeground:(BOOL)canPromptForRating;

`userDidSignificantEvent:` tells UAAppReviewManager that the user performed a significant event. A significant event is whatever you want it to be. If you're app is used to make VoIP calls, then you might want to call this method whenever the user places a call. If it's a game, you might want to call this whenever the user beats a level boss. If the user has performed enough significant events and used the app enough, you can suppress the rating alert by passing `NO` for `canPromptForRating`. The rating alert will simply be postponed until it is called again with `YES` for `canPromptForRating`. The rating alert can also be triggered by `appLaunched:` and `appEnteredForeground:` (as long as you pass `YES` for `canPromptForRating` in those methods).

    + (void)userDidSignificantEvent:(BOOL)canPromptForRating;


`showPrompt` tells UAAppReviewManager to show the review prompt alert. The prompt will be showed if there is an internet connection available, the user hasn't already declined to rate, hasn't rated the current version and you are tracking new versions. You could call to show the prompt regardless of UAAppReviewManager settings, for instance, in the case of some special event in your app like positive feedback given.

    + (void)showPrompt;

The `reviewURLString` method is the review URL string, generated by substituting the `appID`, `affiliateCode` and `affiliateCampaignCode` into the template URL for the current platform.

    + (NSString *)reviewURLString;

`rateApp` tells UAAppReviewManager to open the App Store page where the user can specify a rating for the app. It also records the fact that this has happened, so the user won't be prompted again to rate the app for this version. The only case where you should call this directly is if your app has an explicit "Rate this app" command somewhere.  In all other cases, don't worry about calling this — instead, just call the other functions listed above, and let UAAppReviewManager handle the bookkeeping of deciding when to ask the user whether to rate the app.

    + (void)rateApp;

##### iOS Only Methods


`closeModalPanel` tells UAAppReviewManager to immediately close any open rating modal panels for instance, a `SKStoreProductViewController`.

    + (void)closeModalPanel;
    
### Block Callbacks


UAAppReviewManager uses blocks instead of delegate methods for callbacks. Default is nil for all of them.

    + (void)setOnDidDisplayAlert:(UAAppReviewManagerBlock)didDisplayAlertBlock;
    + (void)setOnDeclineToRate:(UAAppReviewManagerBlock)didDeclineToRateBlock;
    + (void)setOnDidOptToRate:(UAAppReviewManagerBlock)didOptToRateBlock;
    + (void)setOnDidOptToRemindLater:(UAAppReviewManagerBlock)didOptToRemindLaterBlock;

##### iOS Only Block Callbacks

    + (void)setOnWillPresentModalView:(UAAppReviewManagerAnimateBlock)willPresentModalViewBlock;
    + (void)setOnDidDismissModalView:(UAAppReviewManagerAnimateBlock)didDismissModalViewBlock;


### NSUserDefaults Keys

UAAppReviewManager has sensible defaults for the `NSUserDefaults` keys it uses, but you can customize that here if you want. Get/Set the `NSUserDefaults` Keys that store the usage data for UAAppReviewManager. Default values are all in the form of "UAAppReviewManagerKey<Setting>"

    + (NSString *)keyForUAAppReviewManagerKeyType:(UAAppReviewManagerKeyType)keyType;
    + (void)setKey:(NSString *)key forUAAppReviewManagerKeyType:(UAAppReviewManagerKeyType)keyType;


### Configuration/Usage Examples

For more information on how to use and setup UAAppReviewManager, please see the Example Project for both iOS and OS X Apps.

##  UAAppReviewManager vs. Appirater

[Appirater](https://github.com/arashpayan/appirater) is great and has been used by many, many developers since its introduction in 2009. It has been updated throughout the years and suits the need of many people, yet leaves a ton left to be desired for the experienced developer. Appirater is:

- Only available for iOS
- Mixes a bunch of class methods and instance methods unnecessarily
- Relies on a confusing mixture of MACROS and runtime configs for setup when either way would be better on its own
- Utilizes the ancient Delegate pattern for callbacks in the age of Blocks
- Is not able to be disabled on minor patch updates
- No iTunes affiliate support

I started addressing these issues in a fork of Appirater, but quickly realized that the entire project could be re-written in a better way to address the above points. UAAppReviewManager is:

- Available for iOS and Mac
- Runs as a Singleton, with Class level, pass-through convenience methods.
- Every aspect is configurable at runtime through an established API.
- Uses Blocks for all event callbacks and notifications.
- Allows developers to disable the prompt easily on minor updates
- Allows iTunes affiliate codes to be used.

Once all these additions, alteration and features were added, it was too much to push back up to Appirater, so UAAppReviewManager was born. That being said, some of the existing code logic, methods, and language translations (27 of them!) are used from [Appirater](https://github.com/arashpayan/appirater) and due credit needs to be given. UAAppReviewManager could not have existed without it. Thank you!


##  Upgrading From Appirater

The API for the methods is backwards close to Appirater, with a few exceptions (there are no delegate methods in UAAppReviewManager, only black calbacks). The first several releases of UAAppReviewManager will include the deprecated Appirater methods and naming scheme. Those methods, and their new counterparts are:

````
OLD: + (void)setAppId:(NSString*)appId __attribute__((deprecated("Use setAppID:")));
NEW: + (void)setAppID:(NSString *)appID;
```` 
````
OLD: + (void)setTimeBeforeReminding:(double)value __attribute__((deprecated("Use setDaysBeforeReminding:")));
NEW: + (void)setDaysBeforeReminding:(NSUInteger)daysBeforeReminding;
```` 
````
OLD: + (void)setAlwaysUseMainBundle:(BOOL)useMainBundle __attribute__((deprecated("Use setUseMainAppBundleForLocalizations:")));
NEW: + (void)setUseMainAppBundleForLocalizations:(BOOL)useMainAppBundleForLocalizations;
```` 
````
OLD: + (void)appLaunched __attribute__((deprecated("Use appLaunched: instead")));
NEW: + (void)appLaunched:(BOOL)canShowPrompt;
```` 
````
OLD: + (void)setDelegate:(id)delegate __attribute__((deprecated("Use the block-based callbacks instead")));
NEW: + (void)setOnDidDisplayAlert:(UAAppReviewManagerBlock)didDisplayAlertBlock;
NEW: + (void)setOnDeclineToRate:(UAAppReviewManagerBlock)didDeclineToRateBlock;
NEW: + (void)setOnDidOptToRate:(UAAppReviewManagerBlock)didOptToRateBlock;
NEW: + (void)setOnDidOptToRemindLater:(UAAppReviewManagerBlock)didOptToRemindLaterBlock;
NEW: + (void)setOnWillPresentModalView:(UAAppReviewManagerAnimateBlock)willPresentModalViewBlock;
NEW: + (void)setOnDidDismissModalView:(UAAppReviewManagerAnimateBlock)didDismissModalViewBlock;
```` 
````
OLD: + (void)setOpenInAppStore:(BOOL)openInAppStore __attribute__((deprecated("Use setOpensInStoreKit:")));
NEW: + (void)setOpensInStoreKit:(BOOL)opensInStoreKit;
````

For most people doing an upgrade, __all that will be needed for upgrading is a simple find/replace__ on `Appirater` to change it to `UAAppReviewManager`.

UAAppReviewManager will automatically convert the NSUserDefault keys store under Appirater apps into the keys used by UAAppReviewManager. The values will transfer over, and the old, unused Appirater keys will be deleted from the settings.


## What's Planned?

There are some ideas we have for future versions of UAAppReviewManager. Feel free to fork/implement if you would like to expedite the process.

- Add the ability to present the prompt using a custom class other than a `UIAlertView` or `NSAlert`
- Add additional localizations
-  [Your idea](https://github.com/coneybeare/UAAppReviewManager/issues).

## Bugs / Pull Requests
Let us know if you see ways to improve UAAppReviewManager or see something wrong with it. We are happy to pull in pull requests that have clean code, and have features that are useful for most people.

## What Does UA stand for?
[Urban Apps](http://urbanapps.com). We make neat stuff. Check us out.

## Open-Source Urban Apps Projects

- [UAModalPanel](https://github.com/coneybeare/UAModalPanel) - An animated modal panel alternative for iOS
- [UALogger](https://github.com/coneybeare/UALogger) - A logging utility for Mac/iOS apps

## Feeling Generous?

If you want to thank us for open-sourcing UAAppReviewManager, you can [buy one of our apps](http://itunes.com/apps/urbanapps?at=11l7j9&ct=github) or even donate something small.

<a href='http://www.pledgie.com/campaigns/21926'><img alt='Click here to lend your support to: Support UAAppReviewManager Development and make a donation at www.pledgie.com !' src='http://www.pledgie.com/campaigns/21926.png?skin_name=chrome' border='0' /></a>

[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/coneybeare/uaappreviewmanager/trend.png)](https://bitdeli.com/free "Bitdeli Badge")

