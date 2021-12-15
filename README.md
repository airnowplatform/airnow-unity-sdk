Airnow Media Unity plugin
======
> **Airnow Media Unity plugin 11.4.2 Documentation**

## Integrate the Airnow Media SDK for Unity

Use these instructions to integrate the Airnow Media SDK with your Unity app.

#### High-Level Steps to Integrate
Airnow Media supports Unity editor version 2018.4 (LTS) and above. In general, we will support any Unity version from 2-years prior.
1. Integrate the Airnow Media Unity plugin.
2. Initialize the SDK.
3. Configure ad units in your app.

#### Step 1. Integrate the Plugin
1. Download the latest Airnow Media Unity plugin.
2. To import the Airnow Media Unity plugin, double-click on the AirnowPlugin.unitypackage, or go to Assets → Import Package → Custom Package. Keep all the files in the Importing Package window selected, and click Import.
3. Build and run your Unity project.

> **Note**: The Airnow Media Unity plugin contains a demo scene with an example of integration.

#### Step 2. Initialize the SDK
You need to initialize once per app lifecycle, this will typically happen upon launching the app. Use the API Key and appID that were provided when registering the Android Application on Airnow Media. These will both be numeric codes that can be found in the app dashboard.
**Do not make any ad requests until the SDK initialization is complete.**

In your app’s `Start()` method, call 
``` c#
Airnow.InitializeSdk(apiKey, appId)
```
with the valid `apiKey` and `appId`.

If your game is distributed on both `iOS` and `Android`, you only need to import the plugin once, but you must create a different `apiKey` and `appId` for each platform:

``` c#
#if UNITY_ANDROID
    string apiKey = "YOUR_ANDROID_API_KEY";
    string appId= "YOUR_ANDROID_APP_ID";
#elif UNITY_IPHONE
    string apiKey = "YOUR_IOS_API_KEY";
    string appId= "YOUR_IOS_APP_ID";
#endif
```
##### Airnow Media SDK Test Mode (optional)
You can set your app to test mode in order to customize ads in your application. 
**Please note, in test mode, you will only receive test ads.**

To enable or disable test mode, follow the below:
``` c#
Airnow.TestMode = mode;
```

##### Airnow Media Plugin log level (optional)

You can set the log level to `Debug`, `Info`, or `None` for the plugin using `Airnow.LogLevel`. Default level is `None`.

``` c#
PluginLogLevel = logLevel
```
##### Airnow Media Opt-Out Ads (optional, Android only)

An Android mobile device may provide a "Limit Ad Tracking" or "Opt out of interest-based advertising" setting. If a user opts out of using this option on a device, Airnow Media will not use the information collected from that device to identify his interests or to display ads on that device that target his intended interests.

The Airnow Media SDK also contains built-in functionality to manage personalized ad opt-outs. Application developers can use them to provide this functionality at the application level.

To manage personalized ad opt-outs, follow the below:

``` c#
Airnow.CanCollectPersonalInfo = mode;
```
If a user opts out, he will still see the same amount of Airnow Media ads on his mobile device; however, these ads may be less relevant because they will not be relevant to his interests.

#### Step 3. Configure Ad Units in Your App

Depending on the ad format you are integrating, load the plugin for it as shown below:

``` c#
Airnow.LoadBannerPlugin();
Airnow.LoadBannerPlugin(_bannerPlacement);
Airnow.LoadInterstitialPlugin();
Airnow.LoadInterstitialPlugin(_interstitialPlacement);
Airnow.LoadRewardedPlugin(_rewardedPlacement, userId);
```
Every method returns `adUnitId`. Use this to access your plugin.

#### Banner Ads

Banner ads usually appear at the top or bottom of your app’s screen. Adding one to your app takes just a few lines of code.

You can specify a desired maximum ad size, and the Airnow Media SDK will request an ad for that size. 

##### Banner integration

1. Request a banner:

``` c#
Airnow.RequestBanner(bannerAdUnit, position, maxAdSize);
```
Use `Airnow.MaxAdSize` to set the maximum ad size::

``` c#
Width320Height50
Width468Height60
Width300Height250
Width728Height90
```
Use `Airnow.AdPosition` to set the position of the banner.:

``` c#
TOP
CENTER
BOTTOM
```

> **Note**: Ad requests must be made on the main thread.

2. Show or hide the banner (this is optional; the banner is shown by default):

``` c#
Airnow.ShowBanner (bannerAdUnitId, true);   // shows the banner
Airnow.ShowBanner (bannerAdUnitId, false);  // hides the banner
```

3. Destroy the banner:

``` c#
Airnow.DestroyBanner(bannerAdUnitId);
```
> **Note**: When the hosting Activity or Fragment is destroyed, be sure to also destroy the Airnow banner.

4. Optionally implement lifecycle callbacks. 

The Airnow Media Unity plugin includes a variety of optional callbacks that you can use to be notified of events. The banner-related callback handlers are below:

``` c#
OnBannerStartLoadEvent(string adUnitId, float height)
OnBannerLoadedEvent(string adUnitId)
OnBannerLoadFailureEvent(string adUnitId, string error)
OnBannerClickedEvent(string adUnitId)
OnBannerOpenedEvent(string adUnitId)
OnBannerClosedEvent(string adUnitId)
OnBannerCompletedEvent(string adUnitId)
OnBannerLeaveApplicationEvent(string adUnitId)
OnBannerShowFailureEvent(string adUnitId, string error)
```

##### Best Practices
• Only create a banner ad view when you display it to the user.

• If the user navigates from a screen with a banner on it to a screen that does not have a banner, destroy or hide the banner.

#### Interstitial Ads

Interstitial Ads are full-screen ads that cover the interface of their main application. They are usually displayed at points of natural transition in the flow of the application, for example, between levels in a game. 
Airnow Media SDK supports static and animated Interstitial ad types, as well as video VAST ads.

##### Interstitial ads integration

1. Pre-fetch an interstitial ad:

``` c#
Airnow.RequestInterstitialAd(interstitialAdUnitId);
```

> **Note**: Ad requests must be made on the main thread.

2. Check whether the interstitial ad is ready to be shown or not:

``` c#
Airnow.IsInterstitialAdReady(interstitialAdUnitId);
```

3. Show the interstitial ad:

``` c#
AIrnow.ShowInterstitialAd (interstitialAdUnitId);
```

4. Optionally implement lifecycle callbacks. 

The interstitial-related callback handlers are below:

``` c#
OnInterstitialStartLoadEvent(string adUnitId)
OnInterstitialLoadSuccessEvent(string adUnitId)
OnInterstitialLoadFailureEvent(string adUnitId, string error)
OnInterstitialClickedEvent(string adUnitId)
OnInterstitialOpenedEvent(string adUnitId)
OnInterstitialClosedEvent(string adUnitId)
OnInterstitialCompletedEvent(string adUnitId)
OnInterstitialLeaveApplicationEvent(string adUnitId)
OnInterstitialShowFailureEvent(string adUnitId, string error)

```

5. Destroy the interstitial ad:

``` c#
Airnow.DestroyInterstitialAd (interstitialAdUnitId);
```

> **Note**: When the hosting Activity is destroyed, be sure to also destroy the Airnow interstitial ad.


##### Best Practices

• You can be notified that an interstitial was fetched successfully by implementing OnInterstitialLoadSuccessEvent. We highly recommend waiting for the OnInterstitialLoadSuccessEvent callback before showing the interstitial ad. Waiting provides the ad enough time to load and show as expected.

#### Rewarded Ads

Rewarded ads are an excellent way to keep users engaged in your app while earning ad revenue. The reward usually comes in the form of in-game currency (coins, lives, gold, etc.). The user gets it after a successful ad completion. 

##### Rewarded ads integration

1. Pre-fetch a rewarded ad:

``` c#
Airnow.RequestRewardedAd(rewardedAdUnitId);
```

> **Note**: Ad requests must be made on the main thread.

2. Check whether the rewarded ad is ready to be shown or not:

``` c#
Airnow.IsRewardedAdReady(rewardedAdUnitId);
```

3. Show the rewarded ad:

``` c#
AIrnow.ShowRewardedAd (rewardedAdUnitId);
```

4. Optionally implement lifecycle callbacks. 

These are the rewarded ad-related callback handlers:

``` c#
OnRewardedStartLoadEvent(string adUnitId)
OnRewardedLoadedEvent(string adUnitId, string label, float amount)
OnRewardedLoadFailureEvent(string adUnitId, string error)
OnRewardedClickedEvent(string adUnitId)
OnRewardedOpenedEvent(string adUnitId)
OnRewardedClosedEvent(string adUnitId)
OnRewardedLeaveApplicationEvent(string adUnitId)
OnRewardedShowFailureEvent(string adUnitId, string error)
OnRewardedStartedEvent(string adUnitId)
OnRewardedCompletedEvent(string adUnitId)
OnRewardedReceivedRewardEvent(string adUnitId, string label, float amount)
```

6. Destroy the rewarded ad:

``` c#
Airnow.DestroyRewardedAd (rewardedAdUnitId);
```

> **Note**: When the hosting Activity is destroyed, be sure to also destroy the Airnow rewarded ad.

##### Best Practices

• We recommend placing rewarded ads where your users are already engaging with in-app purchases or in locations where users may be seeking an in-app reward, such as the end of a game or at currency redemption points.

• You can be notified that a rewarded ad was fetched successfully by implementing OnRewardedlLoadSuccessEvent. We highly recommend waiting for the OnRewardedLoadSuccessEvent callback before showing the rewarded ad. Waiting provides the ad enough time to load and show as expected.

#### Integration specifics for XCode (iOS, macOS, etc.)

After building the XCode project, do the following steps:
1. Open Terminal app on your Mac
2. Go to your project’s folder via Terminal
3. Install CocoaPods dependencies (run **`pod install`** command)
