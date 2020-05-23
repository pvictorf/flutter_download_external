# 1. For this exemple I using Android, you need configure your AndroidManifest. Set user permission:
```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android" package="com.example.your_packge">

    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    ...

</manifest>
```

# 2. Next step install this libs on package.yml
```
dio: any
path_provider: ^1.6.8
permission_handler: ^5.0.0+hotfix.6
open_file: ^3.0.1
ext_storage: ^1.0.3
```

# 3. In main.dart import the files
```
import 'dart:io';
import 'dart:async';

import 'package:flutter/material.dart';
import 'package:permission_handler/permission_handler.dart';
import 'package:ext_storage/ext_storage.dart';
import 'package:open_file/open_file.dart';
import 'package:dio/dio.dart';
import 'package:webview_flutter/webview_flutter.dart'; 
```

# 4. Use the URL wich have 'filename' as param, extract the param using this function:

URL Example: http://ipv4.download.thinkbroadband.com/10MB.zip?filename=myfilezippaded.zip

```dart
String getFileNameURL(String url) {
   Uri uri = Uri.dataFromString(url);
   return uri.queryParameters['filename'] ?? "";
}
```

# 5. Create _downloadFile function. Check full example in main.dart file.






