Add dependencies to build.gradle.kts (:app)
```
implementation (libs.gms.play.services.ads)
implementation(platform(libs.firebase.bom))
```	
Manifest File:
```
	<uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>


	<meta-data
        android:name="com.google.android.gms.ads.APPLICATION_ID"
        android:value="@string/app_id"/>
```		
strings.xml File: (Created from AdMob Google)
Sometimes there may be problem with ad display. In taht case use universal ad_unit_id from google ca-app-pub-3940256099942544/6300978111 irrespective of app_id or package
```
<resources>
    <string name="app_name">AddSampleBanner</string>
    <string name="app_id">ca-app-pub-8071855784360484~3118039738</string>
    <string name="ad_unit_id">ca-app-pub-8071855784360484/1667827899</string>
    <string name="ad_unit_id_universal">ca-app-pub-3940256099942544/6300978111</string>
</resources>
```
Layout: (Note: app:adSize="BANNER" and app:adUnitId="@string/ad_unit_id_universal" required)
```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center"
    tools:context=".MainActivity">

    <com.google.android.gms.ads.AdView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/adView"
        app:adSize="BANNER"
        app:adUnitId="@string/ad_unit_id_universal"/>


</RelativeLayout>
```
MainActivity.java File:
```
public class MainActivity extends AppCompatActivity {
    AdView adView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);

        adView = findViewById(R.id.adView);

        //Step 1
        MobileAds.initialize(this);

        //Step 2
        AdRequest adRequest = new AdRequest.Builder().build();

        //Step 3
        adView.loadAd(adRequest);

        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });
    }
}
```
