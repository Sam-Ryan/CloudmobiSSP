# Getting Started with the Cloudmobi iOS SDK

- [Before You Start](#start)
- [SDK Set Up Manually](#step1)
  - [Rewarded Video](#rewardedvideo)
  - [Native](#native)
  - [Banner](#banner)
  - [Interstitial](#interstitial)
  - [Native Template](#nativetemplate)
  - [Native Video](#nativevideo)

- [Cocoapods Integration](#step2)



## <a name="start">Before You Start</a>

- Cloudmobi iOS SDK supports banner, interstitial, native, and rewarded video 
- Cloudmobi iOS supports iOS 7.0+;
- Please make sure you have signed up on the Cloudmobi Platform. If you haven't signed up, please contact us via email: sdk_support@yeahmobi.com
- Please make sure you have added an app and at least one ad slot in Cloudmobi Platform
- Please download [our latest SDK](https://github.com/cloudmobi/CloudmobiSSP/raw/master/(CT)iOS-SDK.zip)

## <a name="step1">SDK Set Up Manually</a>

* 1. Unzip the SDK package and drop CTSDK.framework k into your Xcode project.

PRO TIP: Checkmark the Copy items if needed option. This creates a local copy of the framework for your project, which keeps your project organized.

* 2. Link the StoreKit.framework, AVFoundation.framework, SystemConfiguration.framework, AdSupport.framework,JavaScriptCore.framework,ImageIO.framework and UIKit frameworks.

**In Info.plist added the NSAppTransportSecurity**

```
<key>NSAppTransportSecurity</key>
<dict>
	<key>NSAllowsArbitraryLoads</key>
	<true/>
</dict>
```

* 3. You should add a static link to Xcode project: Build Settings -> Other Linker Flags -> -ObjC

* 4. Add the import header #import <CTSDK/CTSDK.h> to your 
     AppDelegate.m file.
     4.Initialize CTSDK in your didFinishLaunchingWithOptions method.

```
 - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        [[CTService shareManager] loadRequestGetCTSDKConfigBySlot_id:Your_Cloudmobi_slotID];
        return YES;
}
```
**If you have rewarded video ads in your app, please make sure to use the rewarded video slot id for Initialization**

* 5. Add your Slot ID .

Replace Your_Cloudmobi_slotID with your Slot ID -- Where can I find my Slot ID? Please refer to the picture below

![image](https://user-images.githubusercontent.com/20314643/35969194-03005aa0-0d01-11e8-8d24-92a6b9882a77.png)



###  <a name="rewardedvideo">Adding the RewardVideo Ad API in iOS</a>


```     
    /**
     * to get your rewarded video
     * 1. call loadRewardVideoWithSlotId:delegate:to get RewardVideo，if success, you can use the rewarded video to show
     * @param slot_Id       RewardVideo SlotID
     * @param delegate      declare that ViewController implements the CTRewardVideoDelegate protocol, and add a function that loads the Rewarded video.
     */
    - (void)loadRewardVideoWithSlotId:(NSString *)slot_id delegate:(id)delegate;
    /**
     show 
     @param viewController set present RewardVideo Viewcontroller
     */
    - (void)showRewardVideoWithPresentingViewController:(UIViewController *)viewController;
    /**
     Please check if the reward video works well before show it
     */
    - (BOOL)checkRewardVideoIsReady;
    /**
    set custom prameters for reward video
    @param customParams         
    */
    - (void)setCustomParameters:(NSString *)customParams;
    
    CTRewardVideoDelegate delegate callback interface
    - (void)CTRewardVideoLoadSuccess {
        NSLog(@"reward video is loaded and ready to be displayed");
    }                       
    - (void)CTRewardVideoDidStartPlaying {
        NSLog(@"reward video starts to play");
    }
    - (void)CTRewardVideoDidFinishPlaying {
        NSLog(@"reward video complete-this is called after a full video view,before end card is shown");
    } 
    - (void)CTRewardVideoDidClickRewardAd {
        NSLog(@"reward video clicked");
    }
    - (void)CTRewardVideoWillLeaveApplication {
        NSLog(@"users click reward video and leave application");
    } 
    - (void)CTRewardVideoJumpfailed {
        NSLog(@"reward video click and failed jumping to App Store");
    }
    - (void)CTRewardVideoLoadingFailed:(NSError *)error {
        NSLog(@"reward video request failed ");
    }
    - (void)CTRewardVideoClosed {
        NSLog(@"reward video ad closed-this can be triggered by closing the end card");
    }
    - (void)CTRewardVideoAdRewardedName:(NSString *)rewardName rewardAmount:(NSString *)rewardAmount customParams:(NSString*) customParams {
        NSLog(@"give reward to the users interface");
    }
```



### <a name="native">Adding the Native Ad API in iOS</a>

```
    /**
     * To get the Native ads , You Custom Ad View should inherit CTNativeAd 
     * @param slot_Id       Native Ad SlotID
     * @param delegate      declare that ViewController implements the CTNativeAdDelegate protocol
     * @param WHRate        Request image rate
     * @param isTest        Debug Mode , Retention parameters
     * @param success       Load Ad success callback
     * @param failure       Load Ad Failed callback
     */
    -(void)getNativeADswithSlotId:(NSString *)slot_id
                     delegate:(id)delegate
          imageWidthHightRate:(CTImageWidthHightRate)WHRate
                       isTest:(BOOL)isTest
                      success:(void (^)(CTNativeAdModel *nativeModel))success
                      failure:(void (^)(NSError *error))failure;
    /**
     * To get the multiple Native ads , You Custom Ad View should inherit CTNativeAd 
     * @param slot_Id       Native Ad SlotID
     * @param num           Ad numbers
     * @param delegate      declare that ViewController implements the CTNativeAdDelegate protocol
     * @param WHRate        Request image rate
     * @param isTest        Debug Mode , Retention parameters
     * @param success       Load Ad success callback
     * @param failure       Load Ad Failed callback
     */
    -(void)getMultitermNativeADswithSlotId:(NSString *)slot_id
                             adNumbers:(NSInteger)num
                              delegate:(id)delegate
                   imageWidthHightRate:(CTImageWidthHightRate)WHRate
                                isTest:(BOOL)isTest
                               success:(void (^)(NSArray *nativeArr))success
                               failure:(void (^)(NSError *error))failure;
                               
    CTNativeAdDelegate delegate callback interface
    - (void)CTNativeAdDidClick:(UIView *)nativeAd {
        NSLog(@"click Ad");
    }
    - (void)CTNativeAdDidIntoLandingPage:(UIView *)nativeAd {
        NSLog(@"did Into LandingPage");
    }
    - (void)CTNativeAdDidLeaveLandingPage:(UIView *)nativeAd {
        NSLog(@"did Leave LandingPage");
    }
    - (void)CTNativeAdWillLeaveApplication:(UIView *)nativeAd {
        NSLog(@"will Leave Application");
    }
    - (void)CTNativeAdJumpfail:(UIView *)nativeAd {
        NSLog(@"click ad Jump App store failed");
    }

```



### <a name="banner">Adding the Banner Ad API in iOS</a>


```
    /**
     To get the Banner ads 
     @param slot_id         Banner Ad SlotID
     @param delegate        declare that ViewController implements the CTBannerDelegate protocol
     @param frame           Set Ad Frame
     @param isNeedBtn       is need Close button
     @param isTest          Debug Mode , Retention parameters
     @param success         Load Ad success callback
     @param failure         Load Ad Failed callback
     */
    - (void)getBannerADswithSlotId:(NSString *)slot_id
                          delegate:(id)delegate
                             frame:(CGRect)frame
                   needCloseButton:(BOOL)isNeedBtn
                            isTest:(BOOL)isTest
                           success:(void (^)(UIView *bannerView))success
                           failure:(void (^)(NSError *error))failure;
                           
    CTBannerDelegate Callback interface
    - (void)CTBannerDidClick:(CTBanner*)banner {
        NSLog(@""click Ad");
    }
    - (void)CTBannerDidIntoLandingPage:(CTBanner*)banner {
        NSLog(@"did Into LandingPage");
    }
    - (void)CTBannerDidLeaveLandingPage:(CTBanner*)banner {
        NSLog(@"did Leave LandingPage");
    }
    - (void)CTBannerClosed:(CTBanner*)banner {
        NSLog(@"closed ad");
    }
    - (void)CTBannerWillLeaveApplication:(CTBanner*)banner {
        NSLog(@"will leave Application");
    }
    - (void)CTBannerHtml5Closed:(CTBanner*)banner {
        NSLog(@"closed html5 ad");
    }
    - (void)CTBannerJumpfail:(CTBanner *)banner {
        NSLog(@"click ad Jump App store failed");
    }

```



### <a name="interstitial">Adding the Interstitial Ad API in iOS</a>

```

    /**
     To get the Interstitial ads
     @param slot_id         Interstitial Ad SlotID
     @param delegate        declare that ViewController implements the CTInterstitialDelegate protocol
     @param isFull          Set Ad is Full Screen
     @param isTest          Debug Mode , Retention parameters
     @param success         Load Ad success callback
     @param failure         Load Ad failed callback
     */
    - (void)preloadInterstitialWithSlotId:(NSString *)slot_id
                                 delegate:(id)delegate
                             isFullScreen:(BOOL)isFull
                                   isTest:(BOOL)isTest
                                  success:(void (^)(UIView *InterstitialView))success
                                  failure:(void (^)(NSError *error))failure;
    /**
     Show Interstitial
     */
    - (BOOL)interstitialShow;
    /**
     Close Interstitial
     */
    - (BOOL)interstitialClose;
    /**
     Interstitial Screen Direction
     if you use interstitialShow，please use the interface below set your screen orientation 
     */
    - (void)ScreenIsVerticalScreen:(BOOL)isVerticalScreen;
    /**
     use Viewcontroller to show ads
     */
    - (BOOL)interstitialShowWithControllerStyleFromRootViewController:(UIViewController *)rootViewController;
    
    CTInterstitialDelegate Callback interface
    - (void)CTInterstitialDidClick:(CTInterstitial*)interstitial {
        NSLog(@"click Ad");
    }
    - (void)CTInterstitialDidIntoLandingPage:(CTInterstitial*)interstitial {
        NSLog(@"did Into LandingPage");
    }
    - (void)CTInterstitialDidLeaveLandingPage:(CTInterstitial*)interstitial {
        NSLog(@"did Leave LandingPage");
    }
    - (void)CTInterstitialClosed:(CTInterstitial*)interstitial {
        NSLog(@"closed ad");
    }
    - (void)CTInterstitialWillLeaveApplication:(CTInterstitial*)interstitial {
        NSLog(@"will leave Application");
    }
    - (void)CTInterstitialJumpfail:(CTInterstitial *)interstitialAD {
        NSLog(@"click ad Jump App store failed");
    }
```



### <a name="nativetemplate">Adding the Native Template Ad API in iOS</a>

```

/**
     To get the Native Template ads
     @param slot_id         NATemplate Ad SlotID
     @param delegate        declare that ViewController implements the CTNaTemplateDelegate protocol
     @param frame           Set Ad Frame
     @param isNeedBtn       is need Close button
     @param isTest          Debug Mode , Retention parameters
     @param success         Load Ad success callback
     @param failure         Load Ad failed callback
     */
    - (void)getNaTemplateADswithSlotId:(NSString *)slot_id
                              delegate:(id)delegate
                                 frame:(CGRect)frame
                       needCloseButton:(BOOL)isNeedBtn
                                isTest:(BOOL)isTest
                               success:(void (^)(UIView *NaTemplateView))success
                               failure:(void (^)(NSError *error))failure;
                               
    CTNaTemplateDelegate Callback Delegate 
    - (void)CTNaTemplateDidClick:(CTNaTemplate*)naTemplate {
        NSLog(@"click Ad");
    }
    - (void)CTNaTemplateDidIntoLandingPage:(CTNaTemplate*)naTemplate {
        NSLog(@"did Into LandingPage");
    }
    - (void)CTNaTemplateDidLeaveLandingPage:(CTNaTemplate*)naTemplate {
        NSLog(@"did Leave LandingPage");
    }
    - (void)CTNaTemplateClosed:(CTNaTemplate*)naTemplate {
        NSLog(@"closed ad");
    }
    - (void)CTNaTemplateWillLeaveApplication:(CTNaTemplate*)naTemplate {
        NSLog(@"will leave Application");
    }
    - (void)CTNaTemplateHtml5Closed:(CTNaTemplate*)naTemplate {
        NSLog(@"closed html5 ad");
    }
    - (void)CTNaTemplateJumpfail:(CTNaTemplate *)naTemplate {
        NSLog(@"click ad Jump App store failed");
    }

```

### <a name="nativevideo">Adding the Native Ad API in iOS</a>

```
/**
 Get Native video Ad
 Call this interface to get Native Video AD.
 
 @param slot_id         You should use Native Video AD ID or you wouldn't get ads.
 @param delegate        Set Delegate of Ads event (<CTNativeVideoDelegate>)
 @param WHRate          Set Image Rate
 @param isTest          Use test advertisement or not
 */
- (void)getNativeVideoADswithSlotId:(NSString*)slot_id
                           delegate:(id)delegate
                imageWidthHightRate:(CTImageWidthHightRate)WHRate
                             isTest:(BOOL)isTest;
			  
CTNativeVideoDelegate Callback Delegate
-(void)CTNativeVideoLoadSuccess:(CTNativeVideoModel *)nativeVideoModel;  //Advertisement load success.
-(void)CTNativeVideoLoadFailed:(NSError *)error;                         //Advertisement load failed
```

Done!

You are now all set to deliver Cloudmobi Ads within your application!

## <a name="step2">Cocoapods Integration</a>
* Install CocoaPods and make sure you are running the latest version of CocoaPods by running:

```
gem install cocoapods
# (or if the above fails)
sudo gem install cocoapods
```

* Add CTSDK or CTMediationSDK pods in Podfile. CTMediationSDK integrates Facebook and Admob mediation.

```
pod 'CTSDK'

or

pod 'CTMediationSDK'
```
* Start Coding! You can check the [SDK Set Up Tutorials](#step1) above.







