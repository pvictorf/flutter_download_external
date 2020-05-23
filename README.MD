# 1. For this exemple I using Android. 
* You need configure your AndroidManifest. Set user permission:
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

# 4. Use the URL which have 'filename' as param, extract the param using this function:

URL Example: http://ipv4.download.thinkbroadband.com/10MB.zip?filename=myfilezippaded.zip

```dart
String getFileNameURL(String url) {
   Uri uri = Uri.dataFromString(url);
   return uri.queryParameters['filename'] ?? "";
}
```

# 5. Create _downloadFile and open file function.
```dart
Future<void> _downloadFile(String fileUrl, String filename) async {
   try {
      Dio dio = Dio();

      //Requesting Storage permission
      PermissionStatus permissionStatus = await Permission.storage.request();


      //Getting /Download folder of device
      String dir = await ExtStorage.getExternalStoragePublicDirectory(ExtStorage.DIRECTORY_DOWNLOADS);


      // Create a file reference with the name sent by the server
      final String path = "${dir}/${filename}";


      // Awaits Download Progress
      Response response = await dio.get(fileUrl, onReceiveProgress: (rec, total) {
        setState(() {
          downloading = true;
          progressString = ((rec / total) * 100).toStringAsFixed(0) + "%";

          print(progressString);
        });
      }, options: Options(responseType: ResponseType.bytes, followRedirects: false,receiveTimeout: 0));


      // Download finished
      setState(() {
        downloading = false;
        progressString = "";
      });

      // Writing the file on real device
      File file = new File(path);
      await file.writeAsBytes(response.data);


      //Openning the file
      await openFile(path);

   } catch (e) {
      print(e);
   }
}

   Future<void> openFile(String filePath) async {
      final result = await OpenFile.open(filePath);
   }

   String getFileNameURL(String url) {
      Uri uri = Uri.dataFromString(url);
      return uri.queryParameters['filename'] ?? "";
   }
```

# 6 Executing
```dart
String fileUrl  = "http://ipv4.download.thinkbroadband.com/10MB.zip?filename=myfilezippaded.zip";
String filename = getFileNameURL(fileUrl);

_downloadFile(fileUrl, filename);
```




