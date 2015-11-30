meta-tags: OpenFL
meta-publishedOn: 2015-01-15

I recently [published a HaxeFlixel/OpenFL game to Google Play](https://play.google.com/store/apps/details?id=com.deengames.thisismylord). During the process, I navigated a few issues, which I document here for future benefit.

### Signing Your APK

As you know, every Android app must be signed with a private key. The [process to generate a keystore](http://stackoverflow.com/a/15330139/210780) is well-known. All I can re-iterate is: **please keep your keystore and alias safe.** You will need to know them, and you cannot publish your app without them, nor can you reverse-engineer them from existing apps.

If you forgot your app alias, you can actually open the `keystore` file in a text editor, and you'll see it in plaintext (while the rest of the file is binary).

To automatically sign your APK, add your `keystore` file to the local directory (with `project.xml`) and add this to your `project.xml` file:

```
<certificate
  path="android_release.keystore"
  alias="deengamesproductionkey"
  if="android" />
```

If you run the `android` build, the SDK will prompt you to enter your password. If your source-control repository is private, you can commit your `keystore` file and add this attribute to the `certificate` to not be prompted each build: `password="put your keystore password here"`.

### Specifying the Package Name

Google Play requires a unique package identifier (eg. `com.deengames.thisismylord`) to uniquely identify each game. The default package generates as `com.example.myapp`. To change it, open up `project.xml` and add this attribute to the `app` tag:

`package="com.deengames.thisismylord"`

You can also specify a `company` (eg. `Deen Games`) and a human-readable `version` (eg. `1.0.1`). I suggest using [semantic versioning](http://semver.org/).

### The Generated AndroidManifest.xml

As you may know (if you published Android apps), Google Play requires you to specify special permissions your app needs (the entire list is [in the Manifest permissions doc](http://developer.android.com/reference/android/Manifest.permission.html)).

OpenFL automatically generates the android manifest XML file for you. For fine-grained control, you can overwrite the entire file with your own, like so:

`<template path="to/your/manifest.xml" rename="bin/AndroidManifest.xml" />`

However, I don't recommend this, for a couple of reasons:

- The default generated manifest is quite good, and guaranteed to work with all OpenFL functionality. Changing something may break that.
- If future versions of OpenFL generate different XML, you may find weird bugs with your app once you run it on Android, because of your mismstached manifest file.

If you *must* do this, *please* check in your manifest.xml to source control and don't upgrade your version of OpenFL. This guarantees that things will keep working.

The generated Android manifest also has a `versionCode` in it. You can specify it in your template (but I don't recommend it, because you have to remember to update it every time you publish.)

If you prefer a better approach, read the next two sections on permissions and the APK version.

### Declaring Permissions

OpenFL allows you to specify Android permissions in your `project.xml`, like so:

`<android permission="com.android.vending.CHECK_LICENSE" />`

Some functionality, like Flurry Analytics, requires certain permissions. By default, OpenFL (currently, as of version `2.1.7`) specifies some permissions by default.

### Specifying the VersionCode

If yo upload your app now, you're fine. But, if you need to upload an upgraded version, you may find a mysterious error that `version code must be greater than n` (eg. 68).

It turns out that OpenFL creates a file in `export` called `.build` which contains an auto-incremented build number; this is precisely the number that it uses in the generated Android manifest. If you work on one machine and never delete `export`, this doesn't matter.

But if you clean `export` or work on multiple machines, you get inconsistent builds. Your solutions are either:

- Check in this file (not necessarily a good, or bad, approach), or
- Update this version by hand whenever you need to publish, or
- Override the android manifest.

I prefer the second approach: let this file exist on it's own, but update it by hand if the version ever drops.

### In Conclusion

All this information is current as of OpenFL 2.1.8. Things may change in the future, so check before you do anything.
