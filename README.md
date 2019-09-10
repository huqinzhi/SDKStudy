# SDKStudy
学习记录
广告sdk开发    一：https://www.jianshu.com/p/df74ae71b041        
              二：https://www.jianshu.com/p/338ca2da7895          
什么是SDK      ：https://blog.bihe0832.com/sdk_summary_sdk_of_my_explain.html          
SDK架构及资源   ：https://blog.bihe0832.com/sdk_design_structure_resource.html              
SDK接口设计    ：https://blog.bihe0832.com/sdk_desigin_api.html              

block ：https://blog.csdn.net/zc639143029/article/details/50084227             

1.facebook : 框架.            
FBAudienceNetwork.framework	           

mopub:  https://github.com/mopub/mopub-ios-sdk/releases               
unityAds : https://github.com/Unity-Technologies/unity-ads-ios
#**iOS集成UpArpuSDK**#

##1 简介##
本文档介绍如何去集成iOS端的UparpuSDK（后面简称为SDK），包括获取开发者账号，获取AppID和AppKey并创建配置进行广告投放。
###1.1 支持的广告类型###
UpArpuSDK支持原生广告(Native),激励视频广告(rewardVideo)，banner广告，插屏广告(intersitial)和开屏广告(splash)。
###1.2 SDK架构###
![](UpArpuSDK_Architecture.png)
##<h2 id='1'>2 配置</h2>##
###2.1 基础配置###
	Xcode10版本及以上。
	Target iOS 8.0及以上。

###2.2 导入基础核心框架###
核心模块包含以下框架和资源包文件，只需将它们拖放到Xcode中。

|File|Note|
|---|---|
|UpArpuSDK.framework|Base framework|
|UpArpuSDK.bundle|Resource bundle|
|UpArpuHeaderBiding.framework|Header bidding module|

**注:** 由于**UpArpuSDK**不支持cocoapod，以UpArpu开头的framewrok必须手动下载并导入到您的项目中，而第三方SDK可以使用cocoapod集成。

###2.3 配置 Build Settings 和 Info.plist##

1) 在 Xcode中, 点击到 **Build Settings**, 搜索 **Other Linker Flags** 然后添加 **-ObjC**(这里的字母O和字母C**需要大写**), 注意 **Linker Flags** 是区分大小写的:
![](Other_Linker_Flags.png)
如果您没有看到如上图所示的弹出窗口，只需双击 **Other Linker Flags**。<br><br>
2) 在您app的Info.plist中添加 **NSAllowsArbitraryLoads** 禁用ATS限制。
![](Info_Plist_HTTP.png)
###2.4 导入第三方的SDK###


|第三方平台|需要导入的包|**TopOn**支持的版本|下载链接|参考网址|备注|
|---|---|---|---|---|---|---|
|Facebook|FBAudienceNetwork.framework<br> FBAudienceNetworkBiddingKit.framework <br>FBSDKCoreKit.framework<br>|v5.4.0|https://developers.facebook.com/docs/audience-network/download#ios|https://developers.facebook.com/docs/audience-network/ios|
|Admob|GoogleMobileAds.framework|v7.48.0|https://support.google.com/admob/answer/2993059?hl=en|https://developers.google.com/admob/ios/quick-start|Admob requires that **app id be configured in the Info.plist of your project**; for more information please refer to <a href="https://developers.google.com/admob/ios/quick-start#update\_your\_infoplist">Admob's website</a>.|
| Inmobi |InMobiSDK.framework|v7.3.1|https://support.inmobi.com/monetize/ios-guidelines/||||
| Flurry |libFlurryAds\_1.0.0.a<br>libFlurry\_9.0.0.a|231\_9.0.0|https://dev.flurry.com/admin/applications||||
| Applovin |AppLovinSDK.framework<br>AppLovinSDKResources.bundle|v6.8.1|https://dash.applovin.com/docs/integration#iosIntegration||||
| Mintegral |MTGSDK.framework<br> MTGSDKBidding.framework<br>MTGSDKReward.framework <br> MTGSDKInterstitialVideo.framework <br> MTGSDKInterstitial.framework|v5.5.3|http://cdn-adn.rayjump.com/cdn-adn/v2/markdown\_v2/index.html?file=sdk-m\_sdk-ios&lang=en||||
| Mopub |MobPowerNative.framework <br> MobPowerSDK.framework| v5.8.0 |https://github.com/mopub||||
| GDT |libGDTMobSDK.a|v4.10.7|https://e.qq.com/dev/index.html||||
| Chartboost |Chartboost.framework| v8.0.1 | https://dashboard.chartboost.com/tools/sdk	||||
| Tapjoy |Tapjoy.framework <br> TapjoyResources.bundle| v12.3.1 |||||
| Ironsource |IronSource.framework|v6.8.4.1|https://developers.ironsrc.com/sdk-repository/||||
| UnityAds |UnityAds.framework| v3.2.0 |https://github.com/Unity-Technologies/unity-ads-ios/releases/tag/3.0.3||||
| Vungle |VungleSDK.framework|v6.3.2|||||
| Adcolony |AdColony.framework|v3.3.8.1|https://github.com/AdColony||||
|TouTiao|BUAdSDK.framework<br>BUAdSDK.bundle|v2.3.1.0|http://ad.toutiao.com/union/media/union/download|||
| Oneway |Oneway|v2.1.0|||||
| Appnext |AppnextNativeAdsSDK<br>AppnextIOSSDK| v1.9.3 |https://developers.appnext.com/docs/ios-sdk-installation||||
| Baidu |BaiduMobAdSDK.framework|v4.6.4|https://mssp.baidu.com/bqt/appco.html#/union/download/sdk||||
|Nend|NendAd.framework <br> NendAdResource.bundle|v5.2.0|https://github.com/fan-ADN||||
| Maio |Maio.framework|v1.4.7|https://github.com/imobile-maio||||
| Yeahmobi |CTSDK.framework|v3.2.0|||||

您可以使用CocoaPods导入第三方SDK，也可以手动下载导入第三方SDK。

###2.4 初始化SDK###

您需要在**AppDelegate**的**application:didFinishLaunchingWithOptions:**方法里面初始化**UpArpuSDK**(必须在请求广告之前去初始化SDK)：


<pre><code>- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
	 [UPArpuAPI setLogEnabled:YES];<span style="color:green">//Turn on debug logs</span>
    [[UPArpuAPI sharedInstance] startWithAppID:@"a5acc73c25fbf5" appKey:@"4f7b9ac17decb9babec83aac078742c7"];
    return YES;
}</code></pre>

###2.5 使用UpArpu的广告位进行测试###
使用**UpArpuSDK**提供的测试广告位可以更快地测试广告功能，如下图所示：

|Ad Format|Placement ID|
|---|---|
|Rewarded Video|b5b44a0f115321|
|Interstitial|b5bacad26a752a|
|Banner|b5bacaccb61c29|
|Native Banner|b5b0f5663c6e4a|
|Splash|b5c1b048c498b9|
|Native Splash| b5b0f5663c6e4a |
|Native|b5b0f5663c6e4a|
注：使用这些广告位需要关联 **AppID**：a5b0e8491845b3 和 **AppKey**：7eae0567827cfe2b22874061763f30c9 <br>
测试完成之后，您需要将**id**和**key**更改为您自己在**TopOn**账号下创建的**id**和**key**。

##3 开屏广告(Splash)##
在继续接入之前，您需要保证您已经完成了以上 [配置](#1) 步骤。

###3.1 导入 Splash Framework###
将**UpArpuSplash.framework**拖到您的项目中，除了**UpArpuSplash.framework**，您还需要集成第三方平台的Adapter，目前**UpArpuSDK**支持以下的平台(对应平台需要导入的Adapter)。

|Third Party Ad Network|Adapter Framework|
|---|---|
|TouTiao|UpArpuTTSplashAdapter.framework|
|GDT|UpArpuGDTSplashAdapter.framework|
|Baidu|UpArpuBaiduSplashAdapter.framework|

###3.1 加载并展示Splash###
加载并展示Splash广告的最佳时机是在应用程序的入口，即**AppDelegate**的**application:didFinishLaunchingWithOptions:**方法中，Splash的加载和展示是统一的一个API，您可以使用以下代码加载并展示一个Splash广告：

<pre><code>- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    [UPArpuAPI setLogEnabled:YES];
    [[UPArpuAPI sharedInstance] startWithAppID:@"a5b0e8491845b3" appKey:@"7eae0567827cfe2b22874061763f30c9" error:nil];
    [self.window makeKeyAndVisible];
    [[UPArpuAdManager sharedManager] loadADWithPlacementID:@"b5c1b048c498b9" extra:@{kUPArpuSplashExtraTolerateTimeoutKey:@5.5} customData:nil delegate:self window:self.window containerView:nil];
    return YES;
}</code></pre>

**注**: 在加载和展示之前调用**self.window**的**makeKeyAndVisible**方法[self.window makeKeyAndVisible];

###3.2 实现Splash的Delegate###

您可以实现**UPArpuSplashDelegate**的方法来获取Splash的各种事件。<br> 
1）您需要确认您的app添加了**UPArpuSplashDelegate**代理协议：
<pre><code>
@interface AppDelegate ()\<UPArpuSplashDelegate\>
@end
</code></pre>

2) 在AppDelegate中实现代理方法：
<pre><code>
\#pragma mark - UPArpu Splash Delegate method(s)
-(void) didFinishLoadingADWithPlacementID:(NSString *)placementID {
    NSLog(@"AppDelegate::didFinishLoadingADWithPlacementID:%@", placementID);
}

-(void) didFailToLoadADWithPlacementID:(NSString\*)placementID error:(NSError*)error {
    NSLog(@"AppDelegate::didFailToLoadADWithPlacementID:%@ error:%@", placementID, error);
}

-(void)splashDidShowForPlacementID:(NSString*)placementID {
    NSLog(@"AppDelegate::splashDidShowForPlacementID:%@", placementID);
}

-(void)splashDidClickForPlacementID:(NSString*)placementID {
    NSLog(@"AppDelegate::splashDidClickForPlacementID:%@", placementID);
}

-(void)splashDidCloseForPlacementID:(NSString*)placementID {
    NSLog(@"AppDelegate::splashDidCloseForPlacementID:%@", placementID);
}
</code></pre>

##4 激励视频(Rewarded Video)##
在继续接入之前，您需要保证您已经完成了以上 [配置](#1) 步骤。
###4.1 导入Rewarded Video Framework###
将**UpArpuRewardedVideo.framework**拖到您的项目中，除了**UpArpuRewardedVideo.framework**，您还需要集成第三方平台的Adapter，目前**UpArpuSDK**支持以下的平台(对应平台需要导入的Adapter)。

|Third Party Ad Network|Adapter Framework|
|---|---|
|Facebook|UpArpuFacebookRewardedVideoAdapter.framework|
|Admob|UpArpuAdmobRewardedVideoAdapter.framework|
|Flurry|UpArpuFlurryRewardedVideoAdapter.framework|
|Applovin|UpArpuApplovinRewardedVideoAdapter.framework|
|GDT|UpArpuGDTRewardedVideoAdapter.framework|
|Baidu|UpArpuBaiduRewardedVideoAdapter.framework|
|TouTiao|UpArpuTTRewardedVideoAdapter.framework|
|Nend|UpArpuNendRewardedVideoAdapter.framework|
|Maio|UpArpuMaioRewardedVideoAdapter.framework|
|AppNext|UpArpuAppNextRewardedVideoAdapter.framework|
|Yeahmobi|UpArpuYeahmobiRewardedVideoAdapter.framework|
|Oneway|UpArpuOnewayRewardedVideoAdapter.framework|
|Mintegral|UpArpuMintegralRewardedVideoAdapter.framework|
|Mopub|UpArpuMopubRewardedVideoAdapter.framework|
|Vungle|UpArpuVungleRewardedVideoAdapter.framework|
|Ironsource|UpArpuIronSourceRewardedVideoAdapter.framework|
|Tapjoy|UpArpuTapjoyRewardedVideoAdapter.framework|
|UnityAds|UpArpuUnityAdsRewardedVideoAdapter.framework|
|Chartboost|UpArpuChartboostRewardedVideoAdapter.framework|
|Inmobi|UpArpuInmobiRewardedVideoAdapter.framework|
|Adcolony|UpArpuAdcolonyRewardedVideoAdapter.framework|

###4.2 加载Rewarded Video###
您需要确认您添加了**UPArpuRewardedVideoDelegate**代理协议：
<pre><code>@interface UPArpuRewardedVideoViewController()\<UPArpuRewardedVideoDelegate\>
//Other properties&methods declarations
@end</code></pre>

加载rewarded video广告:
<pre><code>[[UPArpuAdManager sharedManager] loadADWithPlacementID:@"your rv placement id" extra:@{kUPArpuAdLoadingExtraUserIDKey:@"test\_user\_id"} delegate:self];</code></pre>

您可以实现以下的代理方法来获取各种加载事件：
<pre><code>#pragma mark - loading delegate
-(void) didFinishLoadingADWithPlacementID:(NSString *)placementID {
    NSLog(@"RV Demo: didFinishLoadingADWithPlacementID");
}

-(void) didFailToLoadADWithPlacementID:(NSString* )placementID error:(NSError *)error {
    NSLog(@"RV Demo: failed to load:%@", error);
}</code></pre>

###4.3 判断Rewarded Video是否ready###
您可以检查rewarded video广告是否已经ready：
<pre><code>
if ([[UPArpuAdManager sharedManager] rewardedVideoReadyForPlacementID:@"your rv placement id"]) {
    //Show rv here
} else {
    //Load rv here
}
</code></pre>

###4.4 展示Rewarded Video###
在您rewared video加载完成之后，您可以调用API去展示rewared video：
 
<pre><code>-(void) showAD {
    [[UPArpuAdManager sharedManager] showRewardedVideoWithPlacementID:@"rv_placement_id" inViewController:self delegate:self];
}</code></pre>

###4.5 实现Rewarded Video的Delegate###
您可以实现**Rewarded Video Delegate**的方法来获取rewarded video的各种事件：

<pre><code>#pragma mark - showing delegate
-(void) rewardedVideoDidStartPlayingForPlacementID:(NSString*)placementID {
    NSLog(@"RV Demo: rewardedVideoDidStartPlayingForPlacementID:%@", placementID);
}

-(void) rewardedVideoDidEndPlayingForPlacementID:(NSString*)placementID {
    NSLog(@"RV Demo: rewardedVideoDidEndPlayingForPlacementID:%@", placementID);
}

-(void) rewardedVideoDidFailToPlayForPlacementID:(NSString* )placementID error:(NSError*)error {
    NSLog(@"RV Demo: rewardedVideoDidFailToPlayForPlacementID:%@ error:%@", placementID, error);
}

-(void) rewardedVideoDidCloseForPlacementID:(NSString*)placementID rewarded:(BOOL)rewarded {
    NSLog(@"RV Demo: rewardedVideoDidCloseForPlacementID:%@, rewarded:%@", placementID, rewarded ? @"yes" : @"no");
}

-(void) rewardedVideoDidClickForPlacementID:(NSString*)placementID {
    NSLog(@"RV Demo: rewardedVideoDidClickForPlacementID:%@", placementID);
}</code></pre>

##5 插屏广告(Interstitial)##
在继续接入之前，您需要保证您已经完成了以上 [配置](#1) 步骤。

###5.1 导入Interstitial Framework###
将**UpArpuInterstitial.framework**拖到您的项目中，除了**UpArpuInterstitial.framework**，您还需要集成第三方平台的Adapter，目前**UpArpuSDK**支持以下的平台(对应平台需要导入的Adapter)。

|Third Party Ad Network|Adapter Framework|
|---|---|
|Facebook|UpArpuFacebookInterstitialAdapter.framework|
|Admob|UpArpuAdmobInterstitialAdapter.framework|
|Flurry|UpArpuFlurryInterstitialAdapter.framework|
|Applovin|UpArpuApplovinInterstitialAdapter.framework|
|GDT|UpArpuGDTInterstitialAdapter.framework|
|Baidu|UpArpuBaiduInterstitialAdapter.framework|
|TouTiao|UpArpuTTInterstitialAdapter.framework|
|Nend|UpArpuNendInterstitialAdapter.framework|
|Maio|UpArpuMaioInterstitialAdapter.framework|
|AppNext|UpArpuAppNextInterstitialAdapter.framework|
|Yeahmobi|UpArpuYeahmobiInterstitialAdapter.framework|
|Oneway|UpArpuOnewayInterstitialAdapter.framework|
|Mintegral|UpArpuMintegralInterstitialAdapter.framework|
|Mopub|UpArpuMopubInterstitialAdapter.framework|
|Vungle|UpArpuVungleInterstitialAdapter.framework|
|Ironsource|UpArpuIronSourceInterstitialAdapter.framework|
|Tapjoy|UpArpuTapjoyInterstitialAdapter.framework|
|UnityAds|UpArpuUnityAdsInterstitialAdapter.framework|
|Chartboost|UpArpuChartboostInterstitialAdapter.framework|
|Inmobi|UpArpuInmobiInterstitialAdapter.framework|
|Adcolony|UpArpuAdcolonyInterstitialAdapter.framework|

###5.2 加载Interstitial###
您需要确认你添加了**UPArpuInterstitialDelegate**代理协议：
<pre><code>@interface UPArpuInterstitialViewController()\<UPArpuInterstitialDelegate\>
//Other properties&methods declarations
@end</code></pre>

加载Interstitial广告:
<pre><code>[[UPArpuAdManager sharedManager] loadADWithPlacementID:@"your interstitial placement id" extra:nil delegate:self];</code></pre>

您可以实现以下的代理方法来获取各种加载事件：
<pre><code>#pragma mark - loading delegate
-(void) didFinishLoadingADWithPlacementID:(NSString *)placementID {
    NSLog(@"Interstitial Demo: didFinishLoadingADWithPlacementID");
}

-(void) didFailToLoadADWithPlacementID:(NSString* )placementID error:(NSError *)error {
    NSLog(@"Interstitial Demo: failed to load:%@", error);
}</code></pre>

###5.3 判断Interstitial是否Ready###
您可以检查interstitial广告是否已经ready：
<pre><code>
if ([[UPArpuAdManager sharedManager] interstitialReadyForPlacementID:@"your interstitial placement id"]) {
    //Show interstitial here
} else {
    //Load interstitial here
}
</code></pre>

###5.4 展示Interstitial###
在您Interstitial加载完成之后，您可以调用API去展示Interstitial：
 
<pre><code>-(void) showAD {
    [[UPArpuAdManager sharedManager] showInterstitialWithPlacementID:@"interstitial_placement_id" inViewController:self delegate:self];
}</code></pre>

###5.5 实现Interstitial的Delegate###
您可以实现**UPArpuInterstitialDelegate**的方法来获取interstitial的各种事件：
<pre><code>#pragma mark - showing delegate
-(void) interstitialDidShowForPlacementID:(NSString *)placementID {
    NSLog(@"UPArpuInterstitialViewController::interstitialDidShowForPlacementID:%@", placementID);
}

-(void) interstitialFailedToShowForPlacementID:(NSString\*)placementID error:(NSError\*)error {
    NSLog(@"UPArpuInterstitialViewController::interstitialFailedToShowForPlacementID:%@ error:%@", placementID, error);
}

-(void) interstitialDidStartPlayingVideoForPlacementID:(NSString*)placementID {
    NSLog(@"UPArpuInterstitialViewController::interstitialDidStartPlayingVideoForPlacementID:%@", placementID);
}

-(void) interstitialDidEndPlayingVideoForPlacementID:(NSString*)placementID {
    NSLog(@"UPArpuInterstitialViewController::interstitialDidEndPlayingVideoForPlacementID:%@", placementID);
}

-(void) interstitialDidFailToPlayForPlacementID:(NSString\*)placementID error:(NSError\*)error {
    NSLog(@"UPArpuInterstitialViewController::interstitialDidFailToPlayForPlacementID:%@", placementID);
}

-(void) interstitialDidCloseForPlacementID:(NSString*)placementID {
    NSLog(@"UPArpuInterstitialViewController::interstitialDidCloseForPlacementID:%@", placementID);
}

-(void) interstitialDidClickForPlacementID:(NSString*)placementID {
    NSLog(@"UPArpuInterstitialViewController::interstitialDidClickForPlacementID:%@", placementID);
}</code></pre>

##6 Banner广告##
在继续接入之前，您需要保证您已经完成了以上 [配置](#1) 步骤。

###6.1 导入Banner Framework###
将**UpArpuBanner.framework**拖到您的项目中，除了**UpArpuBanner.framework**，您还需要集成第三方平台的Adapter，目前**UpArpuSDK**支持以下的平台(对应平台需要导入的Adapter)。

|Third Party Ad Network|Adapter Framework|
|---|---|
|Facebook|UpArpuFacebookBannerAdapter.framework|
|Admob|UpArpuAdmobBannerAdapter.framework|
|Flurry|UpArpuFlurryBannerAdapter.framework|
|Applovin|UpArpuApplovinBannerAdapter.framework|
|GDT|UpArpuGDTBannerAdapter.framework|
|Baidu|UpArpuBaiduBannerAdapter.framework|
|TouTiao|UpArpuTTBannerAdapter.framework|
|Nend|UpArpuNendBannerAdapter.framework|
|AppNext|UpArpuAppNextBannerAdapter.framework|
|Yeahmobi|UpArpuYeahmobiBannerAdapter.framework|
|Oneway|UpArpuOnewayBannerAdapter.framework|
|Mopub|UpArpuMopubBannerAdapter.framework|
|Mopub|UpArpuInmobiBannerAdapter.framework|

###6.2 加载Banner###
您需要确认你添加了**UPArpuBannerDelegate**代理协议：
<pre><code>@interface UPArpuBannerViewController()\<UPArpuBannerDelegate\>
//Other properties&methods declarations
@end</code></pre>

加载banner广告:
<pre><code>[[UPArpuAdManager sharedManager] loadADWithPlacementID:@"your banner placement id" extra:nil delegate:self];</code></pre>
您可以实现以下的代理方法来获取各种加载事件：
<pre><code>#pragma mark - loading delegate
-(void) didFinishLoadingADWithPlacementID:(NSString *)placementID {
    NSLog(@"Banner Demo: didFinishLoadingADWithPlacementID");
}

-(void) didFailToLoadADWithPlacementID:(NSString* )placementID error:(NSError *)error {
    NSLog(@"Banner Demo: failed to load:%@", error);
}</code></pre>

###6.3 判断Banner是否Ready###

您可以检查banner广告是否已经ready：

<pre><code>
if ([[UPArpuAdManager sharedManager] bannerAdReadyForPlacementID:@"your banner placement id"]) {
    //Show banner here
} else {
    //Load banner here
}
</code></pre>

###6.4 展示Banner###
在您banner加载完成之后，您可以调用API去展示banner：
 
<pre><code>-(void) showBanner {
    if ([[UPArpuAdManager sharedManager] bannerAdReadyForPlacementID:@"banner placement id"]) {
    //Retrieve banner view
        UPArpuBannerView *bannerView = [[UPArpuAdManager sharedManager] retrieveBannerViewForPlacementID:@"banner placement id"];
        bannerView.delegate = self;
        bannerView.translatesAutoresizingMaskIntoConstraints = NO;
        bannerView.tag = tag;
        [self.view addSubview:bannerView];
        //Layour banner
        [self.view addConstraint:[NSLayoutConstraint constraintWithItem:self.view attribute:NSLayoutAttributeCenterX relatedBy:NSLayoutRelationEqual toItem:bannerView attribute:NSLayoutAttributeCenterX multiplier:1.0f constant:.0f]];
        [self.view addConstraint:[NSLayoutConstraint constraintWithItem:bannerView attribute:NSLayoutAttributeTop relatedBy:NSLayoutRelationEqual toItem:self.view attribute:NSLayoutAttributeTop multiplier:1.0f constant:CGRectGetHeight([UIApplication sharedApplication].statusBarFrame) + CGRectGetHeight(self.navigationController.navigationBar.frame)]];
        [self.view addConstraint:[NSLayoutConstraint constraintWithItem:bannerView attribute:NSLayoutAttributeWidth relatedBy:NSLayoutRelationEqual toItem:nil attribute:NSLayoutAttributeNotAnAttribute multiplier:1.0f constant:_adSize.width]];
        [self.view addConstraint:[NSLayoutConstraint constraintWithItem:bannerView attribute:NSLayoutAttributeHeight relatedBy:NSLayoutRelationEqual toItem:nil attribute:NSLayoutAttributeNotAnAttribute multiplier:1.0f constant:_adSize.height]];
    } else {
        NSLog(@"Banner ad's not ready for placementID:%@", _placementIDs[_name]);
    }
}</code></pre>

###6.5 实现Banner的Delegate###
您可以实现**UPArpuBannerDelegate**的方法来获取banner的各种事件：
<pre><code>-(void) bannerView:(UPArpuBannerView *)bannerView didShowAdWithPlacementID:(NSString *)placementID {
    NSLog(@"UPArpuBannerViewController::bannerView:didShowAdWithPlacementID:%@", placementID);
}

-(void) bannerView:(UPArpuBannerView*)bannerView didClickWithPlacementID:(NSString*)placementID {
    NSLog(@"UPArpuBannerViewController::bannerView:didClickWithPlacementID:%@", placementID);
}

-(void) bannerView:(UPArpuBannerView*)bannerView didCloseWithPlacementID:(NSString*)placementID {
    NSLog(@"UPArpuBannerViewController::bannerView:didCloseWithPlacementID:%@", placementID);
}

-(void) bannerView:(UPArpuBannerView *)bannerView didAutoRefreshWithPlacement:(NSString *)placementID {
    NSLog(@"UPArpuBannerViewController::bannerView:didAutoRefreshWithPlacement:%@", placementID);
}

-(void) bannerView:(UPArpuBannerView *)bannerView failedToAutoRefreshWithPlacementID:(NSString *)placementID error:(NSError *)error {
    NSLog(@"UPArpuBannerViewController::bannerView:failedToAutoRefreshWithPlacementID:%@ error:%@", placementID, error);
}</code></pre>

##7 原生广告(Native)##

在继续接入之前，您需要保证您已经完成了以上 [配置](#1) 步骤。

###7.1 导入Native Framework###
将**UpArpuNative.framework**拖到您的项目中，除了**UpArpuNative.framework**，您还需要集成第三方平台的Adapter，目前**UpArpuSDK**支持以下的平台(对应平台需要导入的Adapter)。

|Third Party Ad Network|Adapter Framework|
|---|---|
|Facebook|UpArpuFacebookNativeAdapter.framework|
|Admob|UpArpuAdmobNativeAdapter.framework|
|Flurry|UpArpuFlurryNativeAdapter.framework|
|Applovin|UpArpuApplovinNativeAdapter.framework|
|GDT|UpArpuGDTNativeAdapter.framework|
|Baidu|UpArpuBaiduNativeAdapter.framework|
|TouTiao|UpArpuTTNativeAdapter.framework|
|Nend|UpArpuNendNativeAdapter.framework|
|AppNext|UpArpuAppNextNativeAdapter.framework|
|Yeahmobi|UpArpuYeahmobiNativeAdapter.framework|
|Oneway|UpArpuOnewayNativeAdapter.framework|
|Mintegral|UpArpuMintegralNativeAdapter.framework|
|Mopub|UpArpuMopubNativeAdapter.framework|

###7.2 加载Native###
您需要确认你添加了**UPArpuNativeADDelegate**代理协议：
<pre><code>@interface UPArpuNativeViewController()\<UPArpuNativeADDelegate\>
//Other properties&methods declarations
@end</code></pre>

加载native:
<pre><code>[[UPArpuAdManager sharedManager] loadADWithPlacementID:@"your native placement id" extra:nil delegate:self];</code></pre>

您可以实现以下的代理方法来获取各种加载事件：
<pre><code>#pragma mark - loading delegate
-(void) didFinishLoadingADWithPlacementID:(NSString *)placementID {
    NSLog(@"Native Demo: didFinishLoadingADWithPlacementID");
}

-(void) didFailToLoadADWithPlacementID:(NSString* )placementID error:(NSError *)error {
    NSLog(@"Native Demo: failed to load:%@", error);
}</code></pre>

###7.3 展示Native###
您可以检查Native广告是否已经ready：
 
<pre><code>-(void) showAD {
    UPArpuNativeADConfiguration *config = [[UPArpuNativeADConfiguration alloc] init];
    config.ADFrame = CGRectMake(.0f, 64.0f, CGRectGetWidth(self.view.bounds), 400.0f);
    config.delegate = self;
    config.renderingViewClass = [DMADView class];
    DMADView *adView = (DMADView*)[[UPArpuAdManager sharedManager] retriveAdViewWithPlacementID:_placementIDs[_name] configuration:config];
    adView.tag = adViewTag;
    [self.view addSubview:adView];
}</code></pre>

####7.3.1 实现Custom Native Ad View####
要展示一个Native广告，您需要定义一个自定义的视图，它需要继承于**UPNativeADView**，并添加**UPNativeRendering**协议。所以需要您去实现某些方法，在我们的Demo中，我们通过添加一些属性，确保协议中的方法可以获取到这些属性。
<pre><code>@interface DMADView:UPArpuNativeADView
@property(nonatomic, readonly) UILabel \*advertiserLabel;
@property(nonatomic, readonly) UILabel \*textLabel;
@property(nonatomic, readonly) UILabel \*titleLabel;
@property(nonatomic, readonly) UILabel \*ctaLabel;
@property(nonatomic, readonly) UILabel \*ratingLabel;
@property(nonatomic, readonly) UIImageView \*iconImageView;
@property(nonatomic, readonly) UIImageView \*mainImageView;
@end</code></pre>

在这里，您的广告视图需要做两件事：<br>
1) 创建广告视图需要展示的UI元素：

通过initSubviews方法创建子试图：
 <pre><code>-(void) initSubviews {
    [super initSubviews];
    \_advertiserLabel = [UILabel autolayoutLabelFont:[UIFont boldSystemFontOfSize:15.0f] textColor:[UIColor blackColor] textAlignment:NSTextAlignmentLeft];
    [self addSubview:\_advertiserLabel];
    \_titleLabel = [UILabel autolayoutLabelFont:[UIFont boldSystemFontOfSize:18.0f] textColor:[UIColor blackColor] textAlignment:NSTextAlignmentLeft];
    [self addSubview:\_titleLabel];
    \_textLabel = [UILabel autolayoutLabelFont:[UIFont systemFontOfSize:15.0f] textColor:[UIColor blackColor]];
    [self addSubview:\_textLabel];
    \_ctaLabel = [UILabel autolayoutLabelFont:[UIFont systemFontOfSize:15.0f] textColor:[UIColor blackColor]];
    [self addSubview:\_ctaLabel];
    \_ctaButton = [UIButton autolayoutButtonWithType:UIButtonTypeCustom];
    [self insertSubview:\_ctaButton aboveSubview:\_ctaLabel];
    \_ratingLabel = [UILabel autolayoutLabelFont:[UIFont systemFontOfSize:15.0f] textColor:[UIColor blackColor]];
    [self addSubview:\_ratingLabel];
    \_iconImageView = [UIImageView autolayoutView];
    \_iconImageView.layer.cornerRadius = 4.0f;
    \_iconImageView.layer.masksToBounds = YES;
    \_iconImageView.contentMode = UIViewContentModeScaleAspectFit;
    [self addSubview:\_iconImageView];
    \_mainImageView = [UIImageView autolayoutView];
    \_mainImageView.contentMode = UIViewContentModeScaleAspectFit;
    [self addSubview:_mainImageView];
    self.mediaView.translatesAutoresizingMaskIntoConstraints = NO;
}</code></pre>

UI元素包括：

| UI name | UI Element | Description |
|----------|----------|-----------|
| titleLabel | UILabel | For the title in the ad assets|
| textLabel | UILabel | For the text in the ad assets|
| ratingLabel | UILabel | For the app rating in the ad assets|
| ctaLabel | UILabel | For the cta text in the ad assets|
| iconImageView | UIImage | For the app icon image in the ad assets|
| mainImageView | UIImage | For the cover image in the ad assets|
| advertiserLabel | UILabel | For showing the advertiser name |
对于视频广告，这里有一个media view播放视频，这是SDK为您创建的。一些第三方在media view中显示他们的cover image，也有一些同时展示media view和image view。因此，建议您在子试图中添加image view展示在cover image的索引0的位置，防止覆盖掉media view的播放。

2) UI元素的布局要与你自己的app设计风格相匹配

在我们的示例代码中，我们使用的**autolayout**方法是在**UIView**的分类中实现，他们是原生**cocoa touch**的简单封装，您可以查看**MTAutolayoutCategories**类别，当使用autolayout时，建议您重写makeConstraintsForSubviews并编写布局代码:

<pre><code>-(void) makeConstraintsForSubviews {
    [super makeConstraintsForSubviews];
    NSDictionary *viewsDict = nil;
    if (self.mediaView != nil) {
        viewsDict = @{@"titleLabel":self.titleLabel, @"textLabel":self.textLabel, @"ctaLabel":self.ctaLabel, @"ratingLabel":self.ratingLabel, @"iconImageView":self.iconImageView, @"mainImageView":self.mainImageView, @"mediaView":self.mediaView, @"advertiserLabel":self.advertiserLabel};
    } else {
        viewsDict = @{@"titleLabel":self.titleLabel, @"textLabel":self.textLabel, @"ctaLabel":self.ctaLabel, @"ratingLabel":self.ratingLabel, @"iconImageView":self.iconImageView, @"mainImageView":self.mainImageView, @"advertiserLabel":self.advertiserLabel};
    }
    [self addConstraintsWithVisualFormat:@"|[mainImageView]|" options:0 metrics:nil views:viewsDict];
    [self addConstraintsWithVisualFormat:@"V:[iconImageView][mainImageView]|" options:0 metrics:nil views:viewsDict];
    [self addConstraintWithItem:self.iconImageView attribute:NSLayoutAttributeWidth relatedBy:NSLayoutRelationEqual toItem:self.iconImageView attribute:NSLayoutAttributeHeight multiplier:1.0f constant:.0f];
    [self addConstraintsWithVisualFormat:@"|-15-[iconImageView(90)]-8-[titleLabel]-15-|" options:NSLayoutFormatAlignAllTop metrics:nil views:viewsDict];
    [self addConstraintsWithVisualFormat:@"V:|-15-[titleLabel]-8-[textLabel]-8-[ctaLabel]-8-[ratingLabel]-8-[advertiserLabel]" options:NSLayoutFormatAlignAllLeading | NSLayoutFormatAlignAllTrailing metrics:nil views:viewsDict];
    if (self.mediaView != nil) {
        [self addConstraintsWithVisualFormat:@"|[mediaView]|" options:0 metrics:nil views:viewsDict];
        [self addConstraintsWithVisualFormat:@"V:[iconImageView]-[mediaView]|" options:0 metrics:nil views:viewsDict];
    }
}</code></pre>

您也可以使用**Masonary**开源布局工具，此外还有**struts&springs**布局技术，使用该方法的时候，建议您重写layoutSubviews方法，并给您的subviews设置frames。
使用何种布局技术完全取决于您，可以根据您的习惯任意选择，it‘s up to you。

####7.3.2 使用您的Custom Native Ad View展示Native####
展示广告之前，您需要先创建一个**UPNativeADConfiguration**实例，设置您想要的广告大小。定制广告视图的类，也可以用来去实现delegate获取各种展示的事件，之后你可以调用**UPArpuAdManager**的单例方法**retriveAdViewWithPlacementID:configuration:**并带上placementid会返回一个您准备的广告视图对象，您只需要将其添加到您想要展示广告的视图之上：


<pre><code>-(void) showAD {
    UPArpuNativeADConfiguration *config = [[UPArpuNativeADConfiguration alloc] init];
    config.ADFrame = CGRectMake(.0f, 64.0f, CGRectGetWidth(self.view.bounds), 400.0f);
    config.delegate = self;
    config.renderingViewClass = [DMADView class];
    DMADView *adView = (DMADView*)[[UPArpuAdManager sharedManager] retriveAdViewWithPlacementID:_placementIDs[_name] configuration:config];
    adView.tag = adViewTag;
    [self.view addSubview:adView];
}</code></pre>

###7.4 实现Native的Delegate###
您可以实现**UPArpuNativeDelegate**的方法来获取banner的各种事件：

<pre><code>//Called when user click the ad
-(void) didClickNativeAdInAdView:(UPArpuNativeADView*)adView placementID:(NSString*)placementID {
    NSLog(@"did click native ad with placement id:%@", placementID);
}
//Called when the ad has been shown
-(void) didShowNativeAdInAdView:(UPArpuNativeADView*)adView placementID:(NSString*)placementID {
    adView.mainImageView.hidden = [adView isVideoContents];
}</code></pre>

##8 (原生Banner)Native Banner##
在继续接入之前，您需要保证您已经完成了以上 [配置](#1) 步骤。

###8.1 导入Native Framework###
将**UpArpuNative.framework**拖到您的项目中，除了**UpArpuNative.framework**，您还需要集成第三方平台的Adapter，目前**UpArpuSDK**支持以下的平台(对应平台需要导入的Adapter)。

|Third Party Ad Network|Adapter Framework|
|---|---|
|Facebook|UpArpuFacebookNativeAdapter.framework|
|Admob|UpArpuAdmobNativeAdapter.framework|
|Flurry|UpArpuFlurryNativeAdapter.framework|
|Applovin|UpArpuApplovinNativeAdapter.framework|
|GDT|UpArpuGDTNativeAdapter.framework|
|Baidu|UpArpuBaiduNativeAdapter.framework|
|TouTiao|UpArpuTTNativeAdapter.framework|
|Nend|UpArpuNendNativeAdapter.framework|
|AppNext|UpArpuAppNextNativeAdapter.framework|
|Yeahmobi|UpArpuYeahmobiNativeAdapter.framework|
|Oneway|UpArpuOnewayNativeAdapter.framework|
|Mintegral|UpArpuMintegralNativeAdapter.framework|
|Mopub|UpArpuMopubNativeAdapter.framework|

###8.2 加载Native Banner###
您需要确认你添加了**UPArpuNativeBannerDelegate**代理协议：
<pre><code>@interface UPArpuNativeBannerViewController()\<UPArpuNativeBannerDelegate\>
//Other properties&methods declarations
@end</code></pre>

加载NativeBanner广告:
<pre><code>[UPArpuNativeBannerWrapper loadNativeBannerAdWithPlacementID:_placementID extra:nil customData:nil delegate:self];</code></pre>

###8.3 展示Native Banner###
在您NativeBanner加载完成之后，您可以调用API去展示NativeBanner：
 
<pre><code>-(void) showAd {
    UPArpuNativeBannerView *bannerView = [UPArpuNativeBannerWrapper retrieveNativeBannerAdViewWithPlacementID:_placementID extra:@{kUPArpuNativeBannerAdShowingExtraAdSizeKey:[NSValue valueWithCGSize:CGSizeMake(CGRectGetWidth([UIScreen mainScreen].bounds), 80.0f)], kUPArpuNativeBannerAdShowingExtraAutorefreshIntervalKey:@10.0f, kUPArpuNativeBannerAdShowingExtraHideCloseButtonFlagKey:@NO, kUPArpuNativeBannerAdShowingExtraCTAButtonBackgroundColorKey:[UIColor redColor], kUPArpuNativeBannerAdShowingExtraCTAButtonTitleColorKey:[UIColor whiteColor], kUPArpuNativeBannerAdShowingExtraCTAButtonTitleFontKey:[UIFont systemFontOfSize:12.0f], kUPArpuNativeBannerAdShowingExtraTitleColorKey:[UIColor grayColor], kUPArpuNativeBannerAdShowingExtraTitleFontKey:[UIFont systemFontOfSize:12.0f], kUPArpuNativeBannerAdShowingExtraTextColorKey:[UIColor lightGrayColor], kUPArpuNativeBannerAdShowingExtraTextFontKey:[UIFont systemFontOfSize:10.0f], kUPArpuNativeBannerAdShowingExtraBackgroundColorKey:[UIColor whiteColor], kUPArpuNativeBannerAdShowingExtraAdvertiserTextFontKey:[UIFont systemFontOfSize:12.0f], kUPArpuNativeBannerAdShowingExtraAdvertiserTextColorKey:[UIColor lightGrayColor]} delegate:self];
    bannerView.frame = CGRectMake(.0f, 100.0f, CGRectGetWidth(bannerView.bounds), CGRectGetHeight(bannerView.bounds));
    bannerView.autoresizingMask = UIViewAutoresizingFlexibleWidth;
    [self.view addSubview:bannerView];
}</code></pre>

###8.4 实现Native Banner的Delegate###
<pre><code>#pragma mark - native banner delegate(s)
-(void) didFinishLoadingNativeBannerAdWithPlacementID:(NSString *)placementID {
    NSLog(@"UPArpuNativeBannerViewController::didFinishLoadingNativeBannerAdWithPlacementID:%@", placementID);
}

-(void) didFailToLoadNativeBannerAdWithPlacementID:(NSString*)placementID error:(NSError*)error {
    NSLog(@"UPArpuNativeBannerViewController::didFailToLoadNativeBannerAdWithPlacementID:%@ error:%@", placementID, error);
}

-(void) didShowNativeBannerAdInView:(UPArpuNativeBannerView*)bannerView placementID:(NSString*)placementID {
    NSLog(@"UPArpuNativeBannerViewController::didShowNativeBannerAdInView:%@ placementID:%@", bannerView, placementID);
}

-(void) didClickNativeBannerAdInView:(UPArpuNativeBannerView*)bannerView placementID:(NSString*)placementID {
    NSLog(@"UPArpuNativeBannerViewController::didClickNativeBannerAdInView:%@ placementID:%@", bannerView, placementID);
}

-(void) didClickCloseButtonInNativeBannerAdView:(UPArpuNativeBannerView*)bannerView placementID:(NSString*)placementID {
    NSLog(@"UPArpuNativeBannerViewController::didClickCloseButtonInNativeBannerAdView:%@ placementID:%@", bannerView, placementID);
}

-(void) didAutorefreshNativeBannerAdInView:(UPArpuNativeBannerView*)bannerView placementID:(NSString*)placementID {
    NSLog(@"UPArpuNativeBannerViewController::didAutorefreshNativeBannerAdInView:%@ placementID:%@", bannerView, placementID);
}

-(void) didFailToAutorefreshNativeBannerAdInView:(UPArpuNativeBannerView*)bannerView placementID:(NSString*)placementID error:(NSError*)error {
    NSLog(@"UPArpuNativeBannerViewController::didFailToAutorefreshNativeBannerAdInView:%@ placementID:%@ error:%@", bannerView, placementID, error);
}</code></pre>

##9 (原生Splash)Native Splash##
在继续接入之前，您需要保证您已经完成了以上 [配置](#1) 步骤。

###9.1 导入 Native Framework###
Native Splash是基于Native实现的，所以你需要导入同样的**UpArpuNative.framework**到您的项目中，除了**UpArpuNative.framework**，您还需要集成第三方平台的Adapter，目前**UpArpuSDK**支持以下的平台(对应平台需要导入的Adapter)。

|Third Party Ad Network|Adapter Framework|
|---|---|
|Facebook|UpArpuFacebookNativeAdapter.framework|
|Admob|UpArpuAdmobNativeAdapter.framework|
|Flurry|UpArpuFlurryNativeAdapter.framework|
|Applovin|UpArpuApplovinNativeAdapter.framework|
|GDT|UpArpuGDTNativeAdapter.framework|
|Baidu|UpArpuBaiduNativeAdapter.framework|
|TouTiao|UpArpuTTNativeAdapter.framework|
|Nend|UpArpuNendNativeAdapter.framework|
|AppNext|UpArpuAppNextNativeAdapter.framework|
|Yeahmobi|UpArpuYeahmobiNativeAdapter.framework|
|Oneway|UpArpuOnewayNativeAdapter.framework|
|Mintegral|UpArpuMintegralNativeAdapter.framework|
|Mopub|UpArpuMopubNativeAdapter.framework|

###9.2 加载Native Splash###
您需要确认你添加了**UPArpuNativeSplashDelegate**代理协议：
<pre><code>@interface UPArpuNativeSplashViewController()\<UPArpuNativeSplashDelegate\>
//Other properties&methods declarations
@end</code></pre>

加载native splash广告:
<pre><code>[UPArpuNativeSplashWrapper loadNativeSplashAdWithPlacementID:@"native splash placement id" extra:@{kExtraInfoNativeAdTypeKey:@(UPArpuGDTNativeAdTypeSelfRendering), kExtraInfoNativeAdSizeKey:[NSValue valueWithCGSize:CGSizeMake(CGRectGetWidth(self.view.bounds) - 30.0f, 400.0f)], kUPArpuExtraNativeImageSizeKey:kUPArpuExtraNativeImageSize690_388, kUPArpuNativeSplashShowingExtraCountdownIntervalKey:@3} customData:nil delegate:self];</code></pre>

###9.3 展示Native Splash###
在您Native Splash加载完成之后，您可以调用API去展示banner：
 
<pre><code>-(void) showAd {
    CGFloat width = CGRectGetWidth([UIScreen mainScreen].bounds);
    UILabel *label = [[UILabel alloc] initWithFrame:CGRectMake(.0f, .0f, width, 79.0f)];
    label.textAlignment = NSTextAlignmentCenter;
    label.backgroundColor = [UIColor whiteColor];
    label.text = @"Joypac";
    [UPArpuNativeSplashWrapper showNativeSplashAdWithPlacementID:placementID extra:@{kUPArpuNatievSplashShowingExtraStyleKey:kUPArpuNativeSplashShowingExtraStylePortrait, kUPArpuNativeSplashShowingExtraCountdownIntervalKey:@3, kUPArpuNativeSplashShowingExtraContainerViewKey:label} delegate:self];
}</code></pre>

###9.4 实现Native Splash的Delegate###
您可以实现**UPArpuNativeSplashDelegate**的方法来获取Splash的各种事件：

<pre><code>-(void) finishLoadingNativeSplashAdForPlacementID:(NSString*)placementID {
NSLog(@"ViewController::finishLoadingNativeSplashAdForPlacementID:%@", placementID);
    
}

-(void) failedToLoadNativeSplashAdForPlacementID:(NSString*)placementID error:(NSError*)error {
    NSLog(@"ViewController::failedToLoadNativeSplashAdForPlacementID:%@ error:%@", placementID, error);
}

-(void) didShowNativeSplashAdForPlacementID:(NSString*)placementID {
    NSLog(@"ViewController::didShowNativeSplashAdForPlacementID:%@", placementID);
}

-(void) didClickNaitveSplashAdForPlacementID:(NSString*)placementID {
    NSLog(@"ViewController::didClickNaitveSplashAdForPlacementID:%@", placementID);
}

-(void) didCloseNativeSplashAdForPlacementID:(NSString*)placementID {
    NSLog(@"ViewController::didCloseNativeSplashAdForPlacementID:%@", placementID);
}</code></pre>


##10 头部竞价(Header Bidding)##

应用内header bidding是一种先进的程序化广告竞价技术，允许所有需求方针对同一个广告展示同时竞价，最高出价者获得展示机会，这确保发布商的每次展示可以获得更高的收益。目前TopOn平台支持Mintegral和Facebook的应用内header bidding。

Mintegral和Facebook支持header bidding的应用版本如下：

|广告平台|操作系统|支持广告类型|广告平台的SDK版本号|额外的SDK|
|---|---|---|---|---|
|Facebook|iOS|原生, 激励视频, 插屏|4.99.x – 5.2.x|FBAudienceNetworkBiddingKit-5.0.0-beta|
|Facebook|iOS|原生, 激励视频, 插屏|>= 5.3.x| FBAudienceNetworkBiddingKit-5.3.1-beta|
|Facebook|Android|原生, 激励视频, 插屏|>= 4.99.x|bidding-kit-sdk-5.0.1|
|Mintegral|iOS|原生, 激励视频, 插屏视频|>= 5.4.2|None|
|Mintegral|Android|原生, 激励视频, 插屏视频|>= 9.12.4|None|

注：Facebook的应用内header bidding需要引外额外的SDK。

##11 通用数据保护条例GDPR##

<span style="font-family:‘Times New Roman‘;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>欧盟发布的**《通用数据保护条例》（GDPR）**将于 2018 年 5 月 25 日生效。 为支持GDPR协议我们更新了**<i>UPARPU Privacy Policy</i>**，请开发者从我们官网了解<a href="https://www.uparpu.com/privacy-policy" target = "_blank">**<i>UPARPU Privacy Policy</i>**</a>的相关内容。同时，为保障用户数据的隐私安全，我们在新版的UPARPU SDK v1.2及以上中加入了数据保护功能，请开发者查阅以下文档并完成SDK接入。<br>
<span style="font-family:‘Times New Roman‘;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>我们提供了两种方法给开发者设置GDPR配置。你可以调用UPARPU SDK的方法来为所有网络设置统一的数据保护级别，也可以分别为各网络设置数据保护级别；如果是后者，传入的数据结构需与第三方网络的要求一致而且这些数据结构在未来可能会发生改变。<br>
<h3>4.1 使用UPARPU SDK方法</h3>
<span style="font-family:‘Times New Roman‘;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>你可以调用UPARPU SDK里**UPArpuAPI**的单例中的**setDataConsentSet:consentString:**方法来设置GDPR级别，其中consentString参数是为Flurry预留的。UPArpu SDK提供了四个级别的数据保护：<br>
<span style="font-family:‘Times New Roman‘;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>1) UpArpuDataConsentSetUnknown(0)<br>
<span style="font-family:‘Times New Roman‘;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>这是默认值，当开发者未设置时采用此值；这种情况下，如果用户在GDPR区域内，SDK初始化将失败，后续广告请求也会因为失败。<br>
<span style="font-family:‘Times New Roman‘;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>2) UpArpuDataConsentSetPersonalized(1)<br>
<span style="font-family:‘Times New Roman‘;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>这个级别代表用户同意SDK收集并使用他的个人数据来为他提供相关性更高、更适合他的广告。<br>
<span style="font-family:‘Times New Roman‘;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>3) UpArpuDataConsentSetNonpersonalized(2)<br>
<span style="font-family:‘Times New Roman‘;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>如果数据保护级别设置为这个值，SDK不会收集用户个人数据，因为提供的广告可能不会符合用户的情况。另外，在这种情况下，某些不涉及用户隐私的数据可能仍会被收集。<br>
<span style="font-family:‘Times New Roman‘;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>4) UpArpuDataConsentSetForbidden(3)<br>
<span style="font-family:‘Times New Roman‘;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>禁止收集任何数据，SDK初始化将失败，广告请求将不会发起。<br>

###4.2 Setting data consent separately###
以上四种值为枚举类型，你可以设置每一个平台的数据接受度信息，根据不同平台的规范，您应该为平台提供如下的信息：<br>
**Mintegral**: 您可以以@0(上述枚举类型)作为key设置@YES或者@NO，以此来收集三种类型的数据，例如(@0，@yes)，(@1:yes ,@2:no ,@3:@yes)。有关详情，情浏览其官方网站。<br>
  **Inmobi**: BOOL被包装成NSNumber<br>
  **Mopub**: BOOL被包装成NSNumber<br>
  **Admob**: 字典类型，包含键值:<br>
        1) 在kAdmobConsentStatusKey为key，将NSInteger作为value，指接受度状态(0=unknown, 1=non personalized or 2=personalized)<br>
        2) kAdmobUnderAgeKey为key，将BOOL作为value，指开发者是否知道用户年龄低于协议要求<br>
  **Applovin**: 字典类型，包含键值:<br>
        1) BOOL作为value，表示用户是否同意在协议下与AppLovin共享信息。<br>
        2) BOOL作为value，表示拥护是否受年龄限制d<br>
  **Flurry**: 字典类型，包含键值:<br>
        1) BOOL作为value，表示拥护是否在GDPR内,<br>
        2) 字典类型<br>
        请详细参考[Flurry's develper website](https://developer.yahoo.com/flurry/docs/integrateflurry/ios/)。<br>
  **Tapjoy**: 字典类型，包含键值 (请详细参考Tapjoy官网):<br>
        1) 字符串设置为“0”(用户未选择接受度)、“1”(用户已选择接受度)或填写IAB的可见的协议框架中给出的字符串。<br>
        2) BOOL作为value，当用户适用于GDPR规则时，该值应设置为YES，反之则NO，在没有设置该值的情况下，Tapjoy会使用默认的设置。<br>
  **Chartboost**: BOOL作为value，限制Chartboost从设备收集个人数据，当这个设置为YES时，SDK或服务器不会收集设备IDFA和IP地址，并以此来传达EEU数据主体。<br>
  **Vungle**: NSInteger:<br>
          1 (推荐)开发者在用户层面控制GDPR的设置，然后将用户选择传递给Vungle，为此开发者可以使用自己的机制去收集用户的接受度，然后使用Vungle的API更新或查询用户的接受度状态<br>
          2 允许Vungle处理请求，Vungle将为在欧盟的用户播放广告前显示一个同意对话框，并记住用户对后续广告的接受程度。<br>
  **IronSource**: [IronSource's offical website](https://developers.ironsrc.com/ironsource-mobile/ios/advanced-settings/#step-1) 。<br>
 **AdColony**: 字典类型，包含键值:<br>
         1) BOOL作为value 表示通知AdColony服务器，如果GDPR应该考虑喂用户基于他们是否是欧盟用户或来自欧盟地区，喂了遵守GDPR，默认为NO。[AdColony website](https://www.adcolony.com/gdpr/).<br>
         2) NSString定义终端收集用户的数据接受度，IAB可见的协议框架中定义了用于在协议管理平台(CMPs)之间进行通信的标准API和format，这些API和format收集网络或手机应用程序中的用户最终和供应商达成的协议信息。为了通过IAB遵守GDPR，这些API为开发者集成提供了统一的接口，其中CMPs和供应商不需要与数百个合作伙伴交互集成。[website](https://github.com/InteractiveAdvertisingBureau/GDPR-Transparency-and-Consent-Framework/blob/master/v1.1%20Implementation%20Guidelines.md#vendors).<br>
  **UnityAds**: BOOL作为value.

你可以为各个网络单独设置数据保护级别；<br>
这里提供一个例子：<br> {<br>
            kNetworkNameMobvista:@{@1:@YES, @2:@YES, @3:@NO},<br>
            kNetworkNameInmobi:@YES,<br>
            kNetworkNameMopub:@NO,<br>
            kNetworkNameAdmob:@{kAdmobConsentStatusKey:@1, kAdmobUnderAgeKey:@NO},<br>
            kNetworkNameApplovin:@{kApplovinConscentStatusKey:@YES, kApplovinUnderAgeKey:@NO<br>}<br>
           <br>
具体设置方法请参见[第三方网络配置](http://www.uparpu.com)

