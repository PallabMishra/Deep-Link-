# Deep-Link

Deeplinks are a concept that help users navigate between the web and applications. They are basically URLs which navigate users directly to the specific content in applications.


On the other hand, Android App Links allow an app to designate itself as the default handler of application domain or URL. Unfortunately It works only on on Android 6.0 (API level 23) and higher.

A Scenario For Deep Links to App Content
   a) When a user clicked URL, the Android system tries each of the following actions, in sequential order, until the request succeed.
   b) Open the user’s preferred app that can handle the URI, if one is designated.
   c) Open the only available app that can handle the URI.
   d) Allow the user to select an app from a dialog.
   
How to define intent filters:-

1) When we talk about handling how to navigate users directly to specific content in applications, we should think about adding an intent filter in our manifest file. An intent filter should contain the following elements and attribute values;
Define ACTION_VIEW intent action so that the intent filter can be reached from Google Search.

        <action android:name="android.intent.action.VIEW" />

2) We should include the BROWSABLE category in order to be accessible from a web browser. We should also have DEFAULT category for responding to implicit intents

       <category android:name="android.intent.category.DEFAULT" />
       <category android:name="android.intent.category.BROWSABLE" />
       
3) Lastly, We should define one or more <data> tags. Each of these tags represents a URI format that resolves to the activity. The following example represents a simple data tag for "app" Android app.

       <data
           android:host="app"
           android:scheme="https" />
           
           
4) How to read data from incoming intents

When you define your intent filter that can handle specific URLs, the system can start your activity via that intent filter.

       Intent intent = getIntent();
       Uri data = intent.getData();
       
*** What is the difference between deep links and app links?

   A deep link is an intent filter system that allows users to directly enter a specific activity in an Android app. However there is an issue about this process. When a user click an URL, it might open a dialog which asks the user to select one of multiple apps handling the given URL.
   On the other hand, An Android App Link is a deep link based on your website URL that has been verified to belong to your website. When user clicks that URL, it opens your app.


***Verify Android App Links
   When we want to implement Android App Links to our app, we need to define intent filters using HTTP URLs. In addition to that, We should declare that we own both the app and URLs.


      <intent-filter>
                <action android:name="android.intent.action.VIEW" />

                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />

                <data
                    android:scheme="https"
                    android:host="app" />
       </intent-filter>
       
       
 For example, When user click on a link that contains detail information a random product, we get a productID result from our backend service, use that productID to navigate ProductDetailActivity.
 
 
    @Override
    public boolean isSatisfiedBy(DeepLinkData data) {
        return data.hasProduct();
    }

    @Override
    public void execute(Context context, DeepLinkData data) {
        intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
        context.startActivity(intent);
    }
}

