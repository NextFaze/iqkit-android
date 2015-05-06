iQKit Android SDK
-----------------

The iQKit Android SDK gives you access to the powerful iQNECT Vision Search platform
to integrate into your Android app.

Installation
------------

#### With Gradle

add to your build.gradle:

```
    dependencies {
        compile 'org.iqnect.iqkit:iqkit-core:0.1.0@aar'
        compile 'org.iqnect.iqkit:iqkit-ui:0.1.0@aar'
    }
```

add to your top level (project) build.gradle:

```
    allprojects {
        repositories {
            flatDir {
                dirs "${rootDir}/libs"
            }
        }
    }
```

copy the iqkit aar files into a "libs" folder in your project.

#### Manually


Initialization
---------------

#### Required

In your Application class, initialise the SDK with the App ID and secret you obtained from the [iQNECT Developer Portal](http://developer.iqnect.org):

```java
IQKit.init(context, "APP-ID", "APP-SECRET");
```

#### Optional

For best results, report some further information about the user:

```java
IQKit.getInstance().setUserAge(42);
IQKit.getInstance().setUserGender("f");
```

The age and gender can be updated at anytime with these methods.

Usage
-----

To initiate a vision search, add the following code to an appropriate activity:

```java
    ScannerActivity.start(this);
```

When the search is complete, you will get a callback via the `onActivityResult` method:

```java
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);

        ScannerActivity.handleScannerResult(requestCode, resultCode, data, new ScannerActivity.ScannerResultHandler() {
            @Override
            public void onSearchResult(@Nullable SearchResult searchResult) {
                if (searchResult != null) {
                    WebActivity.start(MainActivity.this, searchResult.getSearchbarTitle(), searchResult.getPayloadUri());
                } else {
                    // no result from image search
                }
            }
        });
    }
```

In the callback, the `SearchResult` object has a `payloadUri` property which is the URL returned by the search. Use this URL, typically to load in a web view or to open the iQNECT app.