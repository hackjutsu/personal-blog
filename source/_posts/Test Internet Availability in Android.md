# Test Internet Availability in Android

title:  Test Internet Availability in Android
date: 2015-11-30 21:26:00
tags:
- Android

categories:
- Android

---

Here is a convenient method to check whether an Android device is connected to the Internet. Note that not all network has Internet access, for example, we could be further asked to sign-in the Starbucks' wifi. This method not only checks the connection to a network, but also checks whether this network routes to the Internet.  

<!--more-->

Add the following permission to the `Manifest.xml` file.
``` xml
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

Create the `ConnectionDetector` class.
``` java
public class ConnectionDetector {
    private Context _context;

    public ConnectionDetector(Context context) {
        this._context = context;
    }

    public boolean isConnectingToInternet() {
        if (networkConnectivity()) {
            try {
                HttpURLConnection urlc = (HttpURLConnection) (new URL(
                        "http://www.google.com").openConnection());
                urlc.setRequestProperty("User-Agent", "Test");
                urlc.setRequestProperty("Connection", "close");
                urlc.setConnectTimeout(3000);
                urlc.setReadTimeout(4000);
                urlc.connect();
                // networkcode2 = urlc.getResponseCode();
                return (urlc.getResponseCode() == 200);
            } catch (IOException e) {
                return (false);
            }
        } else
            return false;
    }

    private boolean networkConnectivity() {
        ConnectivityManager cm = (ConnectivityManager) _context
                .getSystemService(Context.CONNECTIVITY_SERVICE);
        NetworkInfo networkInfo = cm.getActiveNetworkInfo();
        if (networkInfo != null && networkInfo.isConnected()) {
            return true;
        }
        return false;
    }
}
```
Then call like this.
``` java
if ((new ConnectionDetector(MyService.this)).isConnectingToInternet()) {
    Log.d("internet status", "Internet Access");
} else {
    Log.d("internet status", "no Internet Access");
}
```