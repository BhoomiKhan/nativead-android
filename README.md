# nativead-android
This is a sample project to implement AdMob Native Ads in android application using JAVA &amp; XML. Code copied from google admob documentation https://developers.google.com/admob/android/native/start

## Implementation of AdMob Native Ads into Android Application

You can implement native ads on all devices having following properties
- Android Studio 3.2 or higher
- minSdkVersion 16 or higher
- compileSdkVersion 28 or higher


## Steps to integrate native ads into android app

# 1. Add Dependency into gradle.build (app) file 

```
    implementation 'com.google.android.gms:play-services-ads:19.3.0'
```

## 2. Add meta tag into AndroidManifest.xml application tag

```
<application>
        <!-- Sample AdMob App ID: ca-app-pub-3940256099942544~3347511713 -->
        <meta-data
            android:name="com.google.android.gms.ads.APPLICATION_ID"
            android:value="ca-app-pub-xxxxxxxxxxxxxxxx~yyyyyyyyyy"/>
</application>
```
## 3. Initialize Ads SDK in onCreate method

```
protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        MobileAds.initialize(this, new OnInitializationCompleteListener() {
            @Override
            public void onInitializationComplete(InitializationStatus initializationStatus) {
            }
        });
    }
```

## 4. This code is spesifically relates to Native Ads, create an object of AdLoader Class to load the ads to display, this object is normally created in onCreate method to load timely ads 

```
AdLoader adLoader = new AdLoader.Builder(context, "ca-app-pub-3940256099942544/2247696110")
    .forUnifiedNativeAd(new UnifiedNativeAd.OnUnifiedNativeAdLoadedListener() {
        @Override
        public void onUnifiedNativeAdLoaded(UnifiedNativeAd unifiedNativeAd) {
            // Show the ad.
        }
    })
    .withAdListener(new AdListener() {
        @Override
        public void onAdFailedToLoad(LoadAdError adError) {
            // Handle the failure by logging, altering the UI, and so on.
        }
    })
    .withNativeAdOptions(new NativeAdOptions.Builder()
            // Methods in the NativeAdOptions.Builder class can be
            // used here to specify individual options settings.
            .build())
    .build();
```

These callback methods will trigger when Ad-SDK will send a response, onUnifiedNativeAdLoaded() method will call when ad will be loaded and onAdFailedToLoad() mehtod will show the error. It's good practice to handle the exception in onAdFailedToLoad() method. ca-app-pub-3940256099942544/2247696110 is the id of ad unit, it's good practice to use test id when your app is under development, you can find test ids from [this link](https://developers.google.com/admob/android/test-ads). Before publishing your app on google play store, make sure you have replaced test id with the original ad unit id from your admob account. You can find your ad unit as follows Login into Admob Account -> Apps -> Ad Units 


## 4. [Download native ad template](https://github.com/googleads/googleads-mobile-android-native-templates) and import into your project as module. You can follow the steps to add a module into your project

## 5. Add this Templateview into your xml file which will be responsible to show ads. It's same like other views that show screen content.

```
<!--  This is your template view -->
<com.google.android.ads.nativetemplates.TemplateView
   android:id="@+id/my_template"
   <!-- this attribute determines which template is used. The other option is
    @layout/gnt_medium_template_view -->
   app:gnt_template_type="@layout/gnt_small_template_view"
   android:layout_width="match_parent"
   android:layout_height="match_parent" />
   
 ```

## 6. Add this code into onUnifiedNativeAdLoaded() to bind ads with templateview 

```
TemplateView template = findViewById(R.id.my_template);
          template.setStyles(styles);
          template.setNativeAd(unifiedNativeAd);
```


## 7. Add this final line of code to show ads 

``` 
   adLoader.loadAd(new AdRequest.Builder().build());
```


This guideline is taken from [this document](https://developers.google.com/admob/android/quick-start) you can contribute into this code to make it more mature. Thank you
